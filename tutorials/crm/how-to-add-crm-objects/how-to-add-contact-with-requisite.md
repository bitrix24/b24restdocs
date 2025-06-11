# Add a contact with details via a web form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to create contacts in CRM

You can place a form on the site to collect client data and details. When a client fills out the form, their data will be sent to CRM, and you will be able to process the request.

Setting up the form consists of two steps.

1. We will place the form on a PHP page. In the page code, we will retrieve the list of detail templates and address fields for the form. The form data will be sent to the handler.

2. We will create a file to process the data. The handler will accept and prepare the data, and then create a contact with the details.

## 1. Creating the web form

To generate the form fields, we will use data from Bitrix24. To get information about the detail settings, we will sequentially execute two methods:

1. [crm.address.fields](../../../api-reference/crm/requisites/addresses/crm-address-fields.md) — we retrieve the list of address fields. The result is saved in `arAddressFields`.

   ```php
   $arAddressFields = CRest::call('crm.address.fields', []);
   ```

2. [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) — we request the list of detail templates. Using the `select` parameter, we choose the `ID` and `NAME` fields for each template. The result is saved in `arRequisiteType`.

   ```php
   $arRequisiteType = CRest::call(
       'crm.requisite.preset.list', [
           'select' => ["ID", "NAME"]
       ]
   );
   ```

We will add a web form to the site page with the following fields:

-  `REQ_TYPE` — a dropdown list with the type of details from the `arRequisiteType` array, required,

-  `NAME` — contact's first name, required,

-  `LAST_NAME` — last name,

-  `PHONE` — phone number,

-  `${addressFieldsInputs}` — address fields, which are created dynamically from the `arAddressFields` array.

The form sends data using the `POST` method to the `form.php` file.

### Full example of the page code with the form

{% include [Example notes](../../../_includes/examples.md) %}

```php
<?php
// Retrieve the list of address fields and detail templates
$arAddressFields = CRest::call('crm.address.fields', []);
$arRequisiteType = CRest::call('crm.requisite.preset.list', [
    'select' => ["ID", "NAME"]
]);

if (!empty($arRequisiteType['result'])): 
    $arRequisiteType = array_column($arRequisiteType['result'], 'NAME', 'ID');

    // Remove system and unused address fields
    $excludeFields = ['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID'];
    foreach ($excludeFields as $field) {
        unset($arAddressFields['result'][$field]);
    }
?>
    <form id="form_to_crm">
        <select name="REQ_TYPE" required>
            <option value="" disabled selected>Select the type of details</option>
            <?php foreach ($arRequisiteType as $id => $name): ?>
                <option value="<?=$id?>"><?=$name?></option>
            <?php endforeach; ?>
        </select>
        <input type="text" name="NAME" placeholder="First Name" required>
        <input type="text" name="LAST_NAME" placeholder="Last Name">
        <input type="text" name="PHONE" placeholder="Phone">
        <?php foreach ($arAddressFields['result'] as $key => $arField): ?>
            <input type="text" name="ADDRESS[<?=$key?>]" 
                   placeholder="<?=$arField['title']?>" 
                   <?=$arField['isRequired'] ? 'required' : ''?>>
        <?php endforeach; ?>
        <input type="submit" value="Submit">
    </form>
<?php else: ?>
    <p>No available types of details.</p>
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

## 2. Creating the form handler

To process the values from the form fields and add a contact to CRM, we will create a handler `form.php`.

### Preparing the data

We retrieve and clean the data from the form:

-  `REQ_TYPE` is converted to a number,

-  `NAME`, `LAST_NAME`, `PHONE` are cleaned of HTML tags.

```php
$iRequisitePresetID = intVal($_POST["REQ_TYPE"]);
$sName = htmlspecialchars($_POST["NAME"]);
$sLastName = htmlspecialchars($_POST["LAST_NAME"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);
```

We prepare the address fields and collect them into the `$arAddress` array.

-  The values of the fields from the form are cleaned of HTML tags.

-  We add the address type `TYPE_ID`. The address types can be obtained using the method [crm.enum.addresstype](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md). We will specify the value — `1`, which means the actual address.

-  We add the identifier of the [object type](../../../api-reference/crm/data-types.md#object_type) `ENTITY_TYPE_ID`. The identifiers can be obtained using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md). We will specify the value — `8`, which means detail.

```php
$arAddress = [];
foreach($_POST["ADDRESS"] as $key => $val) {
    $arAddress[$key] = htmlspecialchars($val);
}
$arAddress['TYPE_ID'] = 1;
$arAddress['ENTITY_TYPE_ID'] = 8;
```

The system stores the phone as an array of objects [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield), so it needs to be formatted as an array.

1. We add the phone as the first element `VALUE` in the array, and the second value specifies the type `VALUE_TYPE`, for example, `WORK`.

2. For an empty value, we pass an empty array.

```php
$arPhone = !empty($sPhone) ? [['VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK']] : [];
```

### Adding the contact

To create a contact, we will execute the method [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md). In the `fields` object, we pass the fields:

-  `NAME` — contact's first name,

-  `LAST_NAME` — last name,

-  `PHONE` — phone.

{% note warning "" %}

Check which required fields are set for contacts in your Bitrix24. All required fields must be passed to the method [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md).

{% endnote %}

```php
CRest::call(
    'crm.contact.add', [
        'fields' => [
            'NAME' => $sName,
            'LAST_NAME' => $sLastName,
            'PHONE' => $arPhone
        ]
    ]
);
```

As a result, we will receive the identifier of the new contact, for example, `23`.

```json
{
	"result": 23
}
```

### Adding details to the contact

To add details to the contact, we will execute the method [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md). In the `fields` object, we pass the fields:

-  `ENTITY_TYPE_ID` — identifier of the [object type](../../../api-reference/crm/data-types.md#object_type). The identifiers can be obtained using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md). In the example, we will specify the value `3`, which means contact,

-  `ENTITY_ID` — identifier of the contact received in the previous request,

-  `PRESET_ID` — identifier of the detail template received from the form,

-  `ACTIVE` — activity of the detail `Y`,

-  `NAME` — name of the detail, for example, we will combine the first name and last name of the contact.

```php
CRest::call(
    'crm.requisite.add', 
    [
        'fields' => [
            'ENTITY_TYPE_ID' => 3,
            'ENTITY_ID' => $contactId,
            'PRESET_ID' => $iRequisitePresetID,
            'ACTIVE' => 'Y',
            'NAME' => implode(' ', [$sName, $sLastName]),
        ]
    ]
);
```

As a result, we will receive the identifier of the details.

```json
{
    "result": 34
}
```

### Adding an address for the detail

We will add an address for the detail using the method [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md), if the detail was created successfully. In `$arAddress`, we add `ENTITY_ID` with the `ID` of the detail from the response of the previous request. In the `fields` object, we pass the array `$arAddress` with the address fields.

```php
if(!empty($resultRequisite['result'])) {
	$arAddress['ENTITY_ID'] = $resultRequisite['result'];
	CRest::call(
		'crm.address.add',
		[
			'fields' => $arAddress
		]
	);
}
```

### Full example of the handler code

```php
<?php
require_once('crest.php');

// Retrieve and clean the form data
$iRequisitePresetID = intVal($_POST["REQ_TYPE"]);
$sName = htmlspecialchars($_POST["NAME"]);
$sLastName = htmlspecialchars($_POST["LAST_NAME"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);

// Prepare the address
$arAddress = [];
foreach ($_POST["ADDRESS"] as $key => $val) {
    $arAddress[$key] = htmlspecialchars($val);
}
$arAddress['TYPE_ID'] = 1; // Actual address
$arAddress['ENTITY_TYPE_ID'] = 8; // Object type — detail

// Format the phone for Bitrix24
$arPhone = !empty($sPhone) ? [['VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK']] : [];

// Create the contact
$result = CRest::call('crm.contact.add', [
    'fields' => [
        'NAME' => $sName,
        'LAST_NAME' => $sLastName,
        'PHONE' => $arPhone
    ]
]);

// Get the identifier of the new contact
if (!empty($result['result'])) {
    $contactId = $result['result'];

    // Add details for the new contact
    $resultRequisite = CRest::call('crm.requisite.add', [
        'fields' => [
            'ENTITY_TYPE_ID' => 3, // Object type — contact
            'ENTITY_ID' => $contactId,
            'PRESET_ID' => $iRequisitePresetID,
            'ACTIVE' => 'Y',
            'NAME' => implode(' ', [$sName, $sLastName]),
        ]
    ]);

    // Add address if details were created successfully
    if (!empty($resultRequisite['result'])) {
        $arAddress['ENTITY_ID'] = $resultRequisite['result'];
        CRest::call('crm.address.add', ['fields' => $arAddress]);
    }

    echo json_encode(['message' => 'Contact successfully added']);
} else {
    $error = !empty($result['error_description']) ? $result['error_description'] : 'Unknown error';
    echo json_encode(['message' => 'Error: ' . $error]);
}
?>
```