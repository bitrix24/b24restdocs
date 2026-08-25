# Add a Deal and Company with Requisites

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, two permissions are required at once — "Add|Import" for companies and "add" for deals
>
> - [crm.address.fields](../../../api-reference/crm/requisites/addresses/crm-address-fields.md) — any user
> - [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) — a user with permission to read contacts and companies
> - [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) — a user with the "Add|Import" permission for companies
> - [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md) and [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) — a user with permission to add the company that owns the requisite
> - [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) — a user with the "add" permission for deals

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant uses the official REST documentation.

{% endnote %}

Using a web form, you can automatically add new deals and companies with requisites to Bitrix24. When a client fills out the form, the data is sent to a handler. The handler script creates objects in the CRM via the API.

As a result of the scenario, four linked objects appear in the CRM: a company, its requisite, the requisite address, and a deal linked to the company.

The setup consists of two stages.

1. Prepare the fields and place the web form on the page. The set of form fields is taken from the [crm.address.fields](../../../api-reference/crm/requisites/addresses/crm-address-fields.md) and [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) methods

2. Create a handler file that sequentially calls the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md), [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md), [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md), and [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) methods

The order of the calls is set by the links between the objects: the requisite is created for an existing company, the address for an existing requisite, and the deal is linked to the company.

## Before You Start

- At least one requisite template is configured in Bitrix24. If there are no templates, the [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) method returns an empty list and there is nothing to build the form from

- The webhook is created on behalf of a user who has permissions to add companies and deals

- You have a server that serves the page with the form and accepts the form data using the `POST` method. In the examples, this is Express for JS, a PHP script, and Flask for Python

- The webhook URL is stored in the environment, not in the page code. The page with the form is public, and the secret must not end up in it

## 1. Create the Web Form

To generate the fields, we use two methods:

- [crm.address.fields](../../../api-reference/crm/requisites/addresses/crm-address-fields.md) — retrieves a list of address fields. Save the result in the `$arAddressFields` array

- [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) — retrieves a list of requisite templates based on the `ID` and `NAME` fields. Save the result in the `$arPresets` array

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    const arAddressFields = (await $b24.actions.v2.call.make({
        method: 'crm.address.fields', params: {}, requestId: 'address-fields'
    })).getData().result
    const arPresets = (await $b24.actions.v2.call.make({
        method: 'crm.requisite.preset.list', params: { select: ['ID', 'NAME'] }, requestId: 'preset-list'
    })).getData().result
    ```

- Python

    ```python
    ar_address_fields = client.crm.address.fields().result
    ar_presets = client.crm.requisite.preset.list(select=["ID", "NAME"]).result
    ```


- PHP

    ```php
    $arAddressFields = $sb->getCRMScope()->address()->fields()->getFieldsDescription();
    $arPresets = $sb->getCRMScope()->requisitePreset()->list(
        order: [], filter: [], select: ["ID", "NAME"]
    )->getRequisitePresets();
    ```
{% endlist %}

The [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) method returns an array of objects, not identifier-name pairs. For the drop-down list, iterate over this array and take `ID` and `NAME` from each object.

```json
{
    "result": [
        { "ID": "1", "NAME": "Organization" },
        { "ID": "3", "NAME": "Sole Proprietorship" },
        { "ID": "5", "NAME": "Individual" }
    ]
}
```

The [crm.address.fields](../../../api-reference/crm/requisites/addresses/crm-address-fields.md) method returns an object where the key is the field code and the value is its description with the `isRequired` mandatory flag and the `title` name.

```json
{
    "result": {
        "TYPE_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "TYPE_ID"
        },
        "ADDRESS_1": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Street, house, building, structure"
        },
        "CITY": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "City"
        }
    }
}
```

Remove unnecessary address fields from the `$arAddressFields` array so they are not displayed in the form. Three of them — `TYPE_ID`, `ENTITY_TYPE_ID`, and `ENTITY_ID` — are mandatory system fields that the client does not fill in; the handler substitutes them itself.

{% list tabs %}

- JS

    ```javascript
    for (const f of ['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID']) {
        delete arAddressFields[f]
    }
    ```

- Python

    ```python
    for f in ("TYPE_ID", "ENTITY_TYPE_ID", "ENTITY_ID", "COUNTRY_CODE", "ANCHOR_TYPE_ID", "ANCHOR_ID"):
        ar_address_fields.pop(f, None)
    ```


- PHP

    ```php
    foreach (['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID'] as $field) {
        unset($arAddressFields[$field]);
    }
    ```
{% endlist %}

Create an HTML form with the following fields:

- `REQ_TYPE` — a drop-down list with requisite templates from the `$arPresets` array. Mandatory field

- `TITLE` — company name. Mandatory field

- `INN` — company Taxpayer ID

- `PHONE` — phone number

- `ADDRESS` — address fields are created dynamically from `$arAddressFields`. If the field is mandatory, add the attribute `required`

The form collects the data and sends it to the handler using the `POST` method. The form markup is shown below (the requisite drop-down list and address fields are populated from the retrieved data).

{% list tabs %}

- JS

    ```javascript
    // assemble the form string from the received data and insert it into the server response
    const options = arPresets.map(p => `<option value="${p.ID}">${p.NAME}</option>`).join('')
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

- Python

    ```python
    # assemble the form string from the received data and insert it into the server response
    from markupsafe import escape

    options = "".join(
        f'<option value="{escape(preset["ID"])}">{escape(preset["NAME"])}</option>'
        for preset in ar_presets
    )
    address_inputs = "".join(
        f'<input type="text" name="ADDRESS[{escape(key)}]" '
        f'placeholder="{escape(field["title"])}" '
        f'{"required" if field["isRequired"] else ""}>'
        for key, field in ar_address_fields.items()
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


- PHP

    ```html
    <form id="form_to_crm">
        <select name="REQ_TYPE" required>
            <option value="" disabled selected>Select</option>
            <?php foreach($arPresets as $preset):?>
                <option value="<?=$preset->ID?>"><?=$preset->NAME?></option>
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
        const arPresets = (await $b24.actions.v2.call.make({
            method: 'crm.requisite.preset.list', params: { select: ['ID', 'NAME'] }, requestId: 'preset-list'
        })).getData().result

        if (!arPresets.length) {
            res.send('No requisite types.')
            return
        }

        // Remove system and unused address fields
        for (const f of ['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID']) {
            delete arAddressFields[f]
        }

        const options = arPresets.map(p => `<option value="${p.ID}">${p.NAME}</option>`).join('')
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
        ar_address_fields = client.crm.address.fields().result
        ar_presets = client.crm.requisite.preset.list(select=["ID", "NAME"]).result

        if not ar_presets:
            return "No requisite types."

        # unset system + uninteresting address fields
        for f in ("TYPE_ID", "ENTITY_TYPE_ID", "ENTITY_ID", "COUNTRY_CODE", "ANCHOR_TYPE_ID", "ANCHOR_ID"):
            ar_address_fields.pop(f, None)

        # assemble the form string from the received data
        options = "".join(
            f'<option value="{escape(preset["ID"])}">{escape(preset["NAME"])}</option>'
            for preset in ar_presets
        )
        address_inputs = "".join(
            f'<input type="text" name="ADDRESS[{escape(key)}]" '
            f'placeholder="{escape(field["title"])}" '
            f'{"required" if field["isRequired"] else ""}>'
            for key, field in ar_address_fields.items()
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
                <?php foreach($arPresets as $preset):?>
                    <option value="<?=$preset->ID?>"><?=$preset->NAME?></option>
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

- Python

    ```python
    i_requisite_preset_id = int(request.form.get("REQ_TYPE", 0))
    s_title = request.form.get("TITLE", "")
    s_inn = request.form.get("INN", "")
    s_phone = request.form.get("PHONE", "")
    ar_address = {k[len("ADDRESS["):-1]: v for k, v in request.form.to_dict().items()
                  if k.startswith("ADDRESS[")}
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
{% endlist %}

- `$iRequisitePresetID` — convert the requisite template identifier `REQ_TYPE` to an integer

- `$sTitle`, `$sINN`, `$sPhone` — safely process the data from `TITLE`, `INN`, `PHONE` to prevent XSS attacks

- `$arAddress` — save the data from the array containing address fields `ADDRESS`

### Prepare Data

Add two mandatory system fields to the `$arAddress` array.

- `TYPE_ID` — address type. We will specify `1` — street address. You can retrieve the list of address types using the [crm.enum.addresstype](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md) method

- `ENTITY_TYPE_ID` — [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type). We pass `8` — Company details. You can retrieve the full list of object types using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method

The third mandatory field, `ENTITY_ID`, is substituted later: it is the requisite identifier, and the requisite does not exist yet.

{% list tabs %}

- JS

    ```javascript
    arAddress.TYPE_ID = 1
    arAddress.ENTITY_TYPE_ID = 8
    ```

- Python

    ```python
    ar_address["TYPE_ID"] = 1
    ar_address["ENTITY_TYPE_ID"] = 8
    ```


- PHP

    ```php
    $arAddress['TYPE_ID'] = 1;
    $arAddress['ENTITY_TYPE_ID'] = 8;
    ```
{% endlist %}

The system stores the phone number as a [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) array of objects, so the `$sPhone` value must be converted to an array format:

- in the first item `VALUE`, we write `$sPhone`

- in the second item `VALUE_TYPE`, we pass, for example, `WORK`

If the `$sPhone` variable has no value, specify an empty array.

{% list tabs %}

- JS

    ```javascript
    const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []
    ```

- Python

    ```python
    ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []
    ```


- PHP

    ```php
    $arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
    ```
{% endlist %}

### Add a Company

To add a company, use the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) method. You must pass the following data to it:

- `TITLE` — company name. We pass `$sTitle`, which was retrieved from the form

- `COMPANY_TYPE` — company type. We will specify `CUSTOMER` — customer. You can retrieve the list of types using the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method with the `'filter'=>['ENTITY_ID'=>'COMPANY_TYPE']` filter

- `PHONE` — an array containing the phone number `$arPhone` retrieved from the form

{% note warning "" %}

Check which mandatory fields are configured for companies in your Bitrix24. All mandatory fields must be passed to the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) method.

{% endnote %}

{% list tabs %}

- JS

    ```javascript
    const companyResponse = await $b24.actions.v2.call.make({
        method: 'crm.company.add',
        params: { fields: { TITLE: sTitle, COMPANY_TYPE: 'CUSTOMER', PHONE: arPhone } },
        requestId: 'company-add'
    })
    const iCompanyID = companyResponse.getData()?.result
    ```

- Python

    ```python
    i_company_id = client.crm.company.add(fields={
        "TITLE": s_title,
        "COMPANY_TYPE": "CUSTOMER",
        "PHONE": ar_phone,
    }).result
    ```


- PHP

    ```php
    $iCompanyID = $sb->getCRMScope()->company()->add([
        'TITLE' => $sTitle,
        'COMPANY_TYPE' => 'CUSTOMER',
        'PHONE' => $arPhone,
    ])->getId();
    ```
{% endlist %}

If the company is successfully created, the method returns its identifier in `$iCompanyID`. Retain the value: both the requisite and the deal need it.

```json
{
    "result": 2999
}
```

### Add Requisites

To add Company details, use the [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md) method. You must pass the following data to it:

- `ENTITY_TYPE_ID` — [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type). We pass `4` — company. You can retrieve the full list of object types using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method

- `ENTITY_ID` — company identifier. We pass `$iCompanyID`, which was obtained during company creation

- `PRESET_ID` — requisite template identifier. We specify `$iRequisitePresetID`, which was retrieved from the form

- `NAME` — requisite name. We pass `$sTitle`, which was retrieved from the form

- `RQ_INN` — company Taxpayer ID. We pass `$sINN`, which was retrieved from the form

- `ACTIVE` — an activity flag; we will specify `Y`

{% list tabs %}

- JS

    ```javascript
    const requisiteResponse = await $b24.actions.v2.call.make({
        method: 'crm.requisite.add',
        params: {
            fields: {
                ENTITY_TYPE_ID: 4,
                ENTITY_ID: iCompanyID,
                PRESET_ID: iRequisitePresetID,
                ACTIVE: 'Y',
                NAME: sTitle,
                RQ_INN: sINN,
            }
        },
        requestId: 'requisite-add'
    })
    const iRequisiteID = requisiteResponse.getData()?.result
    ```

- Python

    ```python
    i_requisite_id = client.crm.requisite.add(fields={
        "ENTITY_TYPE_ID": 4,
        "ENTITY_ID": i_company_id,
        "PRESET_ID": i_requisite_preset_id,
        "ACTIVE": "Y",
        "NAME": s_title,
        "RQ_INN": s_inn,
    }).result
    ```


- PHP

    ```php
    $iRequisiteID = $sb->getCRMScope()->requisite()->add(
        entityId: $iCompanyID,
        entityTypeId: 4,
        requisitePresetId: $iRequisitePresetID,
        requisiteName: $sTitle,
        fields: ['ACTIVE' => 'Y', 'RQ_INN' => $sINN]
    )->getId();
    ```
{% endlist %}

If the Company details are successfully added, the method returns the record identifier in `$iRequisiteID`.

```json
{
    "result": 409
}
```

{% note warning "" %}

The method does not check whether a template with the passed `PRESET_ID` exists. With a nonexistent identifier, the requisite is still created but remains without the template fields. Take `PRESET_ID` from the [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) response instead of substituting an arbitrary number.

{% endnote %}

### Add Address to Requisites

1. Add the `ENTITY_ID` field — the Company details identifier — to the `$arAddress` array. Pass the `$iRequisiteID` obtained during the creation of the Company details

   {% list tabs %}

   - JS

       ```javascript
       arAddress.ENTITY_ID = iRequisiteID
       ```

   - Python

       ```python
       ar_address["ENTITY_ID"] = i_requisite_id
       ```


   - PHP

       ```php
       $arAddress['ENTITY_ID'] = $iRequisiteID;
       ```
   {% endlist %}

2. Use the [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) method. You must pass the `$arAddress` array to it

   {% list tabs %}

   - JS

       ```javascript
       const bAddressAdded = (await $b24.actions.v2.call.make({
           method: 'crm.address.add', params: { fields: arAddress }, requestId: 'address-add'
       })).getData().result
       ```

   - Python

       ```python
       b_address_added = client.crm.address.add(fields=ar_address).result
       ```


   - PHP

       ```php
       $bAddressAdded = $sb->getCRMScope()->address()->add($arAddress)->isSuccess();
       ```
   {% endlist %}

The method returns one of the following values in the `$bAddressAdded` variable:

- `true` — the address was added

- `false` — the address was not added

```json
{
    "result": true
}
```

### Adding a Deal

Create a `$arDealFields` array with the deal data.

- `TITLE` — the deal title. We will specify the `$sTitle` company name obtained from the form

- `COMPANY_ID` — the identifier of the company linked to the deal. Pass the `$iCompanyID` obtained during the creation of the company

{% list tabs %}

- JS

    ```javascript
    const arDealFields = { TITLE: sTitle, COMPANY_ID: iCompanyID }
    ```

- Python

    ```python
    ar_deal_fields = {"TITLE": s_title, "COMPANY_ID": i_company_id}
    ```


- PHP

    ```php
    $arDealFields = [
        'TITLE' => $sTitle,
        'COMPANY_ID' => $iCompanyID
    ];
    ```
{% endlist %}

You do not need to pass the requisite to the deal separately. The deal takes the requisite from the linked company: Bitrix24 substitutes the client requisite automatically when a deal is created with `COMPANY_ID`.

{% note warning "" %}

A deal has no `REQUISITE_ID` field — it is absent from the [crm.deal.fields](../../../api-reference/crm/deals/crm-deal-fields.md) method response as well. If you pass `REQUISITE_ID` to [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md), the method does not return an error, but the value is ignored.

{% endnote %}

To add a deal, use the [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) method. Pass the `$arDealFields` array to it.

{% list tabs %}

- JS

    ```javascript
    const dealResponse = await $b24.actions.v2.call.make({
        method: 'crm.deal.add', params: { fields: arDealFields }, requestId: 'deal-add'
    })
    const iDealID = dealResponse.getData()?.result
    ```

- Python

    ```python
    i_deal_id = client.crm.deal.add(fields=ar_deal_fields).result
    ```


- PHP

    ```php
    $iDealID = $sb->getCRMScope()->deal()->add($arDealFields)->getId();
    ```
{% endlist %}

If the deal is successfully created, the method returns its identifier.

```json
{
    "result": 1789
}
```

## Verify the Result

Open the created deal in Bitrix24. The "Company" field is filled in on the card, and in the company, on the "Requisites" tab, the requisite with the Taxpayer ID and the address from the form is displayed.

Through REST, the link between the deal and the requisite is checked with the [crm.requisite.link.get](../../../api-reference/crm/requisites/links/crm-requisite-link-get.md) method with the following parameters:

- `entityTypeId` — `2`, a deal

- `entityId` — the identifier of the created deal

{% list tabs %}

- JS

    ```javascript
    const linkResponse = await $b24.actions.v2.call.make({
        method: 'crm.requisite.link.get',
        params: { entityTypeId: 2, entityId: iDealID },
        requestId: 'requisite-link-get'
    })

    console.dir(linkResponse.getData().result)
    ```

- Python

    ```python
    link = client.crm.requisite.link.get(
        entity_type_id=2,
        entity_id=i_deal_id,
    ).result
    ```


- PHP

    ```php
    // crm.requisite.link.get has no wrapper in the SDK — call the method directly
    $link = $sb->core->call(
        'crm.requisite.link.get',
        [
            'entityTypeId' => 2,
            'entityId' => $iDealID,
        ]
    )->getResponseData()->getResult();
    ```
{% endlist %}

The scenario is complete if `REQUISITE_ID` in the response matches the requisite identifier from the "Add Requisites" step.

```json
{
    "result": {
        "ENTITY_TYPE_ID": 2,
        "ENTITY_ID": 1789,
        "REQUISITE_ID": "409",
        "BANK_DETAIL_ID": "0",
        "MC_REQUISITE_ID": "0",
        "MC_BANK_DETAIL_ID": "0"
    }
}
```

## Errors and Diagnostics

If the method returns an error, check the request data. The requisite and address methods return errors with an empty code, so rely on the text in `error_description`.

#|
|| **Error text** | **Reason and action** ||
|| `Entity not found.` | An `ENTITY_ID` of a nonexistent company was passed to [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md). Take the identifier from the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) response ||
|| `ENTITY_TYPE_ID is not defined or invalid.` | The owner type was not passed or is incorrect. A company requisite requires `4`, a requisite address requires `8` ||
|| `ENTITY_ID is not defined or invalid.` | The owner identifier was not passed. In [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md), this is the requisite identifier, not the company identifier ||
|| `TYPE_ID is not defined or invalid.` | The address type was not passed to [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md). The list of values is returned by the [crm.enum.addresstype](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md) method ||
|| `TypeAddress exists.` | The requisite already has an address of this type. One requisite retains one address of each type — modify the existing one with the [crm.address.update](../../../api-reference/crm/requisites/addresses/crm-address-update.md) method or pass a different `TYPE_ID` ||
|| `Access denied.` | The user does not have permission to add a company or a deal. Check which user the webhook was created on behalf of ||
|#

The scenario creates four objects in a row, and an error at any step leaves the previous objects in the CRM. Repeat the step that failed rather than the whole handler:

- An error in [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) — nothing has been created in the CRM, you can repeat the entire handler

- An error in [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md) — the company has already been created. Running the handler again creates a duplicate of it, so pass the existing `ENTITY_ID`

- An error in [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) — the company and the requisite have already been created, but the deal has not. Add the address with a separate call and create the deal

If the deal was created but the requisite was not linked to it, the company has more than one requisite. Bitrix24 substitutes only one, and it is not necessarily the one the handler created.

## Key Considerations

- To link a specific requisite to the deal instead of the one Bitrix24 substituted on its own, call the [crm.requisite.link.register](../../../api-reference/crm/requisites/links/crm-requisite-link-register.md) method with `ENTITY_TYPE_ID`: `2`. The method requires all four link identifiers — `REQUISITE_ID`, `BANK_DETAIL_ID`, `MC_REQUISITE_ID`, and `MC_BANK_DETAIL_ID`. Pass zeros for the ones you do not need, otherwise the method returns an error such as `MC_REQUISITE_ID is not defined or invalid`

- One requisite retains one address of each type. The [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) method does not create a second address of the same type

- The [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) method returns `true` or `false`, not an identifier. An address has no identifier of its own: it is recognized by the `ENTITY_TYPE_ID` and `ENTITY_ID` pair plus `TYPE_ID`

- The set of requisite fields depends on the template. The individual template has no `RQ_INN` field: the value is retained and returned by [crm.requisite.get](../../../api-reference/crm/requisites/universal/crm-requisite-get.md), but it is not displayed on the requisite card. The set of template fields is returned by the [crm.requisite.preset.field.list](../../../api-reference/crm/requisites/presets/fields/crm-requisite-preset-field-list.md) method

- Submitting the form again with the same data creates a new company, a new requisite, and a new deal. Duplicates are not filtered out

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
            const companyResponse = await $b24.actions.v2.call.make({
                method: 'crm.company.add',
                params: { fields: { TITLE: sTitle, COMPANY_TYPE: 'CUSTOMER', PHONE: arPhone } },
                requestId: 'company-add'
            })
            const iCompanyID = companyResponse.getData()?.result
            if (!iCompanyID) {
                res.json({ message: 'not added: ' + companyResponse.getErrorMessages().join('; ') })
                return
            }

            const requisiteResponse = await $b24.actions.v2.call.make({
                method: 'crm.requisite.add',
                params: {
                    fields: {
                        ENTITY_TYPE_ID: 4, // 4 — company (crm.enum.ownertype)
                        ENTITY_ID: iCompanyID,
                        PRESET_ID: iRequisitePresetID,
                        ACTIVE: 'Y',
                        NAME: sTitle,
                        RQ_INN: sINN,
                    }
                },
                requestId: 'requisite-add'
            })
            const iRequisiteID = requisiteResponse.getData()?.result

            if (iRequisiteID) {
                arAddress.ENTITY_ID = iRequisiteID
                await $b24.actions.v2.call.make({
                    method: 'crm.address.add', params: { fields: arAddress }, requestId: 'address-add'
                })
            }

            // We do not pass the requisite to the deal: a deal has no REQUISITE_ID field,
            // Bitrix24 substitutes the requisite of the linked company itself
            await $b24.actions.v2.call.make({
                method: 'crm.deal.add',
                params: { fields: { TITLE: sTitle, COMPANY_ID: iCompanyID } },
                requestId: 'deal-add'
            })

            res.json({ message: 'add' })
        } catch (e) {
            res.json({ message: 'not added: ' + e.message })
        }
    }
    ```

- Python

    ```python
    # pip install b24pysdk flask
    from flask import Flask, request, jsonify
    from b24pysdk import BitrixWebhook, Client

    app = Flask(__name__)

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",  # user_id/token only, without https://
    ))

    @app.route("/form", methods=["POST"])
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
            i_company_id = client.crm.company.add(fields={
                "TITLE": s_title,
                "COMPANY_TYPE": "CUSTOMER",  # client (crm.status.list ENTITY_ID=COMPANY_TYPE)
                "PHONE": ar_phone,
            }).result

            i_requisite_id = client.crm.requisite.add(fields={
                "ENTITY_TYPE_ID": 4,  # 4 — company (crm.enum.ownertype)
                "ENTITY_ID": i_company_id,
                "PRESET_ID": i_requisite_preset_id,
                "ACTIVE": "Y",
                "NAME": s_title,
                "RQ_INN": s_inn,
            }).result

            if i_requisite_id:
                ar_address["ENTITY_ID"] = i_requisite_id
                client.crm.address.add(fields=ar_address)

            # We do not pass the requisite to the deal: a deal has no REQUISITE_ID field,
            # Bitrix24 substitutes the requisite of the linked company itself
            client.crm.deal.add(fields={
                "TITLE": s_title,
                "COMPANY_ID": i_company_id,
            })
            return jsonify({"message": "add"})
        except Exception as e:
            return jsonify({"message": f"not added: {e}"})
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
        $iCompanyID = $crm->company()->add([
            'TITLE' => $sTitle,
            'COMPANY_TYPE' => 'CUSTOMER', // client (crm.status.list ENTITY_ID=COMPANY_TYPE)
            'PHONE' => $arPhone,
        ])->getId();

        $iRequisiteID = $crm->requisite()->add(
            entityId: $iCompanyID,
            entityTypeId: 4, // 4 — company (crm.enum.ownertype)
            requisitePresetId: $iRequisitePresetID,
            requisiteName: $sTitle,
            fields: ['ACTIVE' => 'Y', 'RQ_INN' => $sINN]
        )->getId();

        if (!empty($iRequisiteID)) {
            $arAddress['ENTITY_ID'] = $iRequisiteID;
            $crm->address()->add($arAddress);
        }

        // We do not pass the requisite to the deal: a deal has no REQUISITE_ID field,
        // Bitrix24 substitutes the requisite of the linked company itself
        $crm->deal()->add([
            'TITLE' => $sTitle,
            'COMPANY_ID' => $iCompanyID
        ]);
        echo json_encode(['message' => 'add']);
    } catch (\Throwable $e) {
        echo json_encode(['message' => 'not added: ' . $e->getMessage()]);
    }
    ```
{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/companies/crm-company-add.md)
- [{#T}](../../../api-reference/crm/requisites/universal/crm-requisite-add.md)
- [{#T}](../../../api-reference/crm/requisites/addresses/crm-address-add.md)
- [{#T}](../../../api-reference/crm/deals/crm-deal-add.md)
- [{#T}](../../../api-reference/crm/requisites/addresses/crm-address-fields.md)
- [{#T}](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md)
- [{#T}](../../../api-reference/crm/requisites/links/crm-requisite-link-register.md)
- [{#T}](../../../api-reference/crm/requisites/links/crm-requisite-link-get.md)
- [{#T}](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md)
- [{#T}](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md)
