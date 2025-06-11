# Add Repeat Lead

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with read access permission for leads, contacts, companies, and permission to create leads.

When a client fills out a form on the site, their data is sent to the handler. The script searches the CRM for matches by phone or email among leads, contacts, and companies. If matches are found, the lead is marked as a repeat and linked to the existing record. This approach helps avoid duplicates and increases the efficiency of managers.

{% note info "" %}

The repeat lead mode must be enabled in Bitrix24. Read more in the article [Repeat Leads and Deals](https://helpdesk.bitrix24.com/open/24147842/).

{% endnote %}

The setup consists of two stages:

1. Prepare the fields and place the form on the page.

2. Create a handler file that sequentially calls the methods [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md), [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md), [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md).

## 1. Create Web Form

Create an HTML form with the fields:

- `NAME` — client's first name, required field,

- `LAST_NAME` — last name,

- `PHONE` — phone number,

- `EMAIL` — email address.

The form sends data using the `POST` method to the file `form.php`.

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script>
$(document).ready(function() {
    $('#form_to_crm').on('submit', function(el) { // event submit form
        el.preventDefault(); // the default action of the event will not be triggered
        var formData = $(this).serialize();
        $.ajax({
            'method': 'POST',
            'dataType': 'json',
            'url': 'form.php', // file for saving filled forms
            'data': formData,
            success: function(data) { // success callback
                alert(data.message);
            }
        });
    });
});
</script>
    
<form id="form_to_crm">
    <input type="text" name="NAME" placeholder="Name" required>
    <input type="text" name="LAST_NAME" placeholder="Last name">
    <input type="text" name="PHONE" placeholder="Phone">
    <input type="text" name="EMAIL" placeholder="E-mail">
    <input type="submit" value="Submit">
</form>
```

## 2. Create Form Handler

Create the file `form.php`. The handler will process the data, check for duplicates, and create a lead.

### Get Data from the Form

Retrieve and safely process data from the fields `NAME`, `LAST_NAME`, `PHONE`, `EMAIL` to avoid XSS attacks.

```php
$sName = htmlspecialchars($_POST["NAME"]);    
$sLastName = htmlspecialchars($_POST["LAST_NAME"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);
$sEmail = htmlspecialchars($_POST["EMAIL"]);
```

Form an array `$arFields` with the new lead's data.

```php
$arFields = [
    'TITLE' => 'From the site: ' . implode(' ', [$sName, $sLastName]),
    'NAME' => (!empty($sName)) ? $sName : 'Empty name',
    'LAST_NAME' => $sLastName,
    'PHONE' => (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'HOME')) : array(),
    'EMAIL' => (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array()
];
```

The lead's title is formed as `From the site: FirstName LastName`.

The system stores phone numbers and email addresses as arrays of objects [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield), so we form the arrays `PHONE` and `EMAIL` using the values `$sPhone` and `$sEmail`.

- In the `VALUE` fields, we write `$sPhone` and `$sEmail`.

- In the `VALUE_TYPE` fields, we pass [types](../../../api-reference/crm/data-types.md#crm_multifield), for example, `HOME`.

If there are no values in the variables `$sPhone` and `$sEmail`, we specify empty arrays.

### Search for Duplicate Leads

To find duplicate leads by phone and email, we use the method [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) twice. We need to pass the following data:

- `entity_type` — object type. We pass `LEAD` — lead.

- `type` — communication type. For the first call, we specify `PHONE`, and for the second — `EMAIL`.

- `PHONE` — array with the phone `$arPhone` obtained from the form.

Search by phone, `"type" => "PHONE"`.

```php
if(!empty($sPhone)){
    $arResultDuplicate = CRest::call('crm.duplicate.findbycomm', [
        "entity_type" => "LEAD",
        "type" => "PHONE",
        "values" => array($sPhone)
    ]);
    if(!empty($arResultDuplicate['result']['LEAD'])){
        $arLeadDuplicate = array_merge($arLeadDuplicate, $arResultDuplicate['result']['LEAD']);
    }
}
```

Search for duplicates by email, `"type" => "EMAIL"`.

```php
if(!empty($sEmail)){
    $arResultDuplicate = CRest::call('crm.duplicate.findbycomm', [
        "entity_type" => "LEAD",
        "type" => "EMAIL",
        "values" => array($sEmail)
    ]);
    if(!empty($arResultDuplicate['result']['LEAD'])){
        $arLeadDuplicate = array_merge($arLeadDuplicate, $arResultDuplicate['result']['LEAD']);
    }
}
```

The identifiers of found duplicates are combined in the array `$arLeadDuplicate`.

### Process Duplicates

If duplicates are found, we call the method [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md).

1. Apply a filter by ID and status `CONVERTED`.

2. Select fields: `ID`, `COMPANY_ID`, `CONTACT_ID`.

3. Save the result in the array `$arDuplicateLead`.

4. Fill the fields `COMPANY_ID` and `CONTACT_ID` in the array `$arFields` with values from `$arDuplicateLead`.

```php
if(!empty($arLeadDuplicate)){
    $arDuplicateLead = CRest::call('crm.lead.list', [
        "filter" => [
            '=ID' => $arLeadDuplicate,
            'STATUS_ID' => 'CONVERTED'
        ],
        "select" => ['ID', 'COMPANY_ID', 'CONTACT_ID']
    ]);

    if(!empty($arDuplicateLead['result'])){
        $sCompany = reset(array_diff(array_column($arDuplicateLead['result'], 'COMPANY_ID', 'ID'), ['']));
        $sContact = reset(array_diff(array_column($arDuplicateLead['result'], 'CONTACT_ID', 'ID'), ['']));
        if($sCompany > 0) $arFields['COMPANY_ID'] = $sCompany;
        if($sContact > 0) $arFields['CONTACT_ID'] = $sContact;
    }
}
```

### Add New Lead

To add a lead, we use the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md). We pass the array `$arFields`.

{% note warning "" %}

Check which required fields are set for leads in your Bitrix24. All required fields must be passed to the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md).

{% endnote %}

```php
$result = CRest::call('crm.lead.add', [
    'fields' => $arFields
]);
```

If the lead is created successfully, the method will return its identifier. If you receive an `error`, review the possible error descriptions in the documentation for the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md).

```json
{
    "result": 3289
}
```

### Complete Example of the Handler Code

```php
<?php
$sName = htmlspecialchars($_POST["NAME"]);    
$sLastName = htmlspecialchars($_POST["LAST_NAME"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);
$sEmail = htmlspecialchars($_POST["EMAIL"]);
        
$arFields = [
    'TITLE' => 'From the site: ' . implode(' ', [$sName, $sLastName]),
    'LAST_NAME' => $sLastName,
    'PHONE' => (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'HOME')) : array(),
    'EMAIL' => (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array()
];
    
$arLeadDuplicate = [];
if(!empty($sPhone)){ // search for duplicates by phone
    $arResultDuplicate = CRest::call('crm.duplicate.findbycomm', [
        "entity_type" => "LEAD",
        "type" => "PHONE",
        "values" => array($sPhone)
    ]);
    if(!empty($arResultDuplicate['result']['LEAD'])){
        $arLeadDuplicate = array_merge($arLeadDuplicate, $arResultDuplicate['result']['LEAD']);
    }
}
    
if(!empty($sEmail)) { // search for duplicates by email
    $arResultDuplicate = CRest::call('crm.duplicate.findbycomm', [
        "entity_type" => "LEAD",
        "type" => "EMAIL",
        "values" => [$sEmail]
    ]);
    if(!empty($arResultDuplicate['result']['LEAD'])) {
        $arLeadDuplicate = array_merge($arLeadDuplicate, $arResultDuplicate['result']['LEAD']);
    }
}
    
if(!empty($arLeadDuplicate)){ // get duplicate lead with selection of related contact and company fields
    $arDuplicateLead = CRest::call('crm.lead.list', [
        "filter" => [
            '=ID' => $arLeadDuplicate,
            'STATUS_ID' => 'CONVERTED',
        ],
        'select' => [
            'ID', 'COMPANY_ID', 'CONTACT_ID'
        ]
    ]);
        
    if(!empty($arDuplicateLead['result'])){
        $sCompany = reset(array_diff(array_column($arDuplicateLead['result'], 'COMPANY_ID', 'ID'), ['']));
        $sContact = reset(array_diff(array_column($arDuplicateLead['result'], 'CONTACT_ID', 'ID'), ['']));
        if($sCompany > 0)
            $arFields['COMPANY_ID'] = $sCompany;
        if($sContact > 0)
            $arFields['CONTACT_ID'] = $sContact;
    }
}
    
$result = CRest::call('crm.lead.add', // create duplicate lead
    [
        'fields' => $arFields
    ]
);
if(!empty($result['result'])){
    echo json_encode(['message' => 'Lead added']);
}elseif(!empty($result['error_description'])){
    echo json_encode(['message' => 'Lead not added: '.$result['error_description']]);
}else{
    echo json_encode(['message' => 'Lead not added']);
}
?>
```

## Continue Learning 

- [{#T}](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md)
- [{#T}](../../../api-reference/crm/leads/crm-lead-list.md)
- [{#T}](../../../api-reference/crm/leads/crm-lead-add.md)