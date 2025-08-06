# Add a lead with files via a web form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with the permission to create leads in CRM

You can place a form on the site to collect data from potential clients. When a client fills out the form and attaches files, their data will be sent to CRM, and you will be able to process the request.

Setting up the form consists of two steps.

1. Place the form on an HTML page. It will send the data to the handler.

2. Create a file to process the data. The handler will accept and prepare the data, and then create a lead using the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add).

## 1. Creating the web form

In Bitrix24, you can automatically create a contact and a company from a lead. To make the form suitable for various cases, we will make it universal. For the contact, you need to specify the first name and last name, and for the company — the name. We will create a web form on the site page with the following fields:

-  `NAME` — first name, required,

-  `LAST_NAME` — last name,

-  `COMPANY_TITLE` — company name,

-  `EMAIL` — e-mail,

-  `PHONE` — phone.

To allow the client to upload files, we will add the following fields to the form:

-  `FILE` — for one file,

-  `FILES` — for adding multiple files.

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
    <!-- Field for one file -->
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
	                alert('Error while submitting the form');
	            }
	        });
	    });
	});
</script>
```

## 2. Creating the form handler

To process the values from the form fields and add a lead to CRM, we will create the handler `form.php`.

### Preparing data from the form

To use the data from the form in the lead creation method, we need to prepare it.

#### Cleaning from HTML tags

We will retrieve and clean the data from the form of HTML tags.

```php
// Retrieve and clean data from the form
$sName = htmlspecialchars($_POST["NAME"]);
$sLastName = htmlspecialchars($_POST["LAST_NAME"]);
$sCompanyTitle = htmlspecialchars($_POST["COMPANY_TITLE"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);
$sEmail = htmlspecialchars($_POST["EMAIL"]);
```

#### Preparing files

We will prepare the files for upload to Bitrix24. For each file, we need to pass an array:

-  with the file name,

-  a string with the file encoded in Base64.

To encode the file, we will use the function [base64_encode](https://www.php.net/manual/en/function.base64-encode.php).

{% note tip "Documentation" %}

-  [How to work with files](../../../api-reference/files/index.md)

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

// Process the FILE field with one file
if(!empty($_FILES['FILE']['tmp_name'])) {
    $arSingleFile = [
        'fileData' => [
            $_FILES['FILE']['name'], // file name
            base64_encode(file_get_contents($_FILES['FILE']['tmp_name'])) // file content encoded in base64 
        ]
    ];
}
```

#### Formatting phone and email

The phone and email are stored in the system as an array of objects [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield), so they need to be formatted as an array.

1. If the value exists, we add it as the first element `VALUE` in the array, and the second value is the type `VALUE_TYPE`, for example:

   -  `WORK` — for phone,

   -  `HOME` — for email.

2. If the value does not exist, we pass an empty array.

```php
// Format phone and email for Bitrix24 in crm_multifield format
$arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
$arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();
```

#### Forming the lead title

We will form the lead title from the first name and last name. For companies, we will add the company name to the title.

```php
// Form the lead title from the first name and last name
$sTitle = 'From the site: ' . trim($sName . ' ' . $sLastName);
// If there is a company name — add it with a dash after the first name and last name
if (!empty($sCompanyTitle)) {
    $sTitle .= ' — ' . $sCompanyTitle;
}
```

### Creating the lead

To create a lead, we will use the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md). In the `fields` object, we will pass the fields:

-  `TITLE` — lead title,

-  `NAME` — lead first name,

-  `LAST_NAME` — lead last name,

-  `COMPANY_TITLE` — company name,

-  `PHONE` — phone number,

-  `EMAIL` — e-mail,

-  `UF_CRM_LEAD_FILES` — custom field for adding multiple files,

-  `UF_CRM_LEAD_FILE` — custom field for a file.

Custom fields `UF_CRM_*` need to be created in Bitrix24 before creating the lead. Add them on the account manually or using the method [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md). In the example, replace `UF_CRM_LEAD_FILES` and `UF_CRM_LEAD_FILE` with your field names.

{% note warning "" %}

Check which required fields are set for leads in your Bitrix24. All required fields must be passed to the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md).

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
            'UF_CRM_LEAD_FILE' => $arSingleFile // Field for a file
        ]
    ]
);
```

As a result, we will get the identifier of the new lead `5`.

```json
{
	"result": 5
}
```

### Full example of the handler code

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

// Process the FILE field with one file
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
$sTitle = 'From the site: ' . trim($sName . ' ' . $sLastName);
// If there is a company name — add it with a dash after the first name and last name
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
            'UF_CRM_LEAD_FILE' => $arSingleFile // Field for a file
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