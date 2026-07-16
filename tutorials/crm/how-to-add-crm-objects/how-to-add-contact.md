# Add Contact via Web Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to create contacts in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can place a form on your website to collect client data. When a client fills out the form, their information will be sent to the CRM, allowing you to process the request.

Setting up the form consists of two steps.

1. Place the form on an HTML page. It will send data to the handler.

2. Create a file to process the data. The handler will receive and prepare the data, and then create a contact using the [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) method.

## 1. Creating the Web Form

We will create a web form on a website page with four fields:

-  `NAME` — contact name, required,

-  `LAST_NAME` — last name,

-  `EMAIL` — Email,

-  `PHONE` — phone.

When submitted, the form passes the data to the handler.

{% list tabs %}

- JS

    ```html
    <form id="form_to_crm">
        <!-- First name, required field -->
        <input type="text" name="NAME" placeholder="First name" required>
        <!-- Last name -->
        <input type="text" name="LAST_NAME" placeholder="Last name">
        <!-- Email -->
        <input type="text" name="EMAIL" placeholder="Email">
        <!-- Phone -->
        <input type="text" name="PHONE" placeholder="Phone">
        <!-- Submit button -->
        <input type="submit" value="Submit">
    </form>

    <script>
        document.getElementById('form_to_crm').addEventListener('submit', async (el) => {
            el.preventDefault(); // Canceling standard form submission
            // Collecting form data into JSON
            const formData = Object.fromEntries(new FormData(el.currentTarget).entries());
            // Sending data to the server (Node.js handler endpoint)
            const response = await fetch('/form', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(formData),
            });
            const data = await response.json();
            alert(data.message); // Showing the result
        });
    </script>
    ```

- PHP

    ```html
    <form id="form_to_crm" method="POST" action="form.php">
        <!-- First name, required field -->
        <input type="text" name="NAME" placeholder="First name" required>
        <!-- Last name -->
        <input type="text" name="LAST_NAME" placeholder="Last name">
        <!-- Email -->
        <input type="text" name="EMAIL" placeholder="Email">
        <!-- Phone -->
        <input type="text" name="PHONE" placeholder="Phone">
        <!-- Submit button -->
        <input type="submit" value="Submit">
    </form>

    <!-- Connecting jQuery for the AJAX request -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            $('#form_to_crm').on('submit', function(el) {
                el.preventDefault(); // Canceling standard form submission
                var formData = $(this).serialize(); // Collecting form data
                // Sending data to the server
                $.ajax({
                    'method': 'POST',
                    'dataType': 'json',
                    'url': 'form.php', // Handler file
                    'data': formData,
                    success: function(data) {
                        alert(data.message); // Showing the result
                    }
                });
            });
        });
    </script>
    ```

- Python

    ```html
    <form id="form_to_crm">
        <!-- First name, required field -->
        <input type="text" name="NAME" placeholder="First name" required>
        <!-- Last name -->
        <input type="text" name="LAST_NAME" placeholder="Last name">
        <!-- Email -->
        <input type="text" name="EMAIL" placeholder="Email">
        <!-- Phone -->
        <input type="text" name="PHONE" placeholder="Phone">
        <!-- Submit button -->
        <input type="submit" value="Submit">
    </form>

    <!-- Connecting jQuery for the AJAX request -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            $('#form_to_crm').on('submit', function(el) {
                el.preventDefault(); // Canceling standard form submission
                var formData = $(this).serialize(); // Collecting form data
                // Sending data to the server (Flask handler route)
                $.ajax({
                    'method': 'POST',
                    'dataType': 'json',
                    'url': '/form', // Handler route
                    'data': formData,
                    success: function(data) {
                        alert(data.message); // Showing the result
                    }
                });
            });
        });
    </script>
    ```

{% endlist %}

## 2. Create a Form Handler

To process the values from the form fields and add a contact to the CRM, we will create a handler.

To add a contact, use the [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) method. In the `fields` object, pass the following fields:

-  `NAME` — contact name,

-  `LAST_NAME` — last name,

-  `PHONE` — phone number,

-  `EMAIL` — Email.

Retrieve the field values from the form. The system stores phone and email as an array of [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) objects, so they must be converted to an array format.

1. If a value exists, add it as the first item `VALUE` in the array, and specify the type as the second value `VALUE_TYPE`, for example:

   -  `WORK` — for phone,

   -  `HOME` — for email.

2. If no value exists, pass an empty array.

{% note warning "" %}

Check which mandatory fields are configured for contacts in your Bitrix24. All mandatory fields must be passed to the [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) method.

{% endnote %}

{% list tabs %}

- JS

    ```javascript
    import express from 'express'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const app = express()
    app.use(express.json())

    // The handler receives form data via the /form route
    app.post('/form', async (req, res) => {
        // Getting and cleaning data from the form
        const sName = String(req.body.NAME ?? '')
        const sLastName = String(req.body.LAST_NAME ?? '')
        const sPhone = String(req.body.PHONE ?? '')
        const sEmail = String(req.body.EMAIL ?? '')

        // Formatting phone and email for Bitrix24 into the crm_multifield format
        const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []
        const arEmail = sEmail ? [{ VALUE: sEmail, VALUE_TYPE: 'HOME' }] : []

        // Sending data to Bitrix24
        const response = await $b24.actions.v2.call.make({
            method: 'crm.contact.add',
            params: {
                fields: {
                    NAME: sName, // First name
                    LAST_NAME: sLastName, // Last name
                    PHONE: arPhone, // Phone
                    EMAIL: arEmail, // Email
                }
            },
            requestId: 'contact-add'
        })

        // Checking the result and displaying a message
        if (response.isSuccess && response.getData()?.result) {
            res.json({ message: 'Contact add' })
        } else {
            res.json({ message: 'Contact not added: ' + response.getErrorMessages().join('; ') })
        }
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
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // Getting and cleaning data from the form
    $sName = htmlspecialchars($_POST["NAME"]);
    $sLastName = htmlspecialchars($_POST["LAST_NAME"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    $sEmail = htmlspecialchars($_POST["EMAIL"]);

    // Formatting phone and email for Bitrix24 into the crm_multifield format
    $arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
    $arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();

    // Sending data to Bitrix24
    try {
        $contactId = $sb->getCRMScope()->contact()->add([
            'NAME' => $sName, // First name
            'LAST_NAME' => $sLastName, // Last name
            'PHONE' => $arPhone, // Phone
            'EMAIL' => $arEmail, // Email
        ])->getId();

        echo json_encode(['message' => 'Contact add']);
    } catch (\Throwable $e) {
        echo json_encode(['message' => 'Contact not added: ' . $e->getMessage()]);
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

    @app.route("/form", methods=["POST"])
    def handle_form():
        # Getting data from the form
        s_name = request.form.get("NAME", "")
        s_last_name = request.form.get("LAST_NAME", "")
        s_phone = request.form.get("PHONE", "")
        s_email = request.form.get("EMAIL", "")

        # Formatting phone and email for Bitrix24 into the crm_multifield format
        ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []
        ar_email = [{"VALUE": s_email, "VALUE_TYPE": "HOME"}] if s_email else []

        # Sending data to Bitrix24
        try:
            client.crm.contact.add(fields={
                "NAME": s_name,  # First name
                "LAST_NAME": s_last_name,  # Last name
                "PHONE": ar_phone,  # Phone
                "EMAIL": ar_email,  # Email
            })
            return jsonify({"message": "Contact add"})
        except Exception as e:
            return jsonify({"message": f"Contact not added: {e}"})
    ```

{% endlist %}
