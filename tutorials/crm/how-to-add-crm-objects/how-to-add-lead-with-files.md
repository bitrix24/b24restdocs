# Add Lead with Files via Web Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to create leads in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can place a form on your website to collect data from potential clients. When a client fills out the form and attaches files, their data will be sent to the CRM, allowing you to process the request.

Setting up the form consists of two steps.

1. Place the form on an HTML page. It will send data to the handler.

2. Create a file to process the data. The handler will accept and prepare the data, then create a lead using the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add) method.

## 1. Creating the Web Form

In Bitrix24, you can automatically create a contact and a company from a lead. To make the form suitable for various cases, we will make it universal. For a contact, you need to specify the first name and last name, and for a company, the name. We will create a web form on the website page with the following fields:

-  `NAME` — first name, required,

-  `LAST_NAME` — last name,

-  `COMPANY_TITLE` — company name,

-  `EMAIL` — email address,

-  `PHONE` — phone number.

To allow clients to upload files, we will add the following fields to the form:

-  `FILE` — for a single file,

-  `FILES` — for uploading multiple files.

When submitted, the form sends data to the handler `form.php`.

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
    <!-- Field for a single file -->
    <input type="file" name="FILE">
    <!-- Field for multiple files -->
    <input type="file" name="FILES" multiple>

    <!-- Submit button -->
    <input type="submit" value="Submit">
</form>

<!-- Include jQuery for AJAX request -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script>
	$(document).ready(function() {
	    $('#form_to_crm').on('submit', function(el) {
	        el.preventDefault();
	        var formData = new FormData(this); // Collect form data with files
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
	                alert('Error submitting the form');
	            }
	        });
	    });
	});
</script>
```

## 2. Creating the Form Handler

To process the values from the form fields and add a lead to the CRM, we will create the handler `form.php`.

### Preparing Data from the Form

To use the data from the form in the lead creation method, we need to prepare it.

#### Cleaning HTML Tags

We will retrieve and clean the data from the form.

```php
// Retrieve and clean data from the form
$sName = htmlspecialchars($_POST["NAME"]);
$sLastName = htmlspecialchars($_POST["LAST_NAME"]);
$sCompanyTitle = htmlspecialchars($_POST["COMPANY_TITLE"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);
$sEmail = htmlspecialchars($_POST["EMAIL"]);
```

#### Preparing Files

We will prepare the files for upload to Bitrix24. For each file, we need to pass an array:

-  with the file name,

-  a string with the file encoded in Base64.

To encode the file, we will use the [base64_encode](https://www.php.net/manual/en/function.base64-encode.php) function.

{% note tip "Documentation" %}

-  [How to Work with Files](../../../api-reference/files/index.md)

{% endnote %}

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
                    $_FILES['FILES']['name'][$key], // file name
                    base64_encode(file_get_contents($tmpName)) // file content encoded in base64 
                ]
            ];
        }
    }
}

// Process the FILE field with a single file
if(!empty($_FILES['FILE']['tmp_name'])) {
    $arSingleFile = [
        'fileData' => [
            $_FILES['FILE']['name'], // file name
            base64_encode(file_get_contents($_FILES['FILE']['tmp_name'])) // file content encoded in base64 
        ]
    ];
}
```

#### Formatting Phone and Email

The system stores phone numbers and emails as an array of objects [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield), so they need to be formatted as an array.

1. If a value exists, we add it as the first element `VALUE` in the array, and the second value indicates the type `VALUE_TYPE`, for example:

   -  `WORK` — for phone,

   -  `HOME` — for email.

2. If there is no value, we pass an empty array.

```php
// Format phone and email for Bitrix24 in crm_multifield format
$arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
$arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();
```

#### Forming the Lead Title

We will form the lead title from the first name and last name. For companies, we will add the company name to the title.

```php
// Form the lead title from the first name and last name
$sTitle = 'From website: ' . trim($sName . ' ' . $sLastName);
// If there is a company name, add it after the first name and last name with a dash
if (!empty($sCompanyTitle)) {
    $sTitle .= ' — ' . $sCompanyTitle;
}
```

### Creating the Lead

To create a lead, we will use the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method. In the `fields` object, we will pass the fields:

-  `TITLE` — lead title,

-  `NAME` — lead first name,

-  `LAST_NAME` — lead last name,

-  `COMPANY_TITLE` — company name,

-  `PHONE` — phone number,

-  `EMAIL` — email address,

-  `UF_CRM_LEAD_FILES` — custom field for adding multiple files,

-  `UF_CRM_LEAD_FILE` — custom field for a single file.

Custom fields `UF_CRM_*` need to be created in Bitrix24 before creating the lead. Add them on the account manually or using the [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md) method. In the example, replace `UF_CRM_LEAD_FILES` and `UF_CRM_LEAD_FILE` with your field names.

{% note warning "" %}

Check which required fields are configured for leads in your Bitrix24. All required fields must be passed to the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method.

{% endnote %}

```php
CRest::call(
    'crm.lead.add',
    [
        'fields' => [
            'TITLE' => $sTitle, // Lead title
            'NAME' => $sName, // First Name
            'LAST_NAME' => $sLastName, // Last Name
            'COMPANY_TITLE' => $sCompanyTitle, // Company Name
            'PHONE' => $arPhone, // Phone
            'EMAIL' => $arEmail, // Email
            'UF_CRM_LEAD_FILES' => $arFiles, // Field for adding multiple files
            'UF_CRM_LEAD_FILE' => $arSingleFile // Field for a single file
        ]
    ]
);
```

As a result, we will receive the identifier of the new lead `5`.

```json
{
	"result": 5
}
```

### Complete Example of the Handler Code

```php
<?php
require_once('crest.php');

// Retrieve and clean data from the form
$sName = htmlspecialchars($_POST["NAME"]);
$sLastName = htmlspecialchars($_POST["LAST_NAME"]);
$sCompanyTitle = htmlspecialchars($_POST["COMPANY_TITLE"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);
$sEmail = htmlspecialchars($_POST["EMAIL"]);

// Create variables for arrays with files
$arFiles = [];
$arSingleFile = [];

// Process the FILES field with multiple files
if(!empty($_FILES['FILES']['tmp_name'])) {
    foreach($_FILES['FILES']['tmp_name'] as $key => $tmpName) {
        if(!empty($tmpName)) {
            $arFiles[] = [
                'fileData' => [
                    $_FILES['FILES']['name'][$key],
                    base64_encode(file_get_contents($tmpName))
                ]
            ];
        }
    }
}

// Process the FILE field with a single file
if(!empty($_FILES['FILE']['tmp_name'])) {
    $arSingleFile = [
        'fileData' => [
            $_FILES['FILE']['name'],
            base64_encode(file_get_contents($_FILES['FILE']['tmp_name']))
        ]
    ];
}

// Format phone and email for Bitrix24 in crm_multifield format
$arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
$arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();

// Form the lead title from the first name and last name
$sTitle = 'From website: ' . trim($sName . ' ' . $sLastName);
// If there is a company name, add it after the first name and last name with a dash
if (!empty($sCompanyTitle)) {
    $sTitle .= ' — ' . $sCompanyTitle;
}

// Send data to Bitrix24
$result = CRest::call(
    'crm.lead.add',
    [
        'fields' => [
            'TITLE' => $sTitle, // Lead title
            'NAME' => $sName, // First Name
            'LAST_NAME' => $sLastName, // Last Name
            'COMPANY_TITLE' => $sCompanyTitle, // Company Name
            'PHONE' => $arPhone, // Phone
            'EMAIL' => $arEmail, // Email
            'UF_CRM_LEAD_FILES' => $arFiles, // Field for adding multiple files
            'UF_CRM_LEAD_FILE' => $arSingleFile // Field for a single file
        ]
    ]
);

// Check the result and output a message
if(!empty($result['result'])){
    echo json_encode(['message' => 'Lead added successfully']);
} elseif(!empty($result['error_description'])){
    echo json_encode(['message' => 'Lead not added: '.$result['error_description']]);
} else {
    echo json_encode(['message' => 'Lead not added']);
}
?>
```