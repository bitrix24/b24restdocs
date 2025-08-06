# Add Lead via Web Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with the permission to create leads in CRM

You can place a form on your site to collect potential client data. When a client fills out the form, their information will be sent to CRM, and you will be able to process the request.

Setting up the form consists of two steps.

1. Place the form on an HTML page. It will send data to the handler.

2. Create a file to process the data. The handler will accept and prepare the data, and then create a lead using the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md).

## 1. Creating the Web Form

In Bitrix24, you can automatically create a contact and company from a lead. To make the form suitable for various cases, we will make it universal. For the contact, you need to specify the first name and last name, and for the company — the name. We will create a web form on the site page with five fields:

- `NAME` — first name, required,

- `LAST_NAME` — last name,

- `COMPANY_TITLE` — company name,

- `EMAIL` — e-mail,

- `PHONE` — phone.

When submitted, the form sends data to the handler `form.php`.

```html
<form id="form_to_crm" method="POST" action="form.php">
    <!-- First Name (required field) -->
    <input type="text" name="NAME" placeholder="First Name" required>
    <!-- Last Name -->
    <input type="text" name="LAST_NAME" placeholder="Last Name">
    <!-- Company Name -->
    <input type="text" name="COMPANY_TITLE" placeholder="Company Name">
    <!-- Email -->
    <input type="text" name="EMAIL" placeholder="E-mail">
    <!-- Phone -->
    <input type="text" name="PHONE" placeholder="Phone">
    <!-- Submit Button -->
    <input type="submit" value="Submit">
</form>

<!-- Include jQuery for AJAX request -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script>
    $(document).ready(function() {
        $('#form_to_crm').on('submit', function(el) {
            el.preventDefault(); // Prevent default form submission
            var formData = $(this).serialize(); // Collect form data
            // Send data to the server
            $.ajax({
                'method': 'POST',
                'dataType': 'json',
                'url': 'form.php', // Handler file
                'data': formData,
                success: function(data) {
                    alert(data.message); // Show result
                }
            });
        });
    });
</script>
```

## 2. Creating the Form Handler

To process the values from the form fields and add a lead to CRM, we will create the handler `form.php`.

To add a lead, we will use the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md). In the `fields` object, we will pass the fields:

- `TITLE` — lead title, which can be composed of the first name and last name,

- `NAME` — lead's first name,

- `LAST_NAME` — lead's last name,

- `COMPANY_TITLE` — company name,

- `PHONE` — phone number,

- `EMAIL` — e-mail.

The values for the fields are obtained from the form. The system stores the phone and email as an array of objects [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield), so they need to be formatted as an array.

1. If a value exists, we add it as the first element `VALUE` in the array, and the second value specifies the type `VALUE_TYPE`, for example:

   -  `WORK` — for phone,

   -  `HOME` — for email.

2. If no values exist, we pass an empty array.

{% note warning "" %}

Check which required fields are set for leads in your Bitrix24. All required fields must be passed to the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md).

{% endnote %}

```php
<?php
require_once('crest.php');

// Get and sanitize data from the form
$sName = htmlspecialchars($_POST["NAME"]);
$sLastName = htmlspecialchars($_POST["LAST_NAME"]);
$sCompanyTitle = htmlspecialchars($_POST["COMPANY_TITLE"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);
$sEmail = htmlspecialchars($_POST["EMAIL"]);

// Format phone and email for Bitrix24 in crm_multifield format
$arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
$arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();

// Create lead title from first name and last name
$sTitle = 'From website: ' . trim($sName . ' ' . $sLastName);
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
        ]
    ]
);

// Check the result and output the message
if(!empty($result['result'])){
    echo json_encode(['message' => 'Lead added']);
} elseif(!empty($result['error_description'])){
    echo json_encode(['message' => 'Lead not added: '.$result['error_description']]);
} else {
    echo json_encode(['message' => 'Lead not added']);
}
?>
```