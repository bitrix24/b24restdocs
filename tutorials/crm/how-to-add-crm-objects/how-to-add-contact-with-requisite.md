# Add a Contact with Details via Web Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to create contacts in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant uses the official REST documentation.

{% endnote %}

You can place a form on your website to collect client data and details. When a client fills out the form, their data will be sent to the CRM, and you will be able to process the request.

Setting up the form consists of two steps.

1. Place the form on a PHP page. In the page code, retrieve a list of company detail templates and address fields for the form. Send the form data to a handler.

2. Create a file to process the data. The handler will receive and prepare the data, then create a contact with company details.

## 1. Creating the Web Form

To generate the form fields, we will use data from Bitrix24. To obtain information about the detail settings, we will sequentially execute two methods:

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

2. [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) — requests a list of company detail templates. Use the `select` parameter to select the `ID` and `NAME` fields for each template. Save the result in `arRequisiteType`.

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

-  `REQ_TYPE` — a drop-down list with the company detail type from the `arRequisiteType` array, required,

-  `NAME` — contact name, required,

-  `LAST_NAME` — surname,

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

    // Form page: we receive data from Bitrix24 and render HTML
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

        // Assemble the billing dropdown list and address fields
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
                <input type="text" name="NAME" placeholder="First Name" required>
                <input type="text" name="LAST_NAME" placeholder="Last Name">
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

    // Get the list of address fields and billing templates
    $arAddressFields = $sb->getCRMScope()->address()->fields()->getFieldsDescription();
    $arPresets = $sb->getCRMScope()->requisitePreset()->list(
        order: [], filter: [], select: ["ID", "NAME"]
    )->getRequisitePresets();

    if (!empty($arPresets)):
        $arRequisiteType = [];
        foreach ($arPresets as $preset) {
            $arRequisiteType[$preset->ID] = $preset->NAME;
        }

        // Remove system and unused address fields
        $excludeFields = ['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID'];
        foreach ($excludeFields as $field) {
            unset($arAddressFields[$field]);
        }
    ?>
        <form id="form_to_crm">
            <select name="REQ_TYPE" required>
                <option value="" disabled selected>Select billing type</option>
                <?php foreach ($arRequisiteType as $id => $name): ?>
                    <option value="<?=$id?>"><?=$name?></option>
                <?php endforeach; ?>
            </select>
            <input type="text" name="NAME" placeholder="First Name" required>
            <input type="text" name="LAST_NAME" placeholder="Last Name">
            <input type="text" name="PHONE" placeholder="Phone">
            <?php foreach ($arAddressFields as $key => $arField): ?>
                <input type="text" name="ADDRESS[<?=$key?>]" 
                       placeholder="<?=$arField['title']?>" 
                       <?=$arField['isRequired'] ? 'required' : ''?>>
            <?php endforeach; ?>
            <input type="submit" value="Submit">
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

    # Page template: %(options)s and %(address_inputs)s are substituted from Python
    PAGE = """
        <form id="form_to_crm">
            <select name="REQ_TYPE" required>
                <option value="" disabled selected>Select billing type</option>
                %(options)s
            </select>
            <input type="text" name="NAME" placeholder="First Name" required>
            <input type="text" name="LAST_NAME" placeholder="Last Name">
            <input type="text" name="PHONE" placeholder="Phone">
            %(address_inputs)s
            <input type="submit" value="Submit">
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

        # Assemble the billing dropdown list and address fields
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

To process values from form fields and add a contact to the CRM, we will create a handler `form.php`.

### Prepare the Data

Retrieve and sanitize the data from the form:

- Convert `REQ_TYPE` to a number,

- Strip HTML tags from `NAME`, `LAST_NAME`, and `PHONE`.

{% list tabs %}

- JS

    ```javascript
    const iRequisitePresetID = parseInt(req.body.REQ_TYPE, 10)
    const sName = String(req.body.NAME ?? '')
    const sLastName = String(req.body.LAST_NAME ?? '')
    const sPhone = String(req.body.PHONE ?? '')
    ```

- PHP

    ```php
    $iRequisitePresetID = intVal($_POST["REQ_TYPE"]);
    $sName = htmlspecialchars($_POST["NAME"]);
    $sLastName = htmlspecialchars($_POST["LAST_NAME"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    ```

- Python

    ```python
    i_requisite_preset_id = int(request.form.get("REQ_TYPE", 0))
    s_name = request.form.get("NAME", "")
    s_last_name = request.form.get("LAST_NAME", "")
    s_phone = request.form.get("PHONE", "")
    ```

{% endlist %}

Prepare the address fields and collect them into the `$arAddress` array.

- Strip HTML tags from the form field values.

- Add the address type `TYPE_ID`. You can get address types using the [crm.enum.addresstype](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md) method. We will specify the value — `1`, which is the street address.

- Add the [object type](../../../api-reference/crm/data-types.md#object_type) identifier `ENTITY_TYPE_ID`. You can get identifiers using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method. We will specify the value — `8`, which is the company details.

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

The system stores the phone as a [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) array of objects, so it must be converted to an array format.

1. Add the phone as the first item `VALUE` in the array, and specify the type `VALUE_TYPE` as the second value, for example, `WORK`.

2. Pass an empty array for an empty value.

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

### Add a Contact

To create a contact, call the [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) method. In the `fields` object, pass the following fields:

- `NAME` — the contact name,

- `LAST_NAME` — the surname,

- `PHONE` — the phone.

{% note warning "" %}

Check which mandatory fields are configured for contacts in your Bitrix24. All mandatory fields must be passed to the [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) method.

{% endnote %}

{% list tabs %}

- JS

    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.contact.add',
        params: { fields: { NAME: sName, LAST_NAME: sLastName, PHONE: arPhone } },
        requestId: 'contact-add'
    })
    const contactId = result.getData()?.result
    ```

- PHP

    ```php
    $contactId = $sb->getCRMScope()->contact()->add([
        'NAME' => $sName,
        'LAST_NAME' => $sLastName,
        'PHONE' => $arPhone
    ])->getId();
    ```

- Python

    ```python
    contact_id = client.crm.contact.add(fields={
        "NAME": s_name,
        "LAST_NAME": s_last_name,
        "PHONE": ar_phone,
    }).result
    ```

{% endlist %}

As a result, you will receive the identifier of the new contact, for example, `23`.

```json
{
	"result": 23
}
```

### Add Company Details to a Contact

To add company details to a contact, call the [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md) method. In the `fields` object, pass the following fields:

- `ENTITY_TYPE_ID` — the [object type](../../../api-reference/crm/data-types.md#object_type) identifier. You can retrieve identifiers using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method. In this example, we will specify the value `3`, which is the contact,

- `ENTITY_ID` — the contact identifier received in the previous request,

- `PRESET_ID` — the company details template identifier received from the form,

- `ACTIVE` — the company details activity `Y`,

- `NAME` — the company details name, for example, by combining the contact's first and last name,

{% list tabs %}

- JS

    ```javascript
    await $b24.actions.v2.call.make({
        method: 'crm.requisite.add',
        params: {
            fields: {
                ENTITY_TYPE_ID: 3,
                ENTITY_ID: contactId,
                PRESET_ID: iRequisitePresetID,
                ACTIVE: 'Y',
                NAME: [sName, sLastName].join(' '),
            }
        },
        requestId: 'requisite-add'
    })
    ```

- PHP

    ```php
    $sb->getCRMScope()->requisite()->add(
        entityId: $contactId,
        entityTypeId: 3,
        requisitePresetId: $iRequisitePresetID,
        requisiteName: implode(' ', [$sName, $sLastName]),
        fields: ['ACTIVE' => 'Y']
    );
    ```

- Python

    ```python
    client.crm.requisite.add(fields={
        "ENTITY_TYPE_ID": 3,
        "ENTITY_ID": contact_id,
        "PRESET_ID": i_requisite_preset_id,
        "ACTIVE": "Y",
        "NAME": " ".join([s_name, s_last_name]),
    })
    ```

{% endlist %}

As a result, you will receive the company details identifier.

```php
{
    "result": 34
}
```

### Add an Address for Company Details

Add an address for the company details using the [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) method if the company details were created successfully. In `$arAddress`, add `ENTITY_ID` with the `ID` of the company details from the previous request's response. In the `fields` object, pass the `$arAddress` array containing the address fields.

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
        const sName = String(req.body.NAME ?? '')
        const sLastName = String(req.body.LAST_NAME ?? '')
        const sPhone = String(req.body.PHONE ?? '')

        // Prepare the address
        const arAddress = {}
        for (const [key, val] of Object.entries(req.body.ADDRESS ?? {})) {
            arAddress[key] = String(val)
        }
        arAddress.TYPE_ID = 1 // Physical address
        arAddress.ENTITY_TYPE_ID = 8 // Object type — billing detail

        // Format phone for Bitrix24
        const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []

        // Create contact
        const result = await $b24.actions.v2.call.make({
            method: 'crm.contact.add',
            params: { fields: { NAME: sName, LAST_NAME: sLastName, PHONE: arPhone } },
            requestId: 'contact-add'
        })

        const contactId = result.getData()?.result
        if (contactId) {
            // Add billing details for the new contact
            const resultRequisite = await $b24.actions.v2.call.make({
                method: 'crm.requisite.add',
                params: {
                    fields: {
                        ENTITY_TYPE_ID: 3, // Object type — contact
                        ENTITY_ID: contactId,
                        PRESET_ID: iRequisitePresetID,
                        ACTIVE: 'Y',
                        NAME: [sName, sLastName].join(' '),
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

            res.json({ message: 'Contact added successfully' })
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

    // Get and clean form data
    $iRequisitePresetID = intVal($_POST["REQ_TYPE"]);
    $sName = htmlspecialchars($_POST["NAME"]);
    $sLastName = htmlspecialchars($_POST["LAST_NAME"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);

    // Prepare the address
    $arAddress = [];
    foreach ($_POST["ADDRESS"] as $key => $val) {
        $arAddress[$key] = htmlspecialchars($val);
    }
    $arAddress['TYPE_ID'] = 1; // Physical address
    $arAddress['ENTITY_TYPE_ID'] = 8; // Object type — billing detail

    // Format phone for Bitrix24
    $arPhone = !empty($sPhone) ? [['VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK']] : [];

    // Create contact
    try {
        $contactId = $sb->getCRMScope()->contact()->add([
            'NAME' => $sName,
            'LAST_NAME' => $sLastName,
            'PHONE' => $arPhone
        ])->getId();

        // Add billing details for the new contact
        $requisiteId = $sb->getCRMScope()->requisite()->add(
            entityId: $contactId,
            entityTypeId: 3, // Object type — contact
            requisitePresetId: $iRequisitePresetID,
            requisiteName: implode(' ', [$sName, $sLastName]),
            fields: ['ACTIVE' => 'Y']
        )->getId();

        // Add address if billing details were created successfully
        if (!empty($requisiteId)) {
            $arAddress['ENTITY_ID'] = $requisiteId;
            $sb->getCRMScope()->address()->add($arAddress);
        }

        echo json_encode(['message' => 'Contact added successfully']);
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
        s_name = request.form.get("NAME", "")
        s_last_name = request.form.get("LAST_NAME", "")
        s_phone = request.form.get("PHONE", "")

        # Prepare the address
        ar_address = {key: val for key, val in request.form.to_dict().items()
                      if key.startswith("ADDRESS[")}
        ar_address = {k[len("ADDRESS["):-1]: v for k, v in ar_address.items()}
        ar_address["TYPE_ID"] = 1  # Physical address
        ar_address["ENTITY_TYPE_ID"] = 8  # Object type — billing detail

        # Format phone for Bitrix24
        ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []

        # Create contact
        try:
            contact_id = client.crm.contact.add(fields={
                "NAME": s_name,
                "LAST_NAME": s_last_name,
                "PHONE": ar_phone,
            }).result

            # Add billing details for the new contact
            requisite_id = client.crm.requisite.add(fields={
                "ENTITY_TYPE_ID": 3,  # Object type — contact
                "ENTITY_ID": contact_id,
                "PRESET_ID": i_requisite_preset_id,
                "ACTIVE": "Y",
                "NAME": " ".join([s_name, s_last_name]),
            }).result

            # Add address if billing details were created successfully
            if requisite_id:
                ar_address["ENTITY_ID"] = requisite_id
                client.crm.address.add(fields=ar_address)

            return jsonify({"message": "Contact added successfully"})
        except Exception as e:
            return jsonify({"message": f"Error: {e}"})
    ```

{% endlist %}
