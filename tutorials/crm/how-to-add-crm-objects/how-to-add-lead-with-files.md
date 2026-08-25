# Add Lead with Files via Web Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — CRM administrator. It is needed once, to create the custom fields for files
>
> - [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md) — CRM administrator
> - [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) — a user with permission to create leads
> - [crm.lead.get](../../../api-reference/crm/leads/crm-lead-get.md) — a user with permission to read leads

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can place a form on your website to collect data from potential clients. When a client fills out the form and attaches files, their data will be sent to the CRM, allowing you to process the request.

As a result of the scenario, a new lead appears in the CRM. The lead card has the first name, last name, company name, phone number, and email address filled in, and the custom fields of the "file" type hold the attachments the client added to the form.

The setup consists of two stages:

1. Prepare the fields and place the form on the page

2. Create a handler file. It receives and prepares the data, encodes the files in Base64, and creates a lead with the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method

## Before You Start

- Two custom fields of the "file" type are created for leads: one for a single file and one with the "multiple" flag for several files. Create them in Bitrix24 manually or with the [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md) method, using the `USER_TYPE_ID`: `file` parameter and `MULTIPLE`: `Y` for the multiple field

- The webhook is created on behalf of a user with permission to create leads

- You have a server that serves the page with the form and accepts the form data using the `POST` method in the `multipart/form-data` format. In the examples, this is Express with the `multer` package for JS, a PHP script, and Flask for Python

- The webhook URL is stored in the environment, not in the page code. The form is on a public page, and the secret must not end up in it

## 1. Creating the Web Form

In Bitrix24, a contact and a company can be automatically created from a lead. To make the form suitable for different scenarios, we will make it universal. For a contact, a first name and last name must be specified, and for a company, a name is required. We will create a web form on a website page with the following fields:

- `NAME` — First Name, a required field

- `LAST_NAME` — Last Name

- `COMPANY_TITLE` — Company Name

- `EMAIL` — Email

- `PHONE` — Phone

To allow the customer to upload files, we will add the following fields to the form:

- `FILE` — for a single file

- `FILES` — for several files

The form passes the data to the handler using the `POST` method. The `enctype="multipart/form-data"` attribute is required: without it, the browser sends only the file names, not their content.

### Full Code Example of the Form Page

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```html
    <form id="form_to_crm" enctype="multipart/form-data">
        <!-- First Name (required field) -->
        <input type="text" name="NAME" placeholder="First Name" required>
        <!-- Last Name -->
        <input type="text" name="LAST_NAME" placeholder="Last Name">
        <!-- Company Name -->
        <input type="text" name="COMPANY_TITLE" placeholder="Company Name">
        <!-- Email -->
        <input type="text" name="EMAIL" placeholder="Email">
        <!-- Phone -->
        <input type="text" name="PHONE" placeholder="Phone">
        <!-- Single file field -->
        <input type="file" name="FILE">
        <!-- Multiple files field -->
        <input type="file" name="FILES" multiple>
        <!-- Submit button -->
        <input type="submit" value="Submit">
    </form>

    <script>
        document.getElementById('form_to_crm').addEventListener('submit', async (el) => {
            el.preventDefault();
            // FormData will collect text fields and files itself (multipart/form-data)
            const formData = new FormData(el.currentTarget);
            // Do not specify Content-Type — the browser will set multipart with boundary
            const response = await fetch('/form', { method: 'POST', body: formData });
            const data = await response.json();
            alert(data.message);
        });
    </script>
    ```

- Python

    ```html
    <form id="form_to_crm" enctype="multipart/form-data">
        <!-- First Name (required field) -->
        <input type="text" name="NAME" placeholder="First Name" required>
        <!-- Last Name -->
        <input type="text" name="LAST_NAME" placeholder="Last Name">
        <!-- Company Name -->
        <input type="text" name="COMPANY_TITLE" placeholder="Company Name">
        <!-- Email -->
        <input type="text" name="EMAIL" placeholder="Email">
        <!-- Phone -->
        <input type="text" name="PHONE" placeholder="Phone">
        <!-- Single file field -->
        <input type="file" name="FILE">
        <!-- Multiple files field -->
        <input type="file" name="FILES" multiple>
        <!-- Submit button -->
        <input type="submit" value="Submit">
    </form>

    <!-- Include jQuery for the AJAX request -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            $('#form_to_crm').on('submit', function(el) {
                el.preventDefault();
                var formData = new FormData(this); // Collect form data including files
                $.ajax({
                    method: 'POST',
                    url: '/form', // Flask handler route
                    data: formData,
                    processData: false,
                    contentType: false,
                    dataType: 'json',
                    success: function(data) {
                        alert(data.message);
                    },
                    error: function() {
                        alert('Error during form submission');
                    }
                });
            });
        });
    </script>
    ```


- PHP

    ```html
    <form id="form_to_crm" method="POST" action="form.php" enctype="multipart/form-data">
        <!-- First Name (required field) -->
        <input type="text" name="NAME" placeholder="First Name" required>
        <!-- Last Name -->
        <input type="text" name="LAST_NAME" placeholder="Last Name">
        <!-- Company Name -->
        <input type="text" name="COMPANY_TITLE" placeholder="Company Name">
        <!-- Email -->
        <input type="text" name="EMAIL" placeholder="Email">
        <!-- Phone -->
        <input type="text" name="PHONE" placeholder="Phone">
        <!-- Single file field -->
        <input type="file" name="FILE">
        <!-- Multiple files field -->
        <input type="file" name="FILES" multiple>
        <!-- Submit button -->
        <input type="submit" value="Submit">
    </form>

    <!-- Include jQuery for the AJAX request -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            $('#form_to_crm').on('submit', function(el) {
                el.preventDefault();
                var formData = new FormData(this); // Collect form data including files
                $.ajax({
                    method: 'POST',
                    url: 'form.php',
                    data: formData,
                    processData: false,
                    contentType: false,
                    dataType: 'json',
                    success: function(data) {
                        alert(data.message);
                    },
                    error: function() {
                        alert('Error during form submission');
                    }
                });
            });
        });
    </script>
    ```
{% endlist %}

## 2. Create a Form Handler

The handler accepts the form field values, prepares them for the method, and adds a lead to the CRM.

### Prepare Form Data

#### Retrieve Field Values

Read the `NAME`, `LAST_NAME`, `COMPANY_TITLE`, `PHONE`, and `EMAIL` fields and cast them to a string. If a field is empty, you get an empty string rather than `undefined` or `None`.

The form is filled out by a site visitor, so the values cannot be considered safe. In the PHP example, they are additionally passed through `htmlspecialchars`. If you return these values back to the page, escape them in the other examples as well.

{% list tabs %}

- JS

    ```javascript
    // Get data from the form
    const sName = String(req.body.NAME ?? '')
    const sLastName = String(req.body.LAST_NAME ?? '')
    const sCompanyTitle = String(req.body.COMPANY_TITLE ?? '')
    const sPhone = String(req.body.PHONE ?? '')
    const sEmail = String(req.body.EMAIL ?? '')
    ```

- Python

    ```python
    # Get data from the form
    s_name = request.form.get("NAME", "")
    s_last_name = request.form.get("LAST_NAME", "")
    s_company_title = request.form.get("COMPANY_TITLE", "")
    s_phone = request.form.get("PHONE", "")
    s_email = request.form.get("EMAIL", "")
    ```


- PHP

    ```php
    // Get and sanitize data from the form
    $sName = htmlspecialchars($_POST["NAME"]);
    $sLastName = htmlspecialchars($_POST["LAST_NAME"]);
    $sCompanyTitle = htmlspecialchars($_POST["COMPANY_TITLE"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    $sEmail = htmlspecialchars($_POST["EMAIL"]);
    ```
{% endlist %}

#### Prepare Files

The [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method accepts a file as an object with the `fileData` key. The key holds an array of two items:

- the file name

- the file content, encoded in Base64

Pass such an object to the single-file field, and an array of such objects to the multiple-files field. To encode a file, use the `base64_encode` function in PHP, the `Buffer.toString('base64')` method in JS, and the `base64` module in Python.

{% note tip "Documentation" %}

- [How to Work with Files](../../../api-reference/files/index.md)

- [How to Choose a Transfer Format](../../../api-reference/files/how-to-upload-files.md#formats)

{% endnote %}

{% list tabs %}

- JS

    ```javascript
    // Create variables for arrays with files
    const arFiles = []
    let arSingleFile = []

    // Process the FILES field with multiple files (multer stores them in req.files)
    for (const file of req.files?.FILES ?? []) {
        arFiles.push({
            fileData: [
                file.originalname, // filename
                file.buffer.toString('base64'), // file content, encoded in base64
            ]
        })
    }

    // Process the FILE field with a single file
    const single = req.files?.FILE?.[0]
    if (single) {
        arSingleFile = {
            fileData: [
                single.originalname, // filename
                single.buffer.toString('base64'), // file content, encoded in base64
            ]
        }
    }
    ```

- Python

    ```python
    import base64

    # Create variables for arrays with files
    ar_files = []
    ar_single_file = []

    # Process the FILES field with multiple files
    for file in request.files.getlist("FILES"):
        if file and file.filename:
            ar_files.append({
                "fileData": [
                    file.filename,  # filename
                    base64.b64encode(file.read()).decode(),  # file content, encoded in base64
                ]
            })

    # Process the FILE field with a single file
    single = request.files.get("FILE")
    if single and single.filename:
        ar_single_file = {
            "fileData": [
                single.filename,  # filename
                base64.b64encode(single.read()).decode(),  # file content, encoded in base64
            ]
        }
    ```


- PHP

    ```php
    // Create variables for arrays with files
    $arFiles = [];
    $arSingleFile = [];

    // Process the FILES field with multiple files
    if(!empty($_FILES['FILES']['tmp_name'])) {
        foreach($_FILES['FILES']['tmp_name'] as $key => $tmpName) {
            if(!empty($tmpName)) {
                $arFiles[] = [
                    'fileData' => [
                        $_FILES['FILES']['name'][$key], // filename
                        base64_encode(file_get_contents($tmpName)) // file content, encoded in base64 
                    ]
                ];
            }
        }
    }

    // Process the FILE field with a single file
    if(!empty($_FILES['FILE']['tmp_name'])) {
        $arSingleFile = [
            'fileData' => [
                $_FILES['FILE']['name'], // filename
                base64_encode(file_get_contents($_FILES['FILE']['tmp_name'])) // file content, encoded in base64 
            ]
        ];
    }
    ```
{% endlist %}

#### Format Phone and Email

The system stores phone and email as an array of [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) objects, so they must be converted to an array format.

1. If a value exists, write it to the `VALUE` field, and pass the [type](../../../api-reference/crm/data-types.md#crm_multifield) in the `VALUE_TYPE` field, for example `WORK` for a phone number and `HOME` for an email address

2. If no value exists, pass an empty array

{% list tabs %}

- JS

    ```javascript
    // Format phone and email for Bitrix24 into crm_multifield format
    const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []
    const arEmail = sEmail ? [{ VALUE: sEmail, VALUE_TYPE: 'HOME' }] : []
    ```

- Python

    ```python
    # Format phone and email for Bitrix24 into crm_multifield format
    ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []
    ar_email = [{"VALUE": s_email, "VALUE_TYPE": "HOME"}] if s_email else []
    ```


- PHP

    ```php
    // Format phone and email for Bitrix24 into crm_multifield format
    $arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
    $arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();
    ```
{% endlist %}

#### Formulate the Lead Heading

Formulate the lead heading using the first and last name. For companies, add the company name to the heading.

{% list tabs %}

- JS

    ```javascript
    // Form the lead title from first and last name
    let sTitle = 'From website: ' + `${sName} ${sLastName}`.trim()
    // If there is a company name — add it via a hyphen after the first and last name
    if (sCompanyTitle) {
        sTitle += ' — ' + sCompanyTitle
    }
    ```

- Python

    ```python
    # Form the lead title from first and last name
    s_title = "From website: " + f"{s_name} {s_last_name}".strip()
    # If there is a company name — add it after the first and last name using a hyphen
    if s_company_title:
        s_title += " — " + s_company_title
    ```


- PHP

    ```php
    // Form the lead title from first and last name
    $sTitle = 'From website: ' . trim($sName . ' ' . $sLastName);
    // If there is a company name — add it via a hyphen after the first and last name
    if (!empty($sCompanyTitle)) {
        $sTitle .= ' — ' . $sCompanyTitle;
    }
    ```
{% endlist %}

### Create a Lead

To create a lead, use the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method. Pass the following fields in the `fields` object:

- `TITLE` — lead heading from the `$sTitle` variable

- `NAME` — first name from the `NAME` form field

- `LAST_NAME` — last name from the `LAST_NAME` form field

- `COMPANY_TITLE` — company name from the `COMPANY_TITLE` form field

- `PHONE` — phone number in the `crm_multifield` format from the `$arPhone` variable

- `EMAIL` — email address in the `crm_multifield` format from the `$arEmail` variable

- `UF_CRM_LEAD_FILES` — custom field for several files, an array of `fileData` objects from the `$arFiles` variable

- `UF_CRM_LEAD_FILE` — custom field for a single file, a `fileData` object from the `$arSingleFile` variable

Bitrix24 assigns custom fields names such as `UF_CRM_1711610801`, so replace `UF_CRM_LEAD_FILES` and `UF_CRM_LEAD_FILE` with your own. You can view them with the [crm.lead.userfield.list](../../../api-reference/crm/leads/userfield/crm-lead-userfield-list.md) method.

{% note warning "" %}

Check which required fields are configured for leads in your Bitrix24. All required fields must be passed to the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method.

{% endnote %}

{% list tabs %}

- JS

    ```javascript
    await $b24.actions.v2.call.make({
        method: 'crm.lead.add',
        params: {
            fields: {
                TITLE: sTitle, // Lead title
                NAME: sName, // First name
                LAST_NAME: sLastName, // Last name
                COMPANY_TITLE: sCompanyTitle, // Company name
                PHONE: arPhone, // Phone number
                EMAIL: arEmail, // Email
                UF_CRM_LEAD_FILES: arFiles, // Field for adding multiple files
                UF_CRM_LEAD_FILE: arSingleFile, // File field
            }
        },
        requestId: 'lead-add'
    })
    ```

- Python

    ```python
    client.crm.lead.add(fields={
        "TITLE": s_title,  # Lead title
        "NAME": s_name,  # First name
        "LAST_NAME": s_last_name,  # Last name
        "COMPANY_TITLE": s_company_title,  # Company name
        "PHONE": ar_phone,  # Phone number
        "EMAIL": ar_email,  # Email
        "UF_CRM_LEAD_FILES": ar_files,  # Field for adding multiple files
        "UF_CRM_LEAD_FILE": ar_single_file,  # File field
    })
    ```


- PHP

    ```php
    $sb->getCRMScope()->lead()->add([
        'TITLE' => $sTitle, // Lead title
        'NAME' => $sName, // First name
        'LAST_NAME' => $sLastName, // Last name
        'COMPANY_TITLE' => $sCompanyTitle, // Company name
        'PHONE' => $arPhone, // Phone number
        'EMAIL' => $arEmail, // Email
        'UF_CRM_LEAD_FILES' => $arFiles, // Field for adding multiple files
        'UF_CRM_LEAD_FILE' => $arSingleFile, // File field
    ]);
    ```
{% endlist %}

If the lead is created successfully, the method returns its identifier. Retain this value: you can use it to open the lead and verify the result.

```json
{
    "result": 5
}
```

### Full Handler Code Example

{% list tabs %}

- JS

    ```javascript
    import express from 'express'
    import multer from 'multer'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const app = express()
    // multer stores files in memory — available as Buffer in req.files
    const upload = multer({ storage: multer.memoryStorage() })

    // The handler accepts form data (multipart) via the /form route
    app.post('/form', upload.fields([{ name: 'FILE' }, { name: 'FILES' }]), async (req, res) => {
        // Get and sanitize data from the form
        const sName = String(req.body.NAME ?? '')
        const sLastName = String(req.body.LAST_NAME ?? '')
        const sCompanyTitle = String(req.body.COMPANY_TITLE ?? '')
        const sPhone = String(req.body.PHONE ?? '')
        const sEmail = String(req.body.EMAIL ?? '')

        // Create variables for arrays with files
        const arFiles = []
        let arSingleFile = []

        // Process the FILES field with multiple files
        for (const file of req.files?.FILES ?? []) {
            arFiles.push({
                fileData: [
                    file.originalname, // filename
                    file.buffer.toString('base64'), // file content, encoded in base64
                ]
            })
        }

        // Process the FILE field with a single file
        const single = req.files?.FILE?.[0]
        if (single) {
            arSingleFile = {
                fileData: [
                    single.originalname, // filename
                    single.buffer.toString('base64'), // file content, encoded in base64
                ]
            }
        }

        // Format phone and email for Bitrix24 into crm_multifield format
        const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []
        const arEmail = sEmail ? [{ VALUE: sEmail, VALUE_TYPE: 'HOME' }] : []

        // Form the lead title from first and last name
        let sTitle = 'From website: ' + `${sName} ${sLastName}`.trim()
        if (sCompanyTitle) {
            sTitle += ' — ' + sCompanyTitle
        }

        // Sending data to Bitrix24
        const response = await $b24.actions.v2.call.make({
            method: 'crm.lead.add',
            params: {
                fields: {
                    TITLE: sTitle, // Lead title
                    NAME: sName, // First name
                    LAST_NAME: sLastName, // Last name
                    COMPANY_TITLE: sCompanyTitle, // Company name
                    PHONE: arPhone, // Phone number
                    EMAIL: arEmail, // Email
                    UF_CRM_LEAD_FILES: arFiles, // Field for adding multiple files
                    UF_CRM_LEAD_FILE: arSingleFile, // File field
                }
            },
            requestId: 'lead-add'
        })

        // Check the result and display a message
        if (response.isSuccess && response.getData()?.result) {
            res.json({ message: 'Lead added successfully' })
        } else {
            res.json({ message: 'Lead not added: ' + response.getErrorMessages().join('; ') })
        }
    })

    app.listen(3000)
    ```

- Python

    ```python
    # pip install b24pysdk flask
    import base64
    import os
    from flask import Flask, request, jsonify
    from b24pysdk import BitrixWebhook, Client

    app = Flask(__name__)

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token=os.environ["B24_HOOK_TOKEN"],
    ))
    # B24_HOOK_TOKEN = 'USER_ID/TOKEN' — user_id and token only, without https://

    @app.route("/form", methods=["POST"])
    def handle_form():
        # Get data from the form
        s_name = request.form.get("NAME", "")
        s_last_name = request.form.get("LAST_NAME", "")
        s_company_title = request.form.get("COMPANY_TITLE", "")
        s_phone = request.form.get("PHONE", "")
        s_email = request.form.get("EMAIL", "")

        # Create variables for arrays with files
        ar_files = []
        ar_single_file = []

        # Process the FILES field with multiple files
        for file in request.files.getlist("FILES"):
            if file and file.filename:
                ar_files.append({
                    "fileData": [
                        file.filename,  # filename
                        base64.b64encode(file.read()).decode(),  # file content, encoded in base64
                    ]
                })

        # Process the FILE field with a single file
        single = request.files.get("FILE")
        if single and single.filename:
            ar_single_file = {
                "fileData": [
                    single.filename,  # filename
                    base64.b64encode(single.read()).decode(),  # file content, encoded in base64
                ]
            }

        # Format phone and email for Bitrix24 into crm_multifield format
        ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []
        ar_email = [{"VALUE": s_email, "VALUE_TYPE": "HOME"}] if s_email else []

        # Form the lead title from first and last name
        s_title = "From website: " + f"{s_name} {s_last_name}".strip()
        if s_company_title:
            s_title += " — " + s_company_title

        # Sending data to Bitrix24
        try:
            client.crm.lead.add(fields={
                "TITLE": s_title,  # Lead title
                "NAME": s_name,  # First name
                "LAST_NAME": s_last_name,  # Last name
                "COMPANY_TITLE": s_company_title,  # Company name
                "PHONE": ar_phone,  # Phone number
                "EMAIL": ar_email,  # Email
                "UF_CRM_LEAD_FILES": ar_files,  # Field for adding multiple files
                "UF_CRM_LEAD_FILE": ar_single_file,  # File field
            })
            return jsonify({"message": "Lead added successfully"})
        except Exception as e:
            return jsonify({"message": f"Lead not added: {e}"})
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
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // Get and sanitize data from the form
    $sName = htmlspecialchars($_POST["NAME"]);
    $sLastName = htmlspecialchars($_POST["LAST_NAME"]);
    $sCompanyTitle = htmlspecialchars($_POST["COMPANY_TITLE"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    $sEmail = htmlspecialchars($_POST["EMAIL"]);

    // Create variables for arrays with files
    $arFiles = [];
    $arSingleFile = [];

    // Process the FILES field with multiple files
    if (!empty($_FILES['FILES']['tmp_name'])) {
        foreach ($_FILES['FILES']['tmp_name'] as $key => $tmpName) {
            if (!empty($tmpName)) {
                $arFiles[] = [
                    'fileData' => [
                        $_FILES['FILES']['name'][$key], // filename
                        base64_encode(file_get_contents($tmpName)) // file content, encoded in base64
                    ]
                ];
            }
        }
    }

    // Process the FILE field with a single file
    if (!empty($_FILES['FILE']['tmp_name'])) {
        $arSingleFile = [
            'fileData' => [
                $_FILES['FILE']['name'], // filename
                base64_encode(file_get_contents($_FILES['FILE']['tmp_name'])) // file content, encoded in base64
            ]
        ];
    }

    // Format phone and email for Bitrix24 into crm_multifield format
    $arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
    $arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();

    // Form the lead title from first and last name
    $sTitle = 'From website: ' . trim($sName . ' ' . $sLastName);
    if (!empty($sCompanyTitle)) {
        $sTitle .= ' — ' . $sCompanyTitle;
    }

    // Sending data to Bitrix24
    try {
        $sb->getCRMScope()->lead()->add([
            'TITLE' => $sTitle, // Lead title
            'NAME' => $sName, // First name
            'LAST_NAME' => $sLastName, // Last name
            'COMPANY_TITLE' => $sCompanyTitle, // Company name
            'PHONE' => $arPhone, // Phone number
            'EMAIL' => $arEmail, // Email
            'UF_CRM_LEAD_FILES' => $arFiles, // Field for adding multiple files
            'UF_CRM_LEAD_FILE' => $arSingleFile, // File field
        ]);

        echo json_encode(['message' => 'Lead added successfully']);
    } catch (\Throwable $e) {
        echo json_encode(['message' => 'Lead not added: ' . $e->getMessage()]);
    }
    ```
{% endlist %}

## Verify the Result

Open the created lead in Bitrix24. In the lead card, the custom fields for files show the attachments as links — the files can be downloaded.

Through REST, the lead is checked with the [crm.lead.get](../../../api-reference/crm/leads/crm-lead-get.md) method using the identifier from the response of the previous step.

{% list tabs %}

- JS

    ```javascript
    const checkResponse = await $b24.actions.v2.call.make({
        method: 'crm.lead.get',
        params: { id: 5 },
        requestId: 'lead-get'
    })

    console.dir(checkResponse.getData().result)
    ```

- Python

    ```python
    lead = client.crm.lead.get(bitrix_id=5).result
    ```


- PHP

    ```php
    $lead = $sb->getCRMScope()->lead()->get(5)->lead();
    ```
{% endlist %}

The scenario is complete if the response has:

- `TITLE` starting with `From website:` — the lead came from the form

- `PHONE` and `EMAIL` matching what the form submitted

- the `UF_CRM_LEAD_FILE` and `UF_CRM_LEAD_FILES` fields filled in. The method returns in them not the Base64 string itself but the data of the uploaded file: `id`, `showUrl`, and `downloadUrl`. In the single field this is one object, in the multiple field it is an array of objects

```json
{
    "result": {
        "ID": "5",
        "TITLE": "From website: Klaus Weber",
        "UF_CRM_LEAD_FILE": {
            "id": 37375,
            "showUrl": "/bitrix/components/bitrix/crm.lead.show/show_file.php?ownerId=5&fieldName=UF_CRM_LEAD_FILE&dynamic=Y&fileId=37375",
            "downloadUrl": "/bitrix/components/bitrix/crm.lead.show/show_file.php?auth=&ownerId=5&fieldName=UF_CRM_LEAD_FILE&dynamic=Y&fileId=37375"
        },
        "UF_CRM_LEAD_FILES": [
            {
                "id": 37377,
                "showUrl": "/bitrix/components/bitrix/crm.lead.show/show_file.php?ownerId=5&fieldName=UF_CRM_LEAD_FILES&dynamic=Y&fileId=37377",
                "downloadUrl": "/bitrix/components/bitrix/crm.lead.show/show_file.php?auth=&ownerId=5&fieldName=UF_CRM_LEAD_FILES&dynamic=Y&fileId=37377"
            }
        ]
    }
}
```

If the client did not attach any files, the multiple field is returned as an empty array, and the single field does not appear in the response at all. This is also a correct result.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| Empty value `Access denied` | The user does not have permission to create leads. Check which user the webhook was created on behalf of ||
|#

A lead may be created without an error but without files. The [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method does not report problems with files: it skips both an unknown field name and a value in an unsuitable format. Check the following in order:

- The custom field name in the code does not match the name in Bitrix24. The method ignores an unknown field, and the lead is created without an error

- The form has no `enctype="multipart/form-data"` attribute. In that case the browser sends only the file names, and the handler does not receive the content

- `multer` is not connected in the JS handler, or the `FILE` and `FILES` fields are not listed in it. Without it, `req.files` remains empty

- An array of `fileData` objects was passed to the single field. The method does not retain such a value at all — the single field takes one object

- The request exceeded the size limit. The Base64 string is about a third longer than the original file, so check against the string size, not the file size. For more details, read the article [How to Upload Files](../../../api-reference/files/how-to-upload-files.md)

Retrieving the list of fields and retrieving the lead do not create anything, so they can be executed any number of times. If [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) returned the error, the lead was not created: fix the `fields` and repeat only that call.

## Key Considerations

- The field for several files must be created with the "multiple" flag. The reverse substitution is safe: one `fileData` object in a multiple field is retained as an array of one file

- Submitting the form again with the same data creates a new lead every time. Duplicates are not filtered out. To link repeat requests, use the [{#T}](./how-to-add-repeat-lead.md) scenario

- Files go into the request in full, so large attachments increase the handler response time. If there are many files, transfer them in separate requests

## Continue Learning

- [{#T}](../../../api-reference/crm/leads/crm-lead-add.md)
- [{#T}](../../../api-reference/crm/leads/crm-lead-get.md)
- [{#T}](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md)
- [{#T}](../../../api-reference/files/index.md)
- [{#T}](../../../api-reference/files/how-to-upload-files.md)
- [{#T}](../../../api-reference/crm/data-types.md)
