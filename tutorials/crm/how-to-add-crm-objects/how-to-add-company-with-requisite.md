# Add a Company with Details via Web Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to create companies in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can place a form on the website to collect client data and details. When a client fills out the form, their information will be sent to the CRM, allowing you to process the request.

Setting up the form consists of two steps.

1. Place the form on a PHP page. In the page code, retrieve a list of company details templates and address fields for the form. Send the form data to a handler.

2. Create a file to process the data. The handler will receive and prepare the data, then create a company with company details.

## 1. Creating the Web Form

To generate the form fields, use data from Bitrix24. To retrieve information about the company details settings, call the following two methods in sequence:

1. [crm.address.fields](../../../api-reference/crm/requisites/addresses/crm-address-fields.md) — retrieves a list of address fields. Save the result in `arAddressFields`,

   {% list tabs %}

   - JS

       ```javascript
       const arAddressFields = await $b24.actions.v2.call.make({
           method: 'crm.address.fields', params: {}, requestId: 'address-fields'
       })
       ```

   - PHP

       ```php
       $arAddressFields = $sb->getCRMScope()->address()->fields()->getFieldsDescription();
       ```

   - Python

       ```python
       ar_address_fields = client.crm.address.fields().result
       ```

   {% endlist %}

2. [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) — requests a list of company details templates. Using the `select` parameter, select the `ID` and `NAME` fields for each template. Save the result in `arRequisiteType`.

   {% list tabs %}

   - JS

       ```javascript
       const arRequisiteType = await $b24.actions.v2.call.make({
           method: 'crm.requisite.preset.list',
           params: { select: ['ID', 'NAME'] },
           requestId: 'preset-list'
       })
       ```

   - PHP

       ```php
       $arRequisiteType = $sb->getCRMScope()->requisitePreset()->list(
           order: [], filter: [], select: ['ID', 'NAME']
       )->getRequisitePresets();
       ```

   - Python

       ```python
       ar_requisite_type = client.crm.requisite.preset.list(select=["ID", "NAME"]).result
       ```

   {% endlist %}

Add a web form to the website page with the following fields:

-  `REQ_TYPE` — a drop-down list with the type of company details from the `arRequisiteType` array,
  
-  `TITLE` — company name, required,

-  `INN` — Taxpayer ID,

-  `PHONE` — phone,

-  `${addressFieldsInputs}` — address fields, which are created dynamically from the `arAddressFields` array.

The form sends data to the handler using the `POST` method.

### Full Page Code Example with Form

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import express from 'express'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const app = express()

    // Form page: we get data from Bitrix24 and render HTML
    app.get('/', async (req, res) => {
        // Get the list of address fields and billing templates
        const arAddressFields = (await $b24.actions.v2.call.make({
            method: 'crm.address.fields', params: {}, requestId: 'address-fields'
        })).getData().result
        const presets = (await $b24.actions.v2.call.make({
            method: 'crm.requisite.preset.list', params: { select: ['ID', 'NAME'] }, requestId: 'preset-list'
        })).getData().result

        if (!presets.length) {
            res.send('<p>No available billing types.</p>')
            return
        }

        // Remove system and unused address fields
        for (const f of ['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID']) {
            delete arAddressFields[f]
        }

        // Assemble the dropdown list of billing details and address fields
        const options = presets.map(p => `<option value="${p.ID}">${p.NAME}</option>`).join('')
        const addressInputs = Object.entries(arAddressFields).map(([key, field]) =>
            `<input type="text" name="ADDRESS[${key}]" placeholder="${field.title}" ${field.isRequired ? 'required' : ''}>`
        ).join('')

        res.send(`
            <form id="form_to_crm">
                <select name="REQ_TYPE" required>
                    <option value="" disabled selected>Select billing type</option>
                    ${options}
                </select>
                <input type="text" name="TITLE" placeholder="Organization name" required>
                <input type="text" name="INN" placeholder="Tax ID">
                <input type="text" name="PHONE" placeholder="Phone">
                ${addressInputs}
                <input type="submit" value="Send">
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

    // Get the list of address fields and billing templates
    $arAddressFields = $sb->getCRMScope()->address()->fields()->getFieldsDescription();
    $arPresets = $sb->getCRMScope()->requisitePreset()->list(
        order: [], filter: [], select: ["ID", "NAME"]
    )->getRequisitePresets();

    if(!empty($arPresets)):
        $arRequisiteType = [];
        foreach ($arPresets as $preset) {
            $arRequisiteType[$preset->ID] = $preset->NAME;
        }
        // Remove system and unused address fields
        $excludeFields = ['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID'];
        foreach($excludeFields as $field) {
            unset($arAddressFields[$field]);
        }
    ?>
        <form id="form_to_crm">
            <select name="REQ_TYPE" required>
                <option value="" disabled selected>Select billing type</option>
                <?php foreach($arRequisiteType as $id => $name): ?>
                    <option value="<?=$id?>"><?=$name?></option>
                <?php endforeach; ?>
            </select>
            <input type="text" name="TITLE" placeholder="Organization name" required>
            <input type="text" name="INN" placeholder="Tax ID">
            <input type="text" name="PHONE" placeholder="Phone">
            <?php foreach($arAddressFields as $key => $arField): ?>
                <input type="text" name="ADDRESS[<?=$key?>]" 
                       placeholder="<?=$arField['title']?>" 
                       <?=$arField['isRequired'] ? 'required' : ''?>>
            <?php endforeach; ?>
            <input type="submit" value="Send">
        </form>
    <?php else: ?>
        <p>No available billing types.</p>
    <?php endif; ?>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
    $(document).ready(function() {
        $('#form_to_crm').on('submit', function(el) {
            el.preventDefault();
            $.ajax({
                method: 'POST',
                dataType: 'json',
                url: 'form.php',
                data: $(this).serialize(),
                success: function(data) {
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

    # Page template: substitute %(options)s and %(address_inputs)s from Python
    PAGE = """
        <form id="form_to_crm">
            <select name="REQ_TYPE" required>
                <option value="" disabled selected>Select billing type</option>
                %(options)s
            </select>
            <input type="text" name="TITLE" placeholder="Organization name" required>
            <input type="text" name="INN" placeholder="Tax ID">
            <input type="text" name="PHONE" placeholder="Phone">
            %(address_inputs)s
            <input type="submit" value="Send">
        </form>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
        <script>
        $(document).ready(function() {
            $('#form_to_crm').on('submit', function(el) {
                el.preventDefault();
                $.ajax({
                    method: 'POST', dataType: 'json', url: '/form',
                    data: $(this).serialize(),
                    success: function(data) { alert(data.message); }
                });
            });
        });
        </script>
    """

    EMPTY_PAGE = "<p>No available billing types.</p>"

    @app.route("/")
    def form_page():
        # Get the list of address fields and billing templates
        address_fields = client.crm.address.fields().result
        presets = client.crm.requisite.preset.list(select=["ID", "NAME"]).result

        requisite_types = {p["ID"]: p["NAME"] for p in presets}
        if not requisite_types:
            return EMPTY_PAGE

        # Remove system and unused address fields
        for f in ("TYPE_ID", "ENTITY_TYPE_ID", "ENTITY_ID", "COUNTRY_CODE", "ANCHOR_TYPE_ID", "ANCHOR_ID"):
            address_fields.pop(f, None)

        # Assemble the dropdown list of billing details and address fields
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

        return PAGE % {"options": options, "address_inputs": address_inputs}
    ```

{% endlist %}

## 2. Create a Form Handler

To process values from the form fields and add a company to the CRM, we will create a handler `form.php`.

### Prepare the Data

Retrieve and sanitize the data from the form:

- Convert `REQ_TYPE` to a number,

- Strip HTML tags from `TITLE`, `INN`, and `PHONE`.

{% list tabs %}

- JS

    ```javascript
    const iRequisitePresetID = parseInt(req.body.REQ_TYPE, 10)
    const sTitle = String(req.body.TITLE ?? '')
    const sINN = String(req.body.INN ?? '')
    const sPhone = String(req.body.PHONE ?? '')
    ```

- PHP

    ```php
    $iRequisitePresetID = intVal($_POST["REQ_TYPE"]);
    $sTitle = htmlspecialchars($_POST["TITLE"]);
    $sINN = htmlspecialchars($_POST["INN"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    ```

- Python

    ```python
    i_requisite_preset_id = int(request.form.get("REQ_TYPE", 0))
    s_title = request.form.get("TITLE", "")
    s_inn = request.form.get("INN", "")
    s_phone = request.form.get("PHONE", "")
    ```

{% endlist %}

Prepare the address fields and collect them into the `$arAddress` array.

- Strip HTML tags from the form field values.

- Add the address type `TYPE_ID`. Address types can be obtained using the [crm.enum.addresstype](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md) method. We will specify the value — `1`, which is the street address.

- Add the [object type](../../../api-reference/crm/data-types.md#object_type) identifier `ENTITY_TYPE_ID`. Identifiers can be obtained using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method. We will specify the value — `8`, which is the company details.

{% list tabs %}

- JS

    ```javascript
    const arAddress = {}
    for (const [key, val] of Object.entries(req.body.ADDRESS ?? {})) {
        arAddress[key] = String(val)
    }
    arAddress.TYPE_ID = 1
    arAddress.ENTITY_TYPE_ID = 8
    ```

- PHP

    ```php
    $arAddress = [];
    foreach($_POST["ADDRESS"] as $key => $val) {
        $arAddress[$key] = htmlspecialchars($val);
    }
    $arAddress['TYPE_ID'] = 1;
    $arAddress['ENTITY_TYPE_ID'] = 8;
    ```

- Python

    ```python
    ar_address = {k[len("ADDRESS["):-1]: v for k, v in request.form.to_dict().items()
                  if k.startswith("ADDRESS[")}
    ar_address["TYPE_ID"] = 1
    ar_address["ENTITY_TYPE_ID"] = 8
    ```

{% endlist %}

The system stores the phone number as a [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) array of objects, so it must be converted to an array format.

1. Add the phone number as the first item `VALUE` in the array, and specify the type `VALUE_TYPE` as the second value, for example, `WORK`.

2. For an empty value, pass an empty array.

{% list tabs %}

- JS

    ```javascript
    const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []
    ```

- PHP

    ```php
    $arPhone = !empty($sPhone) ? [['VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK']] : [];
    ```

- Python

    ```python
    ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []
    ```

{% endlist %}

### Add a Company

To create a company, call the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) method. Pass the following fields in the `fields` object:

- `TITLE` — the company name.

- `COMPANY_TYPE` — the company type. You can retrieve company types using the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method by using the `filter` filter on the `ENTITY_ID` field with the value `COMPANY_TYPE`. In the example, we will specify the value `CUSTOMER`, as only company customers fill out the form.

- `PHONE` — the phone number.

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
    const companyId = result.getData()?.result
    ```

- PHP

    ```php
    $companyId = $sb->getCRMScope()->company()->add([
        'TITLE' => $sTitle,
        'COMPANY_TYPE' => 'CUSTOMER', // Company type — client
        'PHONE' => $arPhone
    ])->getId();
    ```

- Python

    ```python
    company_id = client.crm.company.add(fields={
        "TITLE": s_title,
        "COMPANY_TYPE": "CUSTOMER",  # Company type — client
        "PHONE": ar_phone,
    }).result
    ```

{% endlist %}

As a result, you will receive the new company identifier `5`.

```json
{
	"result": 5
}
```

### Adding Details to the Company

To add company details to a company, call the [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md) method. Pass the following fields in the `fields` object:

- `ENTITY_TYPE_ID` — the [object type](../../../api-reference/crm/data-types.md#object_type) identifier. Identifiers can be obtained using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method. In the example, we will specify the value `4`, which is the company,

- `ENTITY_ID` — the company identifier received in the previous request,

- `PRESET_ID` — the company details template identifier received from the form,

- `ACTIVE` — the activity status of the company details `Y`,

- `NAME` — the name of the company details,

- `RQ_INN` — the company Taxpayer ID.

{% list tabs %}

- JS

    ```javascript
    await $b24.actions.v2.call.make({
        method: 'crm.requisite.add',
        params: {
            fields: {
                ENTITY_TYPE_ID: 4,
                ENTITY_ID: companyId,
                PRESET_ID: iRequisitePresetID,
                ACTIVE: 'Y',
                NAME: sTitle,
                RQ_INN: sINN,
            }
        },
        requestId: 'requisite-add'
    })
    ```

- PHP

    ```php
    $sb->getCRMScope()->requisite()->add(
        entityId: $companyId,
        entityTypeId: 4,
        requisitePresetId: $iRequisitePresetID,
        requisiteName: $sTitle,
        fields: ['ACTIVE' => 'Y', 'RQ_INN' => $sINN]
    );
    ```

- Python

    ```python
    client.crm.requisite.add(fields={
        "ENTITY_TYPE_ID": 4,
        "ENTITY_ID": company_id,
        "PRESET_ID": i_requisite_preset_id,
        "ACTIVE": "Y",
        "NAME": s_title,
        "RQ_INN": s_inn,
    })
    ```

{% endlist %}

As a result, you will receive the company details identifier.

```php
{
    "result": 27
}
```

### Add an Address for the Company Details

Add an address for the company details using the [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) method if the company details were created successfully. In `$arAddress`, add `ENTITY_ID` with the `ID` of the company details from the previous request's response. Pass the `$arAddress` array containing the address fields in the `fields` object.

{% list tabs %}

- JS

    ```javascript
    if (requisiteId) {
        arAddress.ENTITY_ID = requisiteId
        await $b24.actions.v2.call.make({
            method: 'crm.address.add',
            params: { fields: arAddress },
            requestId: 'address-add'
        })
    }
    ```

- PHP

    ```php
    if (!empty($requisiteId)) {
        $arAddress['ENTITY_ID'] = $requisiteId;
        $sb->getCRMScope()->address()->add($arAddress);
    }
    ```

- Python

    ```python
    if requisite_id:
        ar_address["ENTITY_ID"] = requisite_id
        client.crm.address.add(fields=ar_address)
    ```

{% endlist %}

### Full Handler Code Example

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    export async function handler(req, res) {
        // Get and clean form data
        const iRequisitePresetID = parseInt(req.body.REQ_TYPE, 10)
        const sTitle = String(req.body.TITLE ?? '')
        const sINN = String(req.body.INN ?? '')
        const sPhone = String(req.body.PHONE ?? '')

        // Prepare address
        const arAddress = {}
        for (const [key, val] of Object.entries(req.body.ADDRESS ?? {})) {
            arAddress[key] = String(val)
        }
        arAddress.TYPE_ID = 1 // Value 1 — actual address
        arAddress.ENTITY_TYPE_ID = 8 // Object type ID — 8, i.e., billing detail

        // Format phone for Bitrix24 into crm_multifield format
        const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []

        // Create company
        const result = await $b24.actions.v2.call.make({
            method: 'crm.company.add',
            params: { fields: { TITLE: sTitle, COMPANY_TYPE: 'CUSTOMER', PHONE: arPhone } },
            requestId: 'company-add'
        })

        const companyId = result.getData()?.result
        if (companyId) {
            // Add billing details for the new company
            const resultRequisite = await $b24.actions.v2.call.make({
                method: 'crm.requisite.add',
                params: {
                    fields: {
                        ENTITY_TYPE_ID: 4, // Object type ID — 4, i.e., company
                        ENTITY_ID: companyId,
                        PRESET_ID: iRequisitePresetID,
                        ACTIVE: 'Y',
                        NAME: sTitle,
                        RQ_INN: sINN,
                    }
                },
                requestId: 'requisite-add'
            })

            // Add address if billing details were created successfully
            const requisiteId = resultRequisite.getData()?.result
            if (requisiteId) {
                arAddress.ENTITY_ID = requisiteId
                await $b24.actions.v2.call.make({
                    method: 'crm.address.add',
                    params: { fields: arAddress },
                    requestId: 'address-add'
                })
            }

            res.json({ message: 'Company added successfully' })
        } else {
            res.json({ message: 'Error: ' + result.getErrorMessages().join('; ') })
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

    // Get and clean form data
    $iRequisitePresetID = intVal($_POST["REQ_TYPE"]);
    $sTitle = htmlspecialchars($_POST["TITLE"]);
    $sINN = htmlspecialchars($_POST["INN"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);

    // Prepare address
    $arAddress = [];
    foreach($_POST["ADDRESS"] as $key => $val) {
        $arAddress[$key] = htmlspecialchars($val);
    }
    $arAddress['TYPE_ID'] = 1; // Value 1 — actual address
    $arAddress['ENTITY_TYPE_ID'] = 8; // Object type ID — 8, i.e., billing detail

    // Format phone for Bitrix24 into crm_multifield format
    $arPhone = !empty($sPhone) ? [['VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK']] : [];

    // Create company
    try {
        $companyId = $crm->company()->add([
            'TITLE' => $sTitle,
            'COMPANY_TYPE' => 'CUSTOMER', // Type — client
            'PHONE' => $arPhone
        ])->getId();

        // Add billing details for the new company
        $requisiteId = $crm->requisite()->add(
            entityId: $companyId,
            entityTypeId: 4, // Object type ID — 4, i.e., company
            requisitePresetId: $iRequisitePresetID,
            requisiteName: $sTitle,
            fields: ['ACTIVE' => 'Y', 'RQ_INN' => $sINN]
        )->getId();

        // Add address if billing details were created successfully
        if (!empty($requisiteId)) {
            $arAddress['ENTITY_ID'] = $requisiteId;
            $crm->address()->add($arAddress);
        }

        echo json_encode(['message' => 'Company added successfully']);
    } catch (\Throwable $e) {
        echo json_encode(['message' => 'Error: ' . $e->getMessage()]);
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
        # Get and clean form data
        i_requisite_preset_id = int(request.form.get("REQ_TYPE", 0))
        s_title = request.form.get("TITLE", "")
        s_inn = request.form.get("INN", "")
        s_phone = request.form.get("PHONE", "")

        # Prepare address
        ar_address = {k[len("ADDRESS["):-1]: v for k, v in request.form.to_dict().items()
                      if k.startswith("ADDRESS[")}
        ar_address["TYPE_ID"] = 1  # Value 1 — actual address
        ar_address["ENTITY_TYPE_ID"] = 8  # Object type ID — 8, i.e., billing detail

        # Format phone for Bitrix24 into crm_multifield format
        ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []

        # Create company
        try:
            company_id = client.crm.company.add(fields={
                "TITLE": s_title,
                "COMPANY_TYPE": "CUSTOMER",  # Type — client
                "PHONE": ar_phone,
            }).result

            # Add billing details for the new company
            requisite_id = client.crm.requisite.add(fields={
                "ENTITY_TYPE_ID": 4,  # Object type ID — 4, i.e., company
                "ENTITY_ID": company_id,
                "PRESET_ID": i_requisite_preset_id,
                "ACTIVE": "Y",
                "NAME": s_title,
                "RQ_INN": s_inn,
            }).result

            # Add address if billing details were created successfully
            if requisite_id:
                ar_address["ENTITY_ID"] = requisite_id
                client.crm.address.add(fields=ar_address)

            return jsonify({"message": "Company added successfully"})
        except Exception as e:
            return jsonify({"message": f"Error: {e}"})
    ```

{% endlist %}
