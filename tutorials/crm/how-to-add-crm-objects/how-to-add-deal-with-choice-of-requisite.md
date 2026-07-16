# Add a Deal and Company with Requisites

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant uses the official REST documentation.

{% endnote %}

Using a web form, you can automatically add new deals and companies with requisites to Bitrix24. When a client fills out the form, the data is sent to a handler. The handler script creates objects in the CRM via the API.

The setup consists of two stages.

1. Prepare the fields and place the web form on the page.

2. Create a handler file that sequentially calls the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md), [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md), [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md), and [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) methods.

## 1. Create the Web Form

To generate the fields, we use two methods:

- [crm.address.fields](../../../api-reference/crm/requisites/addresses/crm-address-fields.md) — retrieves a list of address fields. Save the result in the `$arAddressFields` array.

- [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) — retrieves a list of requisite templates based on the `ID` and `NAME` fields. Save the result in an array `$arRequisiteType`.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    const arAddressFields = (await $b24.actions.v2.call.make({
        method: 'crm.address.fields', params: {}, requestId: 'address-fields'
    })).getData().result
    const arRequisiteType = (await $b24.actions.v2.call.make({
        method: 'crm.requisite.preset.list', params: { select: ['ID', 'NAME'] }, requestId: 'preset-list'
    })).getData().result
    ```

- PHP

    ```php
    $arAddressFields = $sb->getCRMScope()->address()->fields()->getFieldsDescription();
    $arRequisiteType = $sb->getCRMScope()->requisitePreset()->list(
        order: [], filter: [], select: ["ID", "NAME"]
    )->getRequisitePresets();
    ```

- Python

    ```python
    ar_address_fields = client.crm.address.fields().result
    ar_requisite_type = client.crm.requisite.preset.list(select=["ID", "NAME"]).result
    ```

{% endlist %}

Remove unnecessary address fields from the `$arAddressFields` array so they are not displayed in the form.

{% list tabs %}

- JS

    ```javascript
    for (const f of ['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID']) {
        delete arAddressFields[f]
    }
    ```

- PHP

    ```php
    foreach (['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID'] as $field) {
        unset($arAddressFields[$field]);
    }
    ```

- Python

    ```python
    for f in ("TYPE_ID", "ENTITY_TYPE_ID", "ENTITY_ID", "COUNTRY_CODE", "ANCHOR_TYPE_ID", "ANCHOR_ID"):
        ar_address_fields.pop(f, None)
    ```

{% endlist %}

Create an HTML form with the following fields:

- `REQ_TYPE` — a drop-down list with requisite templates from the `$arRequisiteType` array. Mandatory field.

- `TITLE` — company name. Mandatory field.

- `INN` — company Taxpayer ID.

- `PHONE` — phone number.

- `ADDRESS` — address fields are created dynamically from `$arAddressFields`. If the field is mandatory, add the attribute `required`.

The form collects the data and sends it to the handler using the `POST` method. The form markup is shown below (the requisite drop-down list and address fields are populated from the retrieved data).

{% list tabs %}

- JS

    ```javascript
    // assemble the form string from the received data and insert it into the server response
    const options = presets.map(p => `<option value="${p.ID}">${p.NAME}</option>`).join('')
    const addressInputs = Object.entries(arAddressFields).map(([key, field]) =>
        `<input type="text" name="ADDRESS[${key}]" placeholder="${field.title}" ${field.isRequired ? 'required' : ''}>`
    ).join('')

    const formHtml = `
        <form id="form_to_crm">
            <select name="REQ_TYPE" required>
                <option value="" disabled selected>Select</option>
                ${options}
            </select>
            <input type="text" name="TITLE" placeholder="Org name" required>
            <input type="text" name="INN" placeholder="INN">
            <input type="text" name="PHONE" placeholder="Phone">
            ${addressInputs}
            <input type="submit" value="Submit">
        </form>`
    ```

- PHP

    ```html
    <form id="form_to_crm">
        <select name="REQ_TYPE" required>
            <option value="" disabled selected>Select</option>
            <?php foreach($arRequisiteType as $id=>$name):?>
                <option value="<?=$id?>"><?=$name?></option>
            <?php endforeach;?>
        </select>
        <input type="text" name="TITLE" placeholder="Org name" required>
        <input type="text" name="INN" placeholder="INN">
        <input type="text" name="PHONE" placeholder="Phone">
        <?php if(is_array($arAddressFields)):?>
            <?php foreach($arAddressFields as $key=>$arField):?>
                <input type="text" name="ADDRESS[<?=$key?>]" placeholder="<?=$arField['title']?>" <?=($arField['isRequired'])?'required':'';?>>
            <?php endforeach;?>
        <?php endif;?>
        <input type="submit" value="Submit">
    </form>
    ```

- Python

    ```python
    # assemble the form string from the received data and insert it into the server response
    from markupsafe import escape

    options = "".join(
        f'<option value="{escape(preset_id)}">{escape(name)}</option>'
        for preset_id, name in requisite_types.items()
    )
    address_inputs = "".join(
        f'<input type="text" name="ADDRESS[{escape(key)}]" '
        f'placeholder="{escape(field["title"])}" '
        f'{"required" if field["isRequired"] else ""}>'
        for key, field in address_fields.items()
    )

    form_html = f"""
        <form id="form_to_crm">
            <select name="REQ_TYPE" required>
                <option value="" disabled selected>Select</option>
                {options}
            </select>
            <input type="text" name="TITLE" placeholder="Org name" required>
            <input type="text" name="INN" placeholder="INN">
            <input type="text" name="PHONE" placeholder="Phone">
            {address_inputs}
            <input type="submit" value="Submit">
        </form>"""
    ```

{% endlist %}

### Full Code Example

{% list tabs %}

- JS

    ```javascript
    import express from 'express'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const app = express()

    // Form page: get data from Bitrix24 and render HTML
    app.get('/', async (req, res) => {
        const arAddressFields = (await $b24.actions.v2.call.make({
            method: 'crm.address.fields', params: {}, requestId: 'address-fields'
        })).getData().result
        const presets = (await $b24.actions.v2.call.make({
            method: 'crm.requisite.preset.list', params: { select: ['ID', 'NAME'] }, requestId: 'preset-list'
        })).getData().result

        if (!presets.length) {
            res.send('No requisite types.')
            return
        }

        // Remove system and unused address fields
        for (const f of ['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID']) {
            delete arAddressFields[f]
        }

        const options = presets.map(p => `<option value="${p.ID}">${p.NAME}</option>`).join('')
        const addressInputs = Object.entries(arAddressFields).map(([key, field]) =>
            `<input type="text" name="ADDRESS[${key}]" placeholder="${field.title}" ${field.isRequired ? 'required' : ''}>`
        ).join('')

        res.send(`
            <form id="form_to_crm">
                <select name="REQ_TYPE" required>
                    <option value="" disabled selected>Select</option>
                    ${options}
                </select>
                <input type="text" name="TITLE" placeholder="Org name" required>
                <input type="text" name="INN" placeholder="INN">
                <input type="text" name="PHONE" placeholder="Phone">
                ${addressInputs}
                <input type="submit" value="Submit">
            </form>
            <script>
                document.getElementById('form_to_crm').addEventListener('submit', async (el) => {
                    el.preventDefault()
                    const formData = Object.fromEntries(new FormData(el.currentTarget).entries())
                    const response = await fetch('/form', {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(formData),
                    })
                    alert((await response.json()).message)
                })
            <\/script>
        `)
    })

    app.listen(3000)
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $arAddressFields = $sb->getCRMScope()->address()->fields()->getFieldsDescription();

    $arPresets = $sb->getCRMScope()->requisitePreset()->list(
        order: [], filter: [], select: ["ID", "NAME"]
    )->getRequisitePresets();
    if(!empty($arPresets)):
        $arRequisiteType = [];
        foreach ($arPresets as $preset) {
            $arRequisiteType[$preset->ID] = $preset->NAME;
        }
        //unset system address fields
        unset($arAddressFields['TYPE_ID']);
        unset($arAddressFields['ENTITY_TYPE_ID']);
        unset($arAddressFields['ENTITY_ID']);
        //unset uninteresting address fields
        unset($arAddressFields['COUNTRY_CODE']);
        unset($arAddressFields['ANCHOR_TYPE_ID']);
        unset($arAddressFields['ANCHOR_ID']);
        ?>
        <form id="form_to_crm">
            <select name="REQ_TYPE" required>
                <option value="" disabled selected>Select</option>
                <?php foreach($arRequisiteType as $id=>$name):?>
                    <option value="<?=$id?>"><?=$name?></option>
                <?php endforeach;?>
            </select>
            <input type="text" name="TITLE" placeholder="Org name" required>
            <input type="text" name="INN" placeholder="INN">
            <input type="text" name="PHONE" placeholder="Phone">
            <?php if(is_array($arAddressFields)):?>
                <?php foreach($arAddressFields as $key=>$arField):?>
                    <input type="text" name="ADDRESS[<?=$key?>]" placeholder="<?=$arField['title']?>" <?=($arField['isRequired'])?'required':'';?>>
                <?php endforeach;?>
            <?php endif;?>
            <input type="submit" value="Submit">
        </form>
    <?php else:?>
        No requisite types.
    <?php endif;?>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
    $(document).ready(function() {
        $('#form_to_crm').on( 'submit', function(el) {//event submit form
            el.preventDefault();//the default action of the event will not be triggered
            var formData = $(this).serialize();
            $.ajax({
                'method': 'POST',
                'dataType': 'json',
                'url': 'form.php', // file for saving completed forms
                'data': formData,
                success: function(data){//success callback
                    alert(data.message);
                }
            });
        });
    });
    </script>
    ```

- Python

    ```python
    # pip install b24pysdk flask
    from flask import Flask
    from markupsafe import escape
    from b24pysdk import BitrixWebhook, Client

    app = Flask(__name__)

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",  # user_id/token only, without https://
    ))

    # regular string without substitutions: JS curly braces do not need to be escaped
    SCRIPT = """
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
        <script>
        $(document).ready(function() {
            $('#form_to_crm').on('submit', function(el) {
                el.preventDefault();
                $.ajax({
                    'method': 'POST', 'dataType': 'json', 'url': '/form',
                    'data': $(this).serialize(),
                    success: function(data){ alert(data.message); }
                });
            });
        });
        </script>
    """

    @app.route("/")
    def form_page():
        address_fields = client.crm.address.fields().result
        presets = client.crm.requisite.preset.list(select=["ID", "NAME"]).result

        requisite_types = {p["ID"]: p["NAME"] for p in presets}
        if not requisite_types:
            return "No requisite types."

        # unset system + uninteresting address fields
        for f in ("TYPE_ID", "ENTITY_TYPE_ID", "ENTITY_ID", "COUNTRY_CODE", "ANCHOR_TYPE_ID", "ANCHOR_ID"):
            address_fields.pop(f, None)

        # assemble the form string from the received data
        options = "".join(
            f'<option value="{escape(preset_id)}">{escape(name)}</option>'
            for preset_id, name in requisite_types.items()
        )
        address_inputs = "".join(
            f'<input type="text" name="ADDRESS[{escape(key)}]" '
            f'placeholder="{escape(field["title"])}" '
            f'{"required" if field["isRequired"] else ""}>'
            for key, field in address_fields.items()
        )

        return f"""
            <form id="form_to_crm">
                <select name="REQ_TYPE" required>
                    <option value="" disabled selected>Select</option>
                    {options}
                </select>
                <input type="text" name="TITLE" placeholder="Org name" required>
                <input type="text" name="INN" placeholder="INN">
                <input type="text" name="PHONE" placeholder="Phone">
                {address_inputs}
                <input type="submit" value="Submit">
            </form>""" + SCRIPT
    ```

{% endlist %}

## 2. Create a Form Handler

Create a file that will process the data and save it to the CRM.

### Retrieve Data

Retrieve and process the data from the form.

{% list tabs %}

- JS

    ```javascript
    const iRequisitePresetID = parseInt(req.body.REQ_TYPE, 10)
    const sTitle = String(req.body.TITLE ?? '')
    const sINN = String(req.body.INN ?? '')
    const sPhone = String(req.body.PHONE ?? '')
    const arAddress = {}
    for (const [key, val] of Object.entries(req.body.ADDRESS ?? {})) {
        arAddress[key] = String(val)
    }
    ```

- PHP

    ```php
    $iRequisitePresetID = intval($_POST["REQ_TYPE"]); 
    $sTitle = htmlspecialchars($_POST["TITLE"]); 
    $sINN = htmlspecialchars($_POST["INN"]); 
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    $arAddress = [];
    foreach ($_POST["ADDRESS"] as $key => $val) {
     $arAddress[$key] = htmlspecialchars($val); 
    }
    ```

- Python

    ```python
    i_requisite_preset_id = int(request.form.get("REQ_TYPE", 0))
    s_title = request.form.get("TITLE", "")
    s_inn = request.form.get("INN", "")
    s_phone = request.form.get("PHONE", "")
    ar_address = {k[len("ADDRESS["):-1]: v for k, v in request.form.to_dict().items()
                  if k.startswith("ADDRESS[")}
    ```

{% endlist %}

- `$iRequisitePresetID` — convert the requisite template identifier `REQ_TYPE` to an integer.

- `$sTitle`, `$sINN`, `$sPhone` — safely process the data from `TITLE`, `INN`, `PHONE` to prevent XSS attacks.

- `$arAddress` — save the data from the array containing address fields `ADDRESS`.

### Prepare Data

Add two mandatory system fields to the `$arAddress` array.

- `TYPE_ID` — address type. We will specify `1` — street address. You can retrieve the list of address types using the [crm.enum.addresstype](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md) method.

- `ENTITY_TYPE_ID` — [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type). We pass `8` — Company details. You can retrieve the full list of object types using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method.

{% list tabs %}

- JS

    ```javascript
    arAddress.TYPE_ID = 1
    arAddress.ENTITY_TYPE_ID = 8
    ```

- PHP

    ```php
    $arAddress['TYPE_ID'] = 1;
    $arAddress['ENTITY_TYPE_ID'] = 8;
    ```

- Python

    ```python
    ar_address["TYPE_ID"] = 1
    ar_address["ENTITY_TYPE_ID"] = 8
    ```

{% endlist %}

The system stores the phone number as a [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) array of objects, so the `$sPhone` value must be converted to an array format:

- in the first item `VALUE`, we write `$sPhone`,

- in the second item `VALUE_TYPE`, we pass, for example, `WORK`.

If the `$sPhone` variable has no value, specify an empty array.

{% list tabs %}

- JS

    ```javascript
    const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []
    ```

- PHP

    ```php
    $arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
    ```

- Python

    ```python
    ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []
    ```

{% endlist %}

### Add a Company

To add a company, use the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) method. You must pass the following data to it:

- `TITLE` — company name. We pass `$sTitle`, which was retrieved from the form.

- `COMPANY_TYPE` — company type. We will specify `CUSTOMER` — customer. You can retrieve the list of types using the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method with the `'filter'=>['ENTITY_ID'=>’COMPANY_TYPE']` filter.

- `PHONE` — an array containing the phone number `$arPhone` retrieved from the form.

{% note warning "" %}

Check which mandatory fields are configured for companies in your Bitrix24. All mandatory fields must be passed to the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) method.

{% endnote %}

{% list tabs %}

- JS

    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.company.add',
        params: { fields: { TITLE: sTitle, COMPANY_TYPE: 'CUSTOMER', PHONE: arPhone } },
        requestId: 'company-add'
    })
    const result = result.getData()?.result
    ```

- PHP

    ```php
    $result = $sb->getCRMScope()->company()->add([
        'TITLE' => $sTitle,
        'COMPANY_TYPE' => 'CUSTOMER',
        'PHONE' => $arPhone,
    ])->getId();
    ```

- Python

    ```python
    result = client.crm.company.add(fields={
        "TITLE": s_title,
        "COMPANY_TYPE": "CUSTOMER",
        "PHONE": ar_phone,
    }).result
    ```

{% endlist %}

If the company is successfully created, the method will return its identifier in `$result`. If you received an error `error`, review the description of possible errors in the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) method documentation.

### Add Requisites

To add Company details, use the [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md) method. You must pass the following data to it:

- `ENTITY_TYPE_ID` — [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type). We pass `4` — company. You can retrieve the full list of object types using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method.

- `ENTITY_ID` — company identifier. We pass `$result`, which was obtained during company creation.

- `PRESET_ID` — requisite template identifier. We specify `$iRequisitePresetID`, which was retrieved from the form.

- `NAME` — requisite name. We pass `$sTitle`, which was retrieved from the form.

- `RQ_INN` — company Taxpayer ID. We pass `$sINN`, which was retrieved from the form.

- `ACTIVE` — an activity flag; we will specify `Y`.

{% list tabs %}

- JS

    ```javascript
    const resultRequisite = await $b24.actions.v2.call.make({
        method: 'crm.requisite.add',
        params: {
            fields: {
                ENTITY_TYPE_ID: 4,
                ENTITY_ID: result,
                PRESET_ID: iRequisitePresetID,
                ACTIVE: 'Y',
                NAME: sTitle,
                RQ_INN: sINN,
            }
        },
        requestId: 'requisite-add'
    })
    const resultRequisite = resultRequisite.getData()?.result
    ```

- PHP

    ```php
    $resultRequisite = $sb->getCRMScope()->requisite()->add(
        entityId: $result,
        entityTypeId: 4,
        requisitePresetId: $iRequisitePresetID,
        requisiteName: $sTitle,
        fields: ['ACTIVE' => 'Y', 'RQ_INN' => $sINN]
    )->getId();
    ```

- Python

    ```python
    result_requisite = client.crm.requisite.add(fields={
        "ENTITY_TYPE_ID": 4,
        "ENTITY_ID": result,
        "PRESET_ID": i_requisite_preset_id,
        "ACTIVE": "Y",
        "NAME": s_title,
        "RQ_INN": s_inn,
    }).result
    ```

{% endlist %}

If the Company details are successfully added, the method will return the record identifier in `$resultRequisite`. If you received an error `error`, review the possible error descriptions in the [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md) method documentation.

### Add Address to Requisites

1. Add the `ENTITY_ID` field — the Company details identifier — to the `$arAddress` array. Pass the `$resultRequisite` obtained during the creation of the Company details.

   {% list tabs %}

   - JS

       ```javascript
       arAddress.ENTITY_ID = resultRequisite
       ```

   - PHP

       ```php
       $arAddress['ENTITY_ID'] = $resultRequisite;
       ```

   - Python

       ```python
       ar_address["ENTITY_ID"] = result_requisite
       ```

   {% endlist %}

2. Use the [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) method. You must pass the `$arAddress` array to it.

   {% list tabs %}

   - JS

       ```javascript
       const resultAddress = (await $b24.actions.v2.call.make({
           method: 'crm.address.add', params: { fields: arAddress }, requestId: 'address-add'
       })).getData().result
       ```

   - PHP

       ```php
       $resultAddress = $sb->getCRMScope()->address()->add($arAddress)->isSuccess();
       ```

   - Python

       ```python
       result_address = client.crm.address.add(fields=ar_address).result
       ```

   {% endlist %}

The method returns one of the following values in the `$resultAddress` variable:

- `true` — the address was added,

- `false` — the address was not added.

### **Adding a Deal**

Create a `$arDealFields` array with the deal data.

- `TITLE` — the deal title. We will specify the `$sTitle` company name obtained from the form,

- `COMPANY_ID` — the identifier of the company linked to the deal. Pass the `$result` obtained during the creation of the company,

- `REQUISITE_ID` — the Company details identifier. If the Company details were created, pass `$resultRequisite`.

{% list tabs %}

- JS

    ```javascript
    const arDealFields = { TITLE: sTitle, COMPANY_ID: result }
    if (resultRequisite) {
        arDealFields.REQUISITE_ID = resultRequisite
    }
    ```

- PHP

    ```php
    $arDealFields = [
        'TITLE' => $sTitle,
        'COMPANY_ID' => $result
    ];
    if (!empty($resultRequisite)) {
        $arDealFields['REQUISITE_ID'] = $resultRequisite;
    }
    ```

- Python

    ```python
    ar_deal_fields = {"TITLE": s_title, "COMPANY_ID": result}
    if result_requisite:
        ar_deal_fields["REQUISITE_ID"] = result_requisite
    ```

{% endlist %}

To add a deal, use the [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) method. Pass the `$arDealFields` array to it.

{% list tabs %}

- JS

    ```javascript
    const resultDeal = await $b24.actions.v2.call.make({
        method: 'crm.deal.add', params: { fields: arDealFields }, requestId: 'deal-add'
    })
    ```

- PHP

    ```php
    $dealId = $sb->getCRMScope()->deal()->add($arDealFields)->getId();
    ```

- Python

    ```python
    deal_id = client.crm.deal.add(fields=ar_deal_fields).result
    ```

{% endlist %}

If the deal is successfully created, the method will return its identifier. If you received an error `error`, review the possible error descriptions in the [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) method documentation.

```json
{
    "result": 1789,
}
```

### Full Handler Code Example

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    export async function handler(req, res) {
        const iRequisitePresetID = parseInt(req.body.REQ_TYPE, 10)
        const sTitle = String(req.body.TITLE ?? '')
        const sINN = String(req.body.INN ?? '')
        const sPhone = String(req.body.PHONE ?? '')

        const arAddress = {}
        for (const [key, val] of Object.entries(req.body.ADDRESS ?? {})) {
            arAddress[key] = String(val)
        }
        arAddress.TYPE_ID = 1 // 1 — actual address (crm.enum.addresstype)
        arAddress.ENTITY_TYPE_ID = 8 // 8 — attribute (crm.enum.ownertype)

        const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []

        try {
            const result = await $b24.actions.v2.call.make({
                method: 'crm.company.add',
                params: { fields: { TITLE: sTitle, COMPANY_TYPE: 'CUSTOMER', PHONE: arPhone } },
                requestId: 'company-add'
            })
            const result = result.getData()?.result
            if (!result) {
                res.json({ message: 'not added: ' + result.getErrorMessages().join('; ') })
                return
            }

            const resultRequisite = await $b24.actions.v2.call.make({
                method: 'crm.requisite.add',
                params: {
                    fields: {
                        ENTITY_TYPE_ID: 4, // 4 — company (crm.enum.ownertype)
                        ENTITY_ID: result,
                        PRESET_ID: iRequisitePresetID,
                        ACTIVE: 'Y',
                        NAME: sTitle,
                        RQ_INN: sINN,
                    }
                },
                requestId: 'requisite-add'
            })

            const arDealFields = { TITLE: sTitle, COMPANY_ID: result }
            const resultRequisite = resultRequisite.getData()?.result
            if (resultRequisite) {
                arDealFields.REQUISITE_ID = resultRequisite
                arAddress.ENTITY_ID = resultRequisite
                await $b24.actions.v2.call.make({
                    method: 'crm.address.add', params: { fields: arAddress }, requestId: 'address-add'
                })
            }

            await $b24.actions.v2.call.make({
                method: 'crm.deal.add', params: { fields: arDealFields }, requestId: 'deal-add'
            })

            res.json({ message: 'add' })
        } catch (e) {
            res.json({ message: 'not added: ' + e.message })
        }
    }
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');
    $crm = $sb->getCRMScope();

    $iRequisitePresetID = intVal($_POST["REQ_TYPE"]);
    $sTitle = htmlspecialchars($_POST["TITLE"]);
    $sINN = htmlspecialchars($_POST["INN"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    $arAddress = [];

    foreach($_POST["ADDRESS"] as $key=>$val){
        $arAddress[$key] = htmlspecialchars($val);
    }
    $arAddress['TYPE_ID'] = 1; // 1 — actual address (crm.enum.addresstype)
    $arAddress['ENTITY_TYPE_ID'] = 8; // 8 — attribute (crm.enum.ownertype)

    $arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();

    try {
        $result = $crm->company()->add([
            'TITLE' => $sTitle,
            'COMPANY_TYPE' => 'CUSTOMER', // client (crm.status.list ENTITY_ID=COMPANY_TYPE)
            'PHONE' => $arPhone,
        ])->getId();

        $resultRequisite = $crm->requisite()->add(
            entityId: $result,
            entityTypeId: 4, // 4 — company (crm.enum.ownertype)
            requisitePresetId: $iRequisitePresetID,
            requisiteName: $sTitle,
            fields: ['ACTIVE' => 'Y', 'RQ_INN' => $sINN]
        )->getId();

        $arDealFields = [
            'TITLE' => $sTitle,
            'COMPANY_ID' => $result
        ];
        if (!empty($resultRequisite)) {
            $arDealFields['REQUISITE_ID'] = $resultRequisite;
            $arAddress['ENTITY_ID'] = $resultRequisite;
            $crm->address()->add($arAddress);
        }

        $crm->deal()->add($arDealFields);
        echo json_encode(['message' => 'add']);
    } catch (\Throwable $e) {
        echo json_encode(['message' => 'not added: ' . $e->getMessage()]);
    }
    ```

- Python

    ```python
    # pip install b24pysdk
    from flask import Flask, request, jsonify
    from b24pysdk import BitrixWebhook, Client

    app = Flask(__name__)

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",  # user_id/token only, without https://
    ))

    @app.route("/form.php", methods=["POST"])
    def handle_form():
        i_requisite_preset_id = int(request.form.get("REQ_TYPE", 0))
        s_title = request.form.get("TITLE", "")
        s_inn = request.form.get("INN", "")
        s_phone = request.form.get("PHONE", "")

        ar_address = {k[len("ADDRESS["):-1]: v for k, v in request.form.to_dict().items()
                      if k.startswith("ADDRESS[")}
        ar_address["TYPE_ID"] = 1  # 1 — actual address (crm.enum.addresstype)
        ar_address["ENTITY_TYPE_ID"] = 8  # 8 — attribute (crm.enum.ownertype)

        ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []

        try:
            result = client.crm.company.add(fields={
                "TITLE": s_title,
                "COMPANY_TYPE": "CUSTOMER",  # client (crm.status.list ENTITY_ID=COMPANY_TYPE)
                "PHONE": ar_phone,
            }).result

            result_requisite = client.crm.requisite.add(fields={
                "ENTITY_TYPE_ID": 4,  # 4 — company (crm.enum.ownertype)
                "ENTITY_ID": result,
                "PRESET_ID": i_requisite_preset_id,
                "ACTIVE": "Y",
                "NAME": s_title,
                "RQ_INN": s_inn,
            }).result

            ar_deal_fields = {"TITLE": s_title, "COMPANY_ID": result}
            if result_requisite:
                ar_deal_fields["REQUISITE_ID"] = result_requisite
                ar_address["ENTITY_ID"] = result_requisite
                client.crm.address.add(fields=ar_address)

            client.crm.deal.add(fields=ar_deal_fields)
            return jsonify({"message": "add"})
        except Exception as e:
            return jsonify({"message": f"not added: {e}"})
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/companies/crm-company-add.md)
- [{#T}](../../../api-reference/crm/requisites/universal/crm-requisite-add.md)
- [{#T}](../../../api-reference/crm/requisites/addresses/crm-address-add.md)
- [{#T}](../../../api-reference/crm/deals/crm-deal-add.md)
- [{#T}](../../../api-reference/crm/requisites/addresses/crm-address-fields.md)
- [{#T}](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md)
- [{#T}](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md)
- [{#T}](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md)
