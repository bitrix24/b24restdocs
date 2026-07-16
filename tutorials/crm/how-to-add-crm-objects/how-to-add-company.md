# Adding a Company via Web Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with the permission to create companies in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can place a form on your website to collect client data. When a client fills out the form, their information will be sent to the CRM, allowing you to process the request.

Setting up the form consists of two steps.

1. Place the form on an HTML page. It will send data to the handler.

2. Create a file to process the data. The handler will accept and prepare the data, then create a company using the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) method.

## 1. Creating the Web Form

We will create a web form on a website page with three fields:

-  `TITLE` — company name, required,

-  `EMAIL` — Email,

-  `PHONE` — phone.

When submitted, the form passes the data to the handler.

{% list tabs %}

- JS

    ```html
    <form id="form_to_crm">
        <!-- Company name (required field) -->
        <input type="text" name="TITLE" placeholder="Company name" required>
        <!-- Email address -->
        <input type="text" name="EMAIL" placeholder="Email">
        <!-- Phone number -->
        <input type="text" name="PHONE" placeholder="Phone">
        <!-- Submit button -->
        <input type="submit" value="Submit">
    </form>

    <script>
        document.getElementById('form_to_crm').addEventListener('submit', async (el) => {
            el.preventDefault(); // Canceling standard submission
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
        <!-- Company name (required field) -->
        <input type="text" name="TITLE" placeholder="Company name" required>
        <!-- Email address -->
        <input type="text" name="EMAIL" placeholder="Email">
        <!-- Phone number -->
        <input type="text" name="PHONE" placeholder="Phone">
        <!-- Submit button -->
        <input type="submit" value="Submit">
    </form>

    <!-- Connecting jQuery for the AJAX request -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            // Form submission without page reload
            $('#form_to_crm').on('submit', function(el) {
                el.preventDefault(); // Canceling standard submission
                // Getting form data
                var formData = $(this).serialize();
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
        <!-- Company name (required field) -->
        <input type="text" name="TITLE" placeholder="Company name" required>
        <!-- Email address -->
        <input type="text" name="EMAIL" placeholder="Email">
        <!-- Phone number -->
        <input type="text" name="PHONE" placeholder="Phone">
        <!-- Submit button -->
        <input type="submit" value="Submit">
    </form>

    <!-- Connecting jQuery for the AJAX request -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            // Form submission without page reload
            $('#form_to_crm').on('submit', function(el) {
                el.preventDefault(); // Canceling standard submission
                // Getting form data
                var formData = $(this).serialize();
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

To process the values from the form fields and add a company to the CRM, we will create a handler.

Use the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) method to add the company. In the `fields` object, pass the following fields:

-  `TITLE` — company name,

-  `COMPANY_TYPE` — company type. Specify `CUSTOMER`, since only company customers fill out the form,

-  `PHONE` — phone number,

-  `EMAIL` — Email.

Retrieve the values for fields `TITLE`, `PHONE`, and `EMAIL` from the form. The system stores phone and email as an array of [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) objects, so they must be converted to an array format.

1. If a value exists, add it as the first item `VALUE` in the array, and specify the type `VALUE_TYPE` as the second value, for example:

   -  `WORK` — for the phone,

   -  `HOME` — for the email.

2. If no value exists, pass an empty array.

{% note warning "" %}

Check which mandatory fields are configured for companies in your Bitrix24. All mandatory fields must be passed to the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) method.

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
        // Getting data from the form
        const sTitle = String(req.body.TITLE ?? '')
        const sPhone = String(req.body.PHONE ?? '')
        const sEmail = String(req.body.EMAIL ?? '')

        // Formatting phone and email for Bitrix24 into the crm_multifield format
        const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []
        const arEmail = sEmail ? [{ VALUE: sEmail, VALUE_TYPE: 'HOME' }] : []

        // Sending data to Bitrix24
        const response = await $b24.actions.v2.call.make({
            method: 'crm.company.add',
            params: {
                fields: {
                    TITLE: sTitle, // Company name
                    COMPANY_TYPE: 'CUSTOMER', // Company type — client
                    PHONE: arPhone, // Phone
                    EMAIL: arEmail, // Email
                }
            },
            requestId: 'company-add'
        })

        // Returning the result
        if (response.isSuccess && response.getData()?.result) {
            res.json({ message: 'Company added' })
        } else {
            res.json({ message: 'Company not added: ' + response.getErrorMessages().join('; ') })
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

    // Getting data from the form
    $sTitle = htmlspecialchars($_POST["TITLE"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    $sEmail = htmlspecialchars($_POST["EMAIL"]);

    // Formatting phone and email for Bitrix24 into the crm_multifield format
    $arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
    $arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();

    // Sending data to Bitrix24
    try {
        $companyId = $sb->getCRMScope()->company()->add([
            "TITLE" => $sTitle, // Company name
            "COMPANY_TYPE" => 'CUSTOMER', // Company type — client
            "PHONE" => $arPhone, // Phone
            "EMAIL" => $arEmail, // Email
        ])->getId();

        echo json_encode(['message' => 'Company added']);
    } catch (\Throwable $e) {
        echo json_encode(['message' => 'Company not added: ' . $e->getMessage()]);
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
        s_title = request.form.get("TITLE", "")
        s_phone = request.form.get("PHONE", "")
        s_email = request.form.get("EMAIL", "")

        # Formatting phone and email for Bitrix24 into the crm_multifield format
        ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []
        ar_email = [{"VALUE": s_email, "VALUE_TYPE": "HOME"}] if s_email else []

        # Sending data to Bitrix24
        try:
            client.crm.company.add(fields={
                "TITLE": s_title,  # Company name
                "COMPANY_TYPE": "CUSTOMER",  # Company type — client
                "PHONE": ar_phone,  # Phone
                "EMAIL": ar_email,  # Email
            })
            return jsonify({"message": "Company added"})
        except Exception as e:
            return jsonify({"message": f"Company not added: {e}"})
    ```

{% endlist %}
