# Add Lead with Files via Web Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to create leads in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can place a form on your website to collect data from potential clients. When a client fills out the form and attaches files, their data will be sent to the CRM, allowing you to process the request.

Setting up the form consists of two steps.

1. Place the form on an HTML page. It will send data to the handler.

2. Create a file to process the data. The handler will receive and prepare the data, and then create a lead using the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method.

## 1. Creating the Web Form

In Bitrix24, a contact and a company can be automatically created from a lead. To make the form suitable for different scenarios, we will make it universal. For a contact, a first name and last name must be specified, and for a company, a name is required. We will create a web form on a website page with the following fields:

-  `NAME` — First Name, required,

-  `LAST_NAME` — Last Name,

-  `COMPANY_TITLE` — Company Name,

-  `EMAIL` — Email,

-  `PHONE` — Phone.

To allow the customer to upload files, we will add the following fields to the form:

-  `FILE` — for a single file,

-  `FILES` — for adding multiple files.

Upon submission, the form passes the data to the handler.

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

{% endlist %}

## 2. Create a Form Handler

To process values from form fields and add a lead to the CRM, we will create a handler.

### Prepare Form Data

To use data from the form in the lead creation method, you must prepare it.

#### Strip HTML Tags

Retrieve the form data and strip HTML tags.

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

- PHP

    ```php
    // Get and sanitize data from the form
    $sName = htmlspecialchars($_POST["NAME"]);
    $sLastName = htmlspecialchars($_POST["LAST_NAME"]);
    $sCompanyTitle = htmlspecialchars($_POST["COMPANY_TITLE"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    $sEmail = htmlspecialchars($_POST["EMAIL"]);
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

{% endlist %}

#### Prepare Files

Prepare files for upload to Bitrix24. For each file, you must pass an array containing:

- the file name,
- a string containing the file encoded in Base64.

To encode a file, use the [base64_encode](https://www.php.net/manual/en/function.base64-encode.php) function.

{% note tip "Documentation" %}

- [How to Work with Files](../../../api-reference/files/index.md)

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

{% endlist %}

#### Format Phone and Email

The system stores phone and email as an array of [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) objects, so they must be converted to an array format.

1. If a value exists, add it as the first item `VALUE` in the array, and specify the type `VALUE_TYPE` as the second value, for example:

   - `WORK` — for phone,
   - `HOME` — for email.

2. If no value exists, pass an empty array.

{% list tabs %}

- JS

    ```javascript
    // Format phone and email for Bitrix24 into crm_multifield format
    const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []
    const arEmail = sEmail ? [{ VALUE: sEmail, VALUE_TYPE: 'HOME' }] : []
    ```

- PHP

    ```php
    // Format phone and email for Bitrix24 into crm_multifield format
    $arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
    $arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();
    ```

- Python

    ```python
    # Format phone and email for Bitrix24 into crm_multifield format
    ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []
    ar_email = [{"VALUE": s_email, "VALUE_TYPE": "HOME"}] if s_email else []
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

- PHP

    ```php
    // Form the lead title from first and last name
    $sTitle = 'From website: ' . trim($sName . ' ' . $sLastName);
    // If there is a company name — add it via a hyphen after the first and last name
    if (!empty($sCompanyTitle)) {
        $sTitle .= ' — ' . $sCompanyTitle;
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

{% endlist %}

### Create a Lead

To create a lead, use the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method. Pass the following fields in the `fields` object:

- `TITLE` — lead heading,
- `NAME` — lead first name,
- `LAST_NAME` — last name,
- `COMPANY_TITLE` — company name,
- `PHONE` — phone number,
- `EMAIL` — Email,
- `UF_CRM_LEAD_FILES` — custom field for adding multiple files,
- `UF_CRM_LEAD_FILE` — custom field for a file.

Custom fields `UF_CRM_*` must be created in Bitrix24 before creating the lead. Add them to the portal manually or via the [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md) method. In the example, replace `UF_CRM_LEAD_FILES` and `UF_CRM_LEAD_FILE` with your own field names.

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

{% endlist %}

As a result, you will receive the identifier of the new lead `5`.

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

- Python

    ```python
    # pip install b24pysdk
    import base64
    from flask import Flask, request, jsonify
    from b24pysdk import BitrixWebhook, Client

    app = Flask(__name__)

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",  # user_id/token only, without https://
    ))

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

{% endlist %}
