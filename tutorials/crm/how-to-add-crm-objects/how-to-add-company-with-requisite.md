# Add a Company with Details via Web Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to create companies in CRM

You can place a form on the site to collect client data and details. When a client fills out the form, their data will be sent to CRM, and you will be able to process the request.

Setting up the form consists of two steps.

1. We will place the form on a PHP page. In the page code, we will retrieve the list of detail templates and address fields for the form. The form data will be sent to the handler.

2. We will create a file to process the data. The handler will accept and prepare the data, and then create a company with the details.

## 1\. Creating the Web Form

To generate the form fields, we will use data from Bitrix24. To get information about the detail settings, we will sequentially execute two methods:

1. [crm.address.fields](../../../api-reference/crm/requisites/addresses/crm-address-fields.md) — we get the list of address fields. The result is saved in `arAddressFields`.

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

-  `REQ_TYPE` — a dropdown list with the type of details from the `arRequisiteType` array,
  
-  `TITLE` — the name of the company, required,

-  `INN` — Tax Identification Number,

-  `PHONE` — phone number,

-  `${addressFieldsInputs}` — address fields that are dynamically created from the `arAddressFields` array.

The form sends data using the `POST` method to the `form.php` file.

### Full Example of the Page Code with the Form

{% include [Example Notes](../../../_includes/examples.md) %}

```php
<?php
// Get the list of address fields and detail templates
$arAddressFields = CRest::call('crm.address.fields', []);
$arRequisiteType = CRest::call('crm.requisite.preset.list', [
    'select' => ["ID", "NAME"]
]);

if(!empty($arRequisiteType['result'])): 
    $arRequisiteType = array_column($arRequisiteType['result'], 'NAME', 'ID');
    // Remove system and unused address fields
    $excludeFields = ['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID'];
    foreach($excludeFields as $field) {
        unset($arAddressFields['result'][$field]);
    }
?>
    <form id="form_to_crm">
        <select name="REQ_TYPE" required>
            <option value="" disabled selected>Select the type of details</option>
            <?php foreach($arRequisiteType as $id => $name): ?>
                <option value="<?=$id?>"><?=$name?></option>
            <?php endforeach; ?>
        </select>
        <input type="text" name="TITLE" placeholder="Organization Name" required>
        <input type="text" name="INN" placeholder="Tax Identification Number">
        <input type="text" name="PHONE" placeholder="Phone Number">
        <?php foreach($arAddressFields['result'] as $key => $arField): ?>
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

## 2\. Creating the Form Handler

To process the values from the form fields and add a company to CRM, we will create the handler `form.php`.

### Preparing the Data

We will retrieve and sanitize the data from the form:

-  `REQ_TYPE` is converted to an integer,

-  `TITLE`, `INN`, `PHONE` are sanitized from HTML tags.

```php
$iRequisitePresetID = intVal($_POST["REQ_TYPE"]);
$sTitle = htmlspecialchars($_POST["TITLE"]);
$sINN = htmlspecialchars($_POST["INN"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);
```

We prepare the address fields and collect them into the `$arAddress` array.

-  The values of the fields from the form are sanitized from HTML tags.

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

### Adding the Company

To create a company, we will execute the method [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md). In the `fields` object, we pass the fields:

-  `TITLE` — the name of the company.

-  `COMPANY_TYPE` — the type of company. The types of companies can be found using the method [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) by filtering the `filter` field by `ENTITY_ID` with the value `COMPANY_TYPE`. In the example, we will specify the value `CUSTOMER`, as only clients of the company fill out the form.

-  `PHONE` — phone number.

{% note warning "" %}

Check which required fields are set for companies in your Bitrix24. All required fields must be passed to the method [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md).

{% endnote %}

```php
CRest::call(
	'crm.company.add', [
    	'fields' => [
        	'TITLE' => $sTitle,
        	'COMPANY_TYPE' => 'CUSTOMER', // Company type — client
        	'PHONE' => $arPhone
    	]
	]
);
```

As a result, we will receive the identifier of the new company `5`.

```json
{
	"result": 5
}
```

### Adding Details to the Company

To add details to the company, we will execute the method [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md). In the `fields` object, we pass the fields:

-  `ENTITY_TYPE_ID` — the identifier of the [object type](../../../api-reference/crm/data-types.md#object_type). The identifiers can be obtained using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md). In the example, we will specify the value `4`, which means company,

-  `ENTITY_ID` — the identifier of the company obtained in the previous request,

-  `PRESET_ID` — the identifier of the detail template obtained from the form,

-  `ACTIVE` — the activity of the detail `Y`,

-  `NAME` — the name of the detail,

-  `RQ_INN` — the Tax Identification Number of the company.

```php
CRest::call(
	'crm.requisite.add', 
	[
	    'fields' => [
			'ENTITY_TYPE_ID' => 4,
			'ENTITY_ID' => $companyId,
			'PRESET_ID' => $iRequisitePresetID,
			'ACTIVE' => 'Y',
			'NAME' => $sTitle,
			'RQ_INN' => $sINN
		]
	]
);
```

As a result, we will receive the identifier of the details.

```json
{
    "result": 27
}
```

### Adding an Address for the Detail

We will add an address for the detail using the method [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) if the detail was created successfully. In `$arAddress`, we add `ENTITY_ID` with the `ID` of the detail from the response of the previous request. In the `fields` object, we pass the `$arAddress` array with the address fields.

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

### Full Example of the Handler Code

```php
<?php
require_once('crest.php');

// Get and sanitize form data
$iRequisitePresetID = intVal($_POST["REQ_TYPE"]);
$sTitle = htmlspecialchars($_POST["TITLE"]);
$sINN = htmlspecialchars($_POST["INN"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);

// Prepare address
$arAddress = [];
foreach($_POST["ADDRESS"] as $key => $val) {
    $arAddress[$key] = htmlspecialchars($val);
}
$arAddress['TYPE_ID'] = 1; // Value 1 — actual address
$arAddress['ENTITY_TYPE_ID'] = 8; // Object type identifier — 8, which means detail

// Format phone for Bitrix24 in crm_multifield format
$arPhone = !empty($sPhone) ? [['VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK']] : [];

// Create company
$result = CRest::call('crm.company.add', [
    'fields' => [
        'TITLE' => $sTitle,
        'COMPANY_TYPE' => 'CUSTOMER', // Type — client
        'PHONE' => $arPhone
    ]
]);

// Get the identifier of the new company from the response of crm.company.add
if(!empty($result['result'])) {
    $companyId = $result['result'];
    
    // Add details for the new company
    $resultRequisite = CRest::call('crm.requisite.add', [
        'fields' => [
            'ENTITY_TYPE_ID' => 4, // Object type identifier — 4, which means company
            'ENTITY_ID' => $companyId,
            'PRESET_ID' => $iRequisitePresetID,
            'ACTIVE' => 'Y',
            'NAME' => $sTitle,
            'RQ_INN' => $sINN
        ]
    ]);
    
    // Add address if details were created successfully
    if(!empty($resultRequisite['result'])) {
        $arAddress['ENTITY_ID'] = $resultRequisite['result'];
        CRest::call('crm.address.add', ['fields' => $arAddress]);
    }
    
    echo json_encode(['message' => 'Company successfully added']);
} else {
    $error = !empty($result['error_description']) ? $result['error_description'] : 'Unknown error';
    echo json_encode(['message' => 'Error: ' . $error]);
}
?>
```