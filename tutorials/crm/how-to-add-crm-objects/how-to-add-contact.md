# Add Contact via Web Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to create contacts in CRM

You can place a form on the site to collect client data. When a client fills out the form, their data will be sent to CRM, and you will be able to process the request.

Setting up the form consists of two steps.

1. Place the form on an HTML page. It will send the data to the handler.

2. Create a file to process the data. The handler will accept and prepare the data, and then create a contact using the method [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md).

## 1. Creating the Web Form

Let's create a web form on the site page with four fields:

-  `NAME` — contact's first name, required,

-  `LAST_NAME` — last name,

-  `EMAIL` — email address,

-  `PHONE` — phone number.

When submitted, the form sends data to the handler `form.php`.

```html
<form id="form_to_crm" method="POST" action="form.php">
    
	<!-- First name, required field -->
    <input type="text" name="NAME" placeholder="First Name" required>

	<!-- Last name --> 
    <input type="text" name="LAST_NAME" placeholder="Last Name">

	<!-- Email --> 
    <input type="text" name="EMAIL" placeholder="Email">

	<!-- Phone -->
    <input type="text" name="PHONE" placeholder="Phone">

	<!-- Submit button --> 
    <input type="submit" value="Submit"> 
</form>

<!-- Include jQuery for AJAX request -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script>
    $(document).ready(function() {
        $('#form_to_crm').on('submit', function(el) {
            el.preventDefault(); // Prevent default form submission
            var formData = $(this).serialize(); // Gather form data
            
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

To process the values from the form fields and add a contact to CRM, we will create the handler `form.php`.

To add a contact, we will use the method [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md). In the `fields` object, we will pass the fields:

-  `NAME` — contact's first name,

-  `LAST_NAME` — last name,

-  `PHONE` — phone number,

-  `EMAIL` — email address.

The values of the fields are obtained from the form. The system stores phone and email as an array of objects [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield), so they need to be formatted as an array.

1. If a value exists, we add it as the first element `VALUE` in the array, and the second value specifies the type `VALUE_TYPE`, for example:

   -  `WORK` — for phone,

   -  `HOME` — for email.

2. If there is no value, we pass an empty array.

{% note warning "" %}

Check which required fields are set for contacts in your Bitrix24. All required fields must be passed to the method [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md).

{% endnote %}

```php
<?php
require_once('crest.php');
            
// Get and sanitize data from the form
$sName = htmlspecialchars($_POST["NAME"]);
$sLastName = htmlspecialchars($_POST["LAST_NAME"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);
$sEmail = htmlspecialchars($_POST["EMAIL"]);                 

// Format phone and email for Bitrix24 in crm_multifield format
$arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
$arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();

// Send data to Bitrix24
$result = CRest::call(
    'crm.contact.add',
    [
        'fields' => [
            'NAME' => $sName, // First Name
            'LAST_NAME' => $sLastName, // Last Name
            'PHONE' => $arPhone, // Phone
            'EMAIL' => $arEmail, // Email
        ]
    ]
);

// Check the result and output the message
if(!empty($result['result'])){
    echo json_encode(['message' => 'Contact added']);
} elseif(!empty($result['error_description'])){
    echo json_encode(['message' => 'Contact not added: '.$result['error_description']]);
} else {
    echo json_encode(['message' => 'Contact not added']);
}
?>
```