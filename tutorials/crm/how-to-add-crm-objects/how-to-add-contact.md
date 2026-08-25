# Add Contact via Web Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, both permissions are required — to add contacts and to read contacts
>
> - [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) — a user with permission to add contacts
> - [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) — a user with permission to read contacts

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can place a form on your website to collect client data. When a client fills out the form, a handler on your server creates a contact in the CRM and returns its identifier. As a result, you get two files — a page with the form and a handler built on the stack of your choice — plus a separate script to verify the result.

The scenario consists of two steps.

1. Place the form on an HTML page. The form sends the data to the handler

2. Create the handler. It validates the data and creates a contact using the universal [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method

After that, we verify the result with the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method using the identifier from the second step response: this call only confirms the creation and does not change any data in the CRM.

## Prepare the Data

To run this example, you need:

- an [incoming webhook](../../../local-integrations/local-webhooks.md) with the `crm` scope. The handler runs on the server: the page with the form does not use the webhook

- permissions for the user on whose behalf the webhook is created: to add contacts — for step 2, and to read contacts — for the verification step

- the page with the form and the handler on the same domain and port. In the example, `handlerUrl` is a relative path, so the request goes to the address the page was opened from. If you open `form.html` from a different address or from the file system, the handler will not receive the data

- environment variables with the webhook data. Set them in the process environment before startup, do not write them into the code:

   - `B24_HOOK` — the full webhook URL, for JS and PHP

   - `B24_DOMAIN` and `B24_WEBHOOK_TOKEN` — the domain and `USER_ID/TOKEN` without `https://`, for Python

   For example, for a local run:

   - Node.js — `B24_HOOK='https://your-domain.bitrix24.com/rest/1/TOKEN/' node handler.mjs`

   - PHP — `B24_HOOK='https://your-domain.bitrix24.com/rest/1/TOKEN/' php -S localhost:3000 -t public`

   - Python — `B24_DOMAIN='your-domain.bitrix24.com' B24_WEBHOOK_TOKEN='1/TOKEN' python handler.py`

If mandatory fields are configured for contacts in your Bitrix24, they must also be passed in the `fields` of the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method — otherwise the method returns an error. The [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method with `entityTypeId` set to `3` returns the list of fields with the `isRequired` flag.

For server-side JS examples with `B24Hook`, Node.js 18, 20, 22 or newer is required. For new projects, take 22 or newer: community support for Node.js 18 and 20 has ended. [B24JsSDK](../../../sdk/b24jssdk/index.md) is an ES module: save the code in an `.mjs` file or add `"type": "module"` to `package.json`.

For examples with [b24pysdk](../../../sdk/b24pysdk/index.md), Python 3.9 or newer is required.

For examples with `bitrix24/b24phpsdk:"^3.3"`, PHP 8.4 or newer is required with the `curl`, `intl`, and `json` extensions, and `mbstring` for the value length check in the example. For the SDK requirements and the recommended file layout, see the [B24PhpSDK](../../../sdk/b24phpsdk/index.md) page.

## 1. Creating the Web Form

We will create a web form on a website page with four fields:

- `NAME` — contact first name, a required form field

- `LAST_NAME` — last name

- `EMAIL` — email

- `PHONE` — phone

Save the page to the `form.html` file. Where it must be located depends on the handler:

- Node.js — in the `public` subfolder of the directory that contains the handler file. Run `node` from this directory: `express.static` looks for the `public` folder relative to the working directory of the process. The page opens at `http://localhost:3000/form.html`

- Flask — in the `static` folder. The page opens at `http://localhost:3000/static/form.html`

- PHP — in the public directory of the web server together with `form.php`. Keep the `vendor` directory above the public one so that it is not accessible from the browser. With the local run `php -S localhost:3000 -t public`, the page opens at `http://localhost:3000/form.html`

The addresses start working after the startup from step 2: in Node.js and Flask, the page is served by the same application that receives the form data, and in PHP — by the web server that hosts `form.php`.

The form sends the data to the handler with the `POST` method in the `application/x-www-form-urlencoded` format. The same code works with all three handler options: only the address in the `handlerUrl` variable changes.

```html
<form id="form_to_crm">
    <input type="text" name="NAME" placeholder="First Name" required>
    <input type="text" name="LAST_NAME" placeholder="Last Name">
    <input type="text" name="EMAIL" placeholder="Email">
    <input type="text" name="PHONE" placeholder="Phone">
    <input type="submit" value="Submit">
</form>

<script>
    // Handler address: '/form' — for Node.js and Flask, 'form.php' — for PHP
    const handlerUrl = '/form';

    document.getElementById('form_to_crm').addEventListener('submit', async (event) => {
        event.preventDefault(); // Canceling the standard form submission
        // Collecting the form fields into the request body
        const body = new URLSearchParams(new FormData(event.currentTarget));

        const response = await fetch(handlerUrl, { method: 'POST', body });
        // The handler responds with JSON. If something else arrives, we show a generic message
        const data = await response.json().catch(() => ({ message: 'The server returned an unexpected response' }));

        // Showing the result: the identifier will be needed for verification via REST
        alert(data.id ? data.message + '. ID: ' + data.id : data.message);
    });
</script>
```

The `required` attribute checks the mandatory field only in the browser. A request can be sent bypassing the form, so the handler validates the data again.

## 2. Creating a Form Handler

The handler receives the form data, validates it, and adds a contact using the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method. In the `entityTypeId` parameter, we pass `3` — the "Contact" object type; the values for the other types are listed in the [CRM object types reference](../../../api-reference/crm/data-types.md#object_type). In the `fields` object, we pass the fields:

- `name` — contact first name

- `lastName` — last name

- `fm` — an array of multifields, in which we pass the phone and email

The universal `crm.item.*` methods use field names in camelCase. They differ from the names in the methods of individual objects: `lastName` instead of `LAST_NAME`, and the phone and email are passed in a single `fm` array instead of separate `PHONE` and `EMAIL` fields.

We retrieve the field values from the form. Each action of the handler is described below, and the full code is provided at the end of the section.

{% include [Note on examples](../../../_includes/examples.md) %}

### Receiving the Request and Connecting the SDK

The handler receives a POST request at the address specified in the `handlerUrl` variable on the page with the form. We work with Bitrix24 through an incoming webhook.

{% list tabs %}

- JS

    ```javascript
    // npm install express @bitrix24/b24jssdk
    import express from 'express'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

    const app = express()
    // The form sends the data in the application/x-www-form-urlencoded format
    app.use(express.urlencoded({ extended: true }))
    // Serving the page with the form from the public folder
    app.use(express.static('public'))

    // Pattern for validating the email address
    const emailPattern = /^[^@\s]+@[^@\s]+\.[^@\s]+$/
    // Value length limit: the form is public
    const maxLength = 100

    // The handler receives the form data via the /form route
    app.post('/form', async (req, res) => {
        // The handler body — in the following steps
    })

    // Run: node handler.mjs
    app.listen(3000)
    ```

- Python

    ```python
    # pip install flask b24pysdk
    import os
    import re

    from flask import Flask, request, jsonify
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # We place the form.html page in the static folder
    app = Flask(__name__)

    client = Client(BitrixWebhook(
        domain=os.environ["B24_DOMAIN"],  # your-domain.bitrix24.com
        webhook_token=os.environ["B24_WEBHOOK_TOKEN"],  # user_id/token only, without https://
    ))

    # Pattern for validating the email address
    EMAIL_PATTERN = re.compile(r"[^@\s]+@[^@\s]+\.[^@\s]+")
    # Value length limit: the form is public
    MAX_LENGTH = 100


    @app.route("/form", methods=["POST"])
    def handle_form():
        ...  # The handler body — in the following steps


    # Run: python handler.py
    if __name__ == "__main__":
        app.run(port=3000)
    ```


- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.3"
    // form.php and form.html are located in the public directory, vendor — above it
    // Local run: php -S localhost:3000 -t public
    require_once __DIR__ . '/../vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;

    header('Content-Type: application/json; charset=utf-8');

    // Value length limit: the form is public
    const MAX_LENGTH = 100;

    // The handler accepts POST requests only
    if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
        http_response_code(405);
        echo json_encode(['message' => 'Method not supported']);
        exit;
    }

    $sb = ServiceBuilderFactory::createServiceBuilderFromWebhook(getenv('B24_HOOK'));
    ```
{% endlist %}

### Validating the Form Data

The data comes from an anonymous visitor, so the handler validates it before calling the method:

- accesses the fields with a default value: the required key may be missing from the request

- trims the surrounding spaces and does not create a contact if the first name is empty

- checks the email format so that a knowingly invalid address is not saved in the CRM

- rejects values that are too long: anything can arrive in a public form

In REST, we pass the values as is. Do not apply `htmlspecialchars` or other HTML escaping functions to them: those are needed when displaying data on a page, and because of them `Weber & Son` would end up in the CRM as `Weber &amp; Son`.

{% list tabs %}

- JS

    ```javascript
    // Getting the data from the form
    const sName = String(req.body.NAME ?? '').trim()
    const sLastName = String(req.body.LAST_NAME ?? '').trim()
    const sPhone = String(req.body.PHONE ?? '').trim()
    const sEmail = String(req.body.EMAIL ?? '').trim()

    // Validating the data before calling the method
    if (!sName) {
        res.status(400).json({ message: 'Enter the first name' })
        return
    }

    if (sEmail && !emailPattern.test(sEmail)) {
        res.status(400).json({ message: 'Check the email address' })
        return
    }

    if ([sName, sLastName, sPhone, sEmail].some(value => value.length > maxLength)) {
        res.status(400).json({ message: 'One of the fields is too long' })
        return
    }
    ```

- Python

    ```python
    # Getting the data from the form
    s_name = request.form.get("NAME", "").strip()
    s_last_name = request.form.get("LAST_NAME", "").strip()
    s_phone = request.form.get("PHONE", "").strip()
    s_email = request.form.get("EMAIL", "").strip()

    # Validating the data before calling the method
    if not s_name:
        return jsonify({"message": "Enter the first name"}), 400

    if s_email and not EMAIL_PATTERN.fullmatch(s_email):
        return jsonify({"message": "Check the email address"}), 400

    if any(len(value) > MAX_LENGTH for value in (s_name, s_last_name, s_phone, s_email)):
        return jsonify({"message": "One of the fields is too long"}), 400
    ```


- PHP

    ```php
    // Getting the data from the form
    $sName = trim((string)($_POST['NAME'] ?? ''));
    $sLastName = trim((string)($_POST['LAST_NAME'] ?? ''));
    $sPhone = trim((string)($_POST['PHONE'] ?? ''));
    $sEmail = trim((string)($_POST['EMAIL'] ?? ''));

    // Validating the data before calling the method
    if ($sName === '') {
        http_response_code(400);
        echo json_encode(['message' => 'Enter the first name']);
        exit;
    }

    if ($sEmail !== '' && !preg_match('/^[^@\s]+@[^@\s]+\.[^@\s]+$/u', $sEmail)) {
        http_response_code(400);
        echo json_encode(['message' => 'Check the email address']);
        exit;
    }

    foreach ([$sName, $sLastName, $sPhone, $sEmail] as $value) {
        if (mb_strlen($value) > MAX_LENGTH) {
            http_response_code(400);
            echo json_encode(['message' => 'One of the fields is too long']);
            exit;
        }
    }
    ```
{% endlist %}

### Collecting the Phone and Email into Multifields

The method accepts the phone and email in the `fm` field — an array of [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) objects. Each object has three keys:

- `typeId` — the multifield type: `PHONE` for a phone, `EMAIL` for an email

- `valueType` — the value type, for example `WORK` — work, `HOME` — home

- `value` — the value from the form

If the visitor did not fill in a field, we do not add the object to the array. If no value is filled in, we pass an empty array.

In the [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) reference, the multifield keys are listed in uppercase — `TYPE_ID`, `VALUE_TYPE`, `VALUE`. That is the format of the methods of individual objects, for example `crm.contact.add`. The universal `crm.item.*` methods accept and return the same keys in camelCase.

{% list tabs %}

- JS

    ```javascript
    // Collecting the phone and email into multifields
    const arFm = []

    if (sPhone) {
        arFm.push({ typeId: 'PHONE', valueType: 'WORK', value: sPhone })
    }

    if (sEmail) {
        arFm.push({ typeId: 'EMAIL', valueType: 'HOME', value: sEmail })
    }
    ```

- Python

    ```python
    # Collecting the phone and email into multifields
    ar_fm = []

    if s_phone:
        ar_fm.append({"typeId": "PHONE", "valueType": "WORK", "value": s_phone})

    if s_email:
        ar_fm.append({"typeId": "EMAIL", "valueType": "HOME", "value": s_email})
    ```


- PHP

    ```php
    // Collecting the phone and email into multifields
    $arFm = [];

    if ($sPhone !== '') {
        $arFm[] = ['typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => $sPhone];
    }

    if ($sEmail !== '') {
        $arFm[] = ['typeId' => 'EMAIL', 'valueType' => 'HOME', 'value' => $sEmail];
    }
    ```
{% endlist %}

### Creating the Contact

We pass the prepared values in the `fields` of the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method. The handler returns the identifier of the created contact to the page in the `id` field. We write the error text to the server log and return a generic message to the visitor: this way technical details do not end up on a public page.

{% list tabs %}

- JS

    ```javascript
    // Sending the data to Bitrix24
    try {
        const response = await $b24.actions.v2.call.make({
            method: 'crm.item.add',
            params: {
                entityTypeId: 3, // CRM object type — contact
                fields: {
                    name: sName, // First name
                    lastName: sLastName, // Last name
                    fm: arFm, // Phone and email
                }
            },
            requestId: 'contact-add'
        })

        // Checking the result and displaying a message
        if (!response.isSuccess) {
            // We write the error details to the log and do not show them to the visitor
            console.error(response.getErrorMessages().join('; '))
            res.status(502).json({ message: 'Could not create the contact, try again later' })
            return
        }

        const contactId = response.getData().result.item.id // Identifier of the created contact
        console.info('Contact created with ID ' + contactId)
        res.json({ message: 'Contact created', id: contactId })
    } catch (error) {
        // Network errors and SDK failures arrive as an exception
        console.error(error)
        res.status(502).json({ message: 'Could not create the contact, try again later' })
    }
    ```

- Python

    ```python
    # Sending the data to Bitrix24
    try:
        bitrix_response = client.crm.item.add(
            entity_type_id=3,  # CRM object type — contact
            fields={
                "name": s_name,  # First name
                "lastName": s_last_name,  # Last name
                "fm": ar_fm,  # Phone and email
            },
        ).response
        contact_id = bitrix_response.result["item"]["id"]  # Identifier of the created contact
        app.logger.info("Contact created with ID %s", contact_id)
        return jsonify({"message": "Contact created", "id": contact_id})
    except (BitrixAPIError, BitrixSDKException) as error:
        # We write the error details to the log and do not show them to the visitor
        app.logger.error(error)
        return jsonify({"message": "Could not create the contact, try again later"}), 502
    ```


- PHP

    ```php
    // Sending the data to Bitrix24
    try {
        $result = $sb->getCRMScope()->item()->add(3, [ // 3 — the "Contact" CRM object type
            'name' => $sName, // First name
            'lastName' => $sLastName, // Last name
            'fm' => $arFm, // Phone and email
        ]);

        $contactId = $result->item()->id; // Identifier of the created contact
        error_log('Contact created with ID ' . $contactId);
        echo json_encode(['message' => 'Contact created', 'id' => $contactId]);
    } catch (\Throwable $e) {
        // We write the error details to the log and do not show them to the visitor
        error_log($e->getMessage());
        http_response_code(502);
        echo json_encode(['message' => 'Could not create the contact, try again later']);
    }
    ```
{% endlist %}

The method returns the data of the created contact in the `result.item` object.

Abbreviated response:

```json
{
    "result": {
        "item": {
            "id": 46,
            "name": "Klaus",
            "lastName": "Weber",
            "entityTypeId": 3
        }
    }
}
```

The handler returns `{ "message": "Contact created", "id": 46 }` to the page. The identifier will be needed to open the contact in the interface or to request its data via REST.

### Full Handler Code Example

{% list tabs %}

- JS

    ```javascript
    // npm install express @bitrix24/b24jssdk
    import express from 'express'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

    const app = express()
    // The form sends the data in the application/x-www-form-urlencoded format
    app.use(express.urlencoded({ extended: true }))
    // Serving the page with the form from the public folder
    app.use(express.static('public'))

    // Pattern for validating the email address
    const emailPattern = /^[^@\s]+@[^@\s]+\.[^@\s]+$/
    // Value length limit: the form is public
    const maxLength = 100

    // The handler receives the form data via the /form route
    app.post('/form', async (req, res) => {
        // Getting the data from the form
        const sName = String(req.body.NAME ?? '').trim()
        const sLastName = String(req.body.LAST_NAME ?? '').trim()
        const sPhone = String(req.body.PHONE ?? '').trim()
        const sEmail = String(req.body.EMAIL ?? '').trim()

        // Validating the data before calling the method
        if (!sName) {
            res.status(400).json({ message: 'Enter the first name' })
            return
        }

        if (sEmail && !emailPattern.test(sEmail)) {
            res.status(400).json({ message: 'Check the email address' })
            return
        }

        if ([sName, sLastName, sPhone, sEmail].some(value => value.length > maxLength)) {
            res.status(400).json({ message: 'One of the fields is too long' })
            return
        }

        // Collecting the phone and email into multifields
        const arFm = []

        if (sPhone) {
            arFm.push({ typeId: 'PHONE', valueType: 'WORK', value: sPhone })
        }

        if (sEmail) {
            arFm.push({ typeId: 'EMAIL', valueType: 'HOME', value: sEmail })
        }

        // Sending the data to Bitrix24
        try {
            const response = await $b24.actions.v2.call.make({
                method: 'crm.item.add',
                params: {
                    entityTypeId: 3, // CRM object type — contact
                    fields: {
                        name: sName, // First name
                        lastName: sLastName, // Last name
                        fm: arFm, // Phone and email
                    }
                },
                requestId: 'contact-add'
            })

            // Checking the result and displaying a message
            if (!response.isSuccess) {
                // We write the error details to the log and do not show them to the visitor
                console.error(response.getErrorMessages().join('; '))
                res.status(502).json({ message: 'Could not create the contact, try again later' })
                return
            }

            const contactId = response.getData().result.item.id // Identifier of the created contact
            console.info('Contact created with ID ' + contactId)
            res.json({ message: 'Contact created', id: contactId })
        } catch (error) {
            // Network errors and SDK failures arrive as an exception
            console.error(error)
            res.status(502).json({ message: 'Could not create the contact, try again later' })
        }
    })

    // Run: node handler.mjs
    app.listen(3000)
    ```

- Python

    ```python
    # pip install flask b24pysdk
    import os
    import re

    from flask import Flask, request, jsonify
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # We place the form.html page in the static folder
    app = Flask(__name__)

    client = Client(BitrixWebhook(
        domain=os.environ["B24_DOMAIN"],  # your-domain.bitrix24.com
        webhook_token=os.environ["B24_WEBHOOK_TOKEN"],  # user_id/token only, without https://
    ))

    # Pattern for validating the email address
    EMAIL_PATTERN = re.compile(r"[^@\s]+@[^@\s]+\.[^@\s]+")
    # Value length limit: the form is public
    MAX_LENGTH = 100


    @app.route("/form", methods=["POST"])
    def handle_form():
        # Getting the data from the form
        s_name = request.form.get("NAME", "").strip()
        s_last_name = request.form.get("LAST_NAME", "").strip()
        s_phone = request.form.get("PHONE", "").strip()
        s_email = request.form.get("EMAIL", "").strip()

        # Validating the data before calling the method
        if not s_name:
            return jsonify({"message": "Enter the first name"}), 400

        if s_email and not EMAIL_PATTERN.fullmatch(s_email):
            return jsonify({"message": "Check the email address"}), 400

        if any(len(value) > MAX_LENGTH for value in (s_name, s_last_name, s_phone, s_email)):
            return jsonify({"message": "One of the fields is too long"}), 400

        # Collecting the phone and email into multifields
        ar_fm = []

        if s_phone:
            ar_fm.append({"typeId": "PHONE", "valueType": "WORK", "value": s_phone})

        if s_email:
            ar_fm.append({"typeId": "EMAIL", "valueType": "HOME", "value": s_email})

        # Sending the data to Bitrix24
        try:
            bitrix_response = client.crm.item.add(
                entity_type_id=3,  # CRM object type — contact
                fields={
                    "name": s_name,  # First name
                    "lastName": s_last_name,  # Last name
                    "fm": ar_fm,  # Phone and email
                },
            ).response
            contact_id = bitrix_response.result["item"]["id"]  # Identifier of the created contact
            app.logger.info("Contact created with ID %s", contact_id)
            return jsonify({"message": "Contact created", "id": contact_id})
        except (BitrixAPIError, BitrixSDKException) as error:
            # We write the error details to the log and do not show them to the visitor
            app.logger.error(error)
            return jsonify({"message": "Could not create the contact, try again later"}), 502


    # Run: python handler.py
    if __name__ == "__main__":
        app.run(port=3000)
    ```


- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.3"
    // form.php and form.html are located in the public directory, vendor — above it
    // Local run: php -S localhost:3000 -t public
    require_once __DIR__ . '/../vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;

    header('Content-Type: application/json; charset=utf-8');

    // Value length limit: the form is public
    const MAX_LENGTH = 100;

    // The handler accepts POST requests only
    if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
        http_response_code(405);
        echo json_encode(['message' => 'Method not supported']);
        exit;
    }

    $sb = ServiceBuilderFactory::createServiceBuilderFromWebhook(getenv('B24_HOOK'));

    // Getting the data from the form
    $sName = trim((string)($_POST['NAME'] ?? ''));
    $sLastName = trim((string)($_POST['LAST_NAME'] ?? ''));
    $sPhone = trim((string)($_POST['PHONE'] ?? ''));
    $sEmail = trim((string)($_POST['EMAIL'] ?? ''));

    // Validating the data before calling the method
    if ($sName === '') {
        http_response_code(400);
        echo json_encode(['message' => 'Enter the first name']);
        exit;
    }

    if ($sEmail !== '' && !preg_match('/^[^@\s]+@[^@\s]+\.[^@\s]+$/u', $sEmail)) {
        http_response_code(400);
        echo json_encode(['message' => 'Check the email address']);
        exit;
    }

    foreach ([$sName, $sLastName, $sPhone, $sEmail] as $value) {
        if (mb_strlen($value) > MAX_LENGTH) {
            http_response_code(400);
            echo json_encode(['message' => 'One of the fields is too long']);
            exit;
        }
    }

    // Collecting the phone and email into multifields
    $arFm = [];

    if ($sPhone !== '') {
        $arFm[] = ['typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => $sPhone];
    }

    if ($sEmail !== '') {
        $arFm[] = ['typeId' => 'EMAIL', 'valueType' => 'HOME', 'value' => $sEmail];
    }

    // Sending the data to Bitrix24
    try {
        $result = $sb->getCRMScope()->item()->add(3, [ // 3 — the "Contact" CRM object type
            'name' => $sName, // First name
            'lastName' => $sLastName, // Last name
            'fm' => $arFm, // Phone and email
        ]);

        $contactId = $result->item()->id; // Identifier of the created contact
        error_log('Contact created with ID ' . $contactId);
        echo json_encode(['message' => 'Contact created', 'id' => $contactId]);
    } catch (\Throwable $e) {
        // We write the error details to the log and do not show them to the visitor
        error_log($e->getMessage());
        http_response_code(502);
        echo json_encode(['message' => 'Could not create the contact, try again later']);
    }
    ```
{% endlist %}

## Verify the Result

1. Submit the form. A message like "Contact created. ID: 46" appears in the browser — the identifier will be needed in step 3

2. Open the CRM section and go to Contacts. The new contact has the first name and last name from the form

3. Request the contact data using the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method. Pass `entityTypeId` set to `3` and the `id` from the handler response

Run the verification with a separate script: it does not depend on the handler and connects to Bitrix24 through the same webhook.

{% list tabs %}

- JS

    ```javascript
    // Save to the check.mjs file in the project directory next to node_modules
    // Run: B24_HOOK='https://your-domain.bitrix24.com/rest/1/TOKEN/' node check.mjs
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    const contactId = 46 // Identifier from the handler response

    const response = await $b24.actions.v2.call.make({
        method: 'crm.item.get',
        params: { entityTypeId: 3, id: contactId },
        requestId: 'contact-get'
    })

    console.info(response.getData().result.item)
    ```

- Python

    ```python
    # Save to the check.py file in the project directory
    # Run: B24_DOMAIN='your-domain.bitrix24.com' B24_WEBHOOK_TOKEN='1/TOKEN' python check.py
    import os

    from b24pysdk import BitrixWebhook, Client

    client = Client(BitrixWebhook(
        domain=os.environ["B24_DOMAIN"],
        webhook_token=os.environ["B24_WEBHOOK_TOKEN"],
    ))
    contact_id = 46  # Identifier from the handler response

    bitrix_response = client.crm.item.get(entity_type_id=3, bitrix_id=contact_id).response

    print(bitrix_response.result["item"])
    ```


- PHP

    ```php
    <?php
    // Save to the check.php file in the project root next to the vendor directory
    // Run: B24_HOOK='https://your-domain.bitrix24.com/rest/1/TOKEN/' php check.php
    require_once __DIR__ . '/vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;

    $sb = ServiceBuilderFactory::createServiceBuilderFromWebhook(getenv('B24_HOOK'));
    $contactId = 46; // Identifier from the handler response

    $result = $sb->getCRMScope()->item()->get(3, $contactId);

    print_r($result->item());
    ```
{% endlist %}

Abbreviated response:

```json
{
    "result": {
        "item": {
            "id": 46,
            "name": "Klaus",
            "lastName": "Weber",
            "hasPhone": "Y",
            "hasEmail": "Y",
            "fm": [
                {
                    "id": 156,
                    "valueType": "WORK",
                    "value": "+49000000000",
                    "typeId": "PHONE"
                },
                {
                    "id": 157,
                    "valueType": "HOME",
                    "value": "klaus@example.com",
                    "typeId": "EMAIL"
                }
            ]
        }
    }
}
```

The `name` and `lastName` values match the form fields. The phone and email are returned in the `fm` array with the same `typeId` and `valueType` types that the handler passed. The `hasPhone` and `hasEmail` flags show that the contact has a phone and email filled in — they confirm that the multifields were saved.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| `ACCESS_DENIED` | The user on whose behalf the webhook is created lacks the required permission: to add contacts — for step 2, and to read contacts — for the verification step. Check the permissions in the CRM settings ||
|| `NOT_FOUND` | At step 2 — an invalid `entityTypeId` was passed, a contact requires the value `3`. At the verification step — there is no object with such an `id`: substitute the identifier from the handler response ||
|| `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid field value. Check the email address, and also that the field names are written in camelCase and that the multifields are passed in `fm` ||
|| `100` | A non-iterable value was passed to a multiple field. Make sure that `fm` is an array, even if it is empty ||
|| `NO_AUTH_FOUND` | Invalid webhook code. Check the value of the environment variable with the webhook URL ||
|| `insufficient_scope` | The webhook does not have the `crm` scope. Create the webhook again and select the required permission ||
|| `QUERY_LIMIT_EXCEEDED` | The request rate limit is exceeded. Repeat the call later ||
|#

The method checks the mandatory fields itself and returns an error if a field is empty. Look for the text of such an error in the handler log: only a generic message is sent to the visitor. The method silently ignores something else — an unknown field name: a typo in `fields` does not raise an error, and the value is not saved. That is why, after the first run, you should verify the saved data with the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method. The full list of method errors is provided on the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) page.

The handler makes a single call, so after fixing an error, submit the form again in full — no incomplete records remain in the CRM. The messages "Enter the first name", "Check the email address", and "One of the fields is too long" are returned by the handler itself with the status `400`, before contacting Bitrix24.

The method does not return errors that occur between the form and the handler — they are visible in the browser.

#|
|| **Symptom** | **Reason and action** ||
|| The handler exits immediately at startup or responds with an error to the very first request | The environment variables with the webhook data are not set: `B24Hook.fromWebhookUrl` and `os.environ` terminate with an error, while in PHP `getenv` returns `false` and the SDK throws the error. `fromWebhookUrl` also validates the URL: the protocol must be HTTPS, and the user identifier must be a number ||
|| The page is opened from the file system at an address like `file://` | There is nothing to resolve the relative path from `handlerUrl` against, and the request is not sent. Open the page at the application address from step 1 ||
|| A CORS error in the browser console | An absolute address of a different domain or port is substituted in `handlerUrl`. The browser will not let the page read the response, but the request is executed and the object is created in the CRM — check for duplicates. With the relative path from the example, this error does not occur ||
|| A 404 error in the browser console | An invalid address in the `handlerUrl` variable. For Node.js and Flask, it is `/form`, and for PHP — `form.php` ||
|| A `405` response with the message "Method not supported" | The `form.php` file is opened directly with a GET request. The handler accepts POST only — send the data with the form ||
|| The message "The server returned an unexpected response" | The handler responded with something other than JSON: it crashed before `header` or returned a web server error page. Check the log: `console.error` — in the Node.js output, `error_log` — in the PHP log, `app.logger.error` — in the Flask output ||
|#

## Key Considerations

- Each form submission creates a new contact. If a client contacts you again, duplicates appear. You can find matches by phone and email before creating a record with the [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) method; an example of such a check is covered in the tutorial [Add Repeat Lead](./how-to-add-repeat-lead.md)

- A webhook grants access to the entire CRM. Call REST from the server only and do not pass the webhook URL to the browser

- The form is filled out by anonymous visitors. The handler from the example limits the value length, but protection against automated submissions must be added separately, for example a captcha

- When switching to a different CRM object type, it is not only `entityTypeId` that changes: each type has its own set of fields. Refer to the description of the `fields` parameter on the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) page

- The built-in servers from the examples — `app.listen`, `app.run`, `php -S` — are suitable for local verification of the scenario. Host the public page on a web server over HTTPS: the form collects the client's personal data

- The scenario uses the universal [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method. Development of the [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) method has stopped: it continues to work, but use `crm.item.add` in new integrations

## Continue Learning

- [{#T}](../../../api-reference/crm/universal/crm-item-add.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-get.md)
- [{#T}](./how-to-add-contact-with-requisite.md)
- [{#T}](./how-to-add-lead.md)
- [{#T}](./how-to-add-company.md)
- [{#T}](./how-to-add-repeat-lead.md)
