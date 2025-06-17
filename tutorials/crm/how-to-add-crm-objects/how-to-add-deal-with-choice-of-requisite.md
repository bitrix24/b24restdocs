# Add a deal and company with requisites

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

Using a web form, you can automatically add new deals and companies with requisites in Bitrix24. When a client fills out the form, the data is sent to a handler. The handler script creates objects in the CRM via the API.

The setup consists of two stages.

1. Prepare the fields and place the web form on the page.

2. Create a handler file that sequentially calls the methods [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md), [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md), [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md), and [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md).

## 1\. Create the web form

To generate the fields, we use two methods:

-  [crm.address.fields](../../../api-reference/crm/requisites/addresses/crm-address-fields.md) — retrieves the list of address fields. The result is saved in the array `$arAddressFields`.

-  [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) — retrieves the list of requisites templates by the fields `ID` and `NAME`. The result is saved in the array `$arRequisiteType`.

{% include [Note on examples](../../../_includes/examples.md) %}

```php
$arAddressFields = CRest::call('crm.address.fields',[]);
$arRequisiteType = CRest::call('crm.requisite.preset.list',
    [
        'select'=>[
            "ID", "NAME"
        ]
    ]
);
```

From the array `$arAddressFields`, we remove unnecessary address fields so that they do not appear in the form.

```php
unset($arAddressFields['result']['TYPE_ID']);
unset($arAddressFields['result']['ENTITY_TYPE_ID']);
unset($arAddressFields['result']['ENTITY_ID']);
unset($arAddressFields['result']['COUNTRY_CODE']);
unset($arAddressFields['result']['ANCHOR_TYPE_ID']);
unset($arAddressFields['result']['ANCHOR_ID']);
```

We create an HTML form with the fields:

- `REQ_TYPE` — a dropdown list with requisites templates from the array `$arRequisiteType`. This is a required field.

- `TITLE` — the name of the company. This is a required field.

- `INN` — the company's INN.

- `PHONE` — the phone number.

- `ADDRESS` — address fields are created dynamically from `$arAddressFields`. If a field is required, the `required` attribute is added.

The form collects data into a string and sends it via `POST` to the file `form.php`.

```html
<form id="form_to_crm">
    <select name="REQ_TYPE" required>
        <option value="" disabled selected>Select</option>
        <?php foreach($arRequisiteType as $id=>$name):?>
            <option value="<?=$id?>"><?=$name?></option>
        <?php endforeach;?>
    </select>
    <input type="text" name="TITLE" placeholder="Org name" required>
    <input type="text" name="INN" placeholder="INN">
    <input type="text" name="PHONE" placeholder="Phone">
    <?php if(is_array($arAddressFields['result'])):?>
        <?php foreach($arAddressFields['result'] as $key=>$arField):?>
            <input type="text" name="ADDRESS[<?=$key?>]" placeholder="<?=$arField['title']?>" <?=($arField['isRequired'])?'required':'';?>>
        <?php endforeach;?>
    <?php endif;?>
    <input type="submit" value="Submit">
</form>
```

### Full example of the code

```php
<?php
require_once('crest.php');

$arAddressFields = CRest::call('crm.address.fields',[]);

$arRequisiteType = CRest::call('crm.requisite.preset.list',
    [
        'select'=>[
            "ID", "NAME"
        ]
    ]
);
if(!empty($arRequisiteType['result'])):
    $arRequisiteType = array_column($arRequisiteType['result'],'NAME','ID');
    //unset system address fields
    unset($arAddressFields['result']['TYPE_ID']);
    unset($arAddressFields['result']['ENTITY_TYPE_ID']);
    unset($arAddressFields['result']['ENTITY_ID']);
    //unset uninteresting address fields
    unset($arAddressFields['result']['COUNTRY_CODE']);
    unset($arAddressFields['result']['ANCHOR_TYPE_ID']);
    unset($arAddressFields['result']['ANCHOR_ID']);
    ?>
    <form id="form_to_crm">
        <select name="REQ_TYPE" required>
            <option value="" disabled selected>Select</option>
            <?php foreach($arRequisiteType as $id=>$name):?>
                <option value="<?=$id?>"><?=$name?></option>
            <?php endforeach;?>
        </select>
        <input type="text" name="TITLE" placeholder="Org name" required>
        <input type="text" name="INN" placeholder="INN">
        <input type="text" name="PHONE" placeholder="Phone">
        <?php if(is_array($arAddressFields['result'])):?>
            <?php foreach($arAddressFields['result'] as $key=>$arField):?>
                <input type="text" name="ADDRESS[<?=$key?>]" placeholder="<?=$arField['title']?>" <?=($arField['isRequired'])?'required':'';?>>
            <?php endforeach;?>
        <?php endif;?>
        <input type="submit" value="Submit">
    </form>
<?php else:?>
    No requisite types.
<?php endif;?>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script>
$(document).ready(function() {
    $('#form_to_crm').on( 'submit', function(el) {//event submit form
        el.preventDefault();//the default action of the event will not be triggered
        var formData = $(this).serialize();
        $.ajax({
            'method': 'POST',
            'dataType': 'json',
            'url': 'form.php', // file for saving filled forms
            'data': formData,
            success: function(data){//success callback
                alert(data.message);
            }
        });
    });
});
</script>
```

## 2\. Create the form handler

Create the file `form.php`, which will process the data and save it in the CRM.

### Retrieve the data

Retrieve and process the data from the form.

```php
$iRequisitePresetID = intval($_POST["REQ_TYPE"]); 
$sTitle = htmlspecialchars($_POST["TITLE"]); 
$sINN = htmlspecialchars($_POST["INN"]); 
$sPhone = htmlspecialchars($_POST["PHONE"]);
$arAddress = [];
foreach ($_POST["ADDRESS"] as $key => $val) {
 $arAddress[$key] = htmlspecialchars($val); 
}
```

-  `$iRequisitePresetID` — convert the requisites template identifier `REQ_TYPE` to an integer.

-  `$sTitle`, `$sINN`, `$sPhone` — safely process the data from `TITLE`, `INN`, `PHONE` to avoid XSS attacks.

-  `$arAddress` — store the data from the array with address fields `ADDRESS`.

### Prepare the data

Add two required system fields to the `$arAddress` array.

-  `TYPE_ID` — the type of address. We will specify `1` — the actual address. The list of address types can be obtained using the method [crm.enum.addresstype](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md).

-  `ENTITY_TYPE_ID` — [the identifier of the CRM object type](../../../api-reference/crm/data-types.md#object_type). We pass `8` — requisites. The complete list of object types can be obtained using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md).

```php
$arAddress['TYPE_ID'] = 1;
$arAddress['ENTITY_TYPE_ID'] = 8;
```

The system stores the phone as an array of objects [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield), so the value of `$sPhone` needs to be formatted as an array:

-  in the first element `VALUE`, we write `$sPhone`,

-  in the second element `VALUE_TYPE`, we pass, for example, `WORK`.

If the variable `$sPhone` has no value, we specify an empty array.

```php
$arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
```

### Add the company

To add a company, we use the method [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md). We need to pass the following data:

-  `TITLE` — the name of the company. We pass `$sTitle`, which we obtained from the form.

-  `COMPANY_TYPE` — the type of company. We will specify `CUSTOMER` — client. The list of types can be obtained using the method [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) with the filter `'filter'=>['ENTITY_ID'=>’COMPANY_TYPE']`.

-  `PHONE` — an array with the phone `$arPhone`, which we obtained from the form.

{% note warning "" %}

Check which required fields are set for companies in your Bitrix24. All required fields must be passed to the method [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md).

{% endnote %}

```php
$result = CRest::call(
    'crm.company.add',
    [
        'fields' =>[
            'TITLE' => $sTitle,
            'COMPANY_TYPE' => 'CUSTOMER',
            'PHONE' => $arPhone,
        ]
    ]
);
```

If the company is successfully created, the method will return its identifier in `$result`. If you receive an `error`, review the possible error descriptions in the documentation for the method [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md).

### Add requisites

To add requisites, we use the method [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md). We need to pass the following data:

-  `ENTITY_TYPE_ID` — [the identifier of the CRM object type](../../../api-reference/crm/data-types.md#object_type). We pass `4` — company. The complete list of object types can be obtained using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md).

-  `ENTITY_ID` — the identifier of the company. We pass `$result`, which we obtained when creating the company.

-  `PRESET_ID` — the identifier of the requisites template. We specify `$iRequisitePresetID`, which we obtained from the form.

-  `NAME` — the name of the requisite. We pass `$sTitle`, which we obtained from the form.

-  `RQ_INN` — the company's INN. We pass `$sINN`, which we obtained from the form.

-  `ACTIVE` — the active flag, we will specify `Y`.

```php
$resultRequisite = CRest::call(
    'crm.requisite.add',
    [
        'fields' =>[
            'ENTITY_TYPE_ID' => 4,
            'ENTITY_ID' => $result['result'],
            'PRESET_ID' => $iRequisitePresetID,
            'ACTIVE' => 'Y',
            'NAME' => $sTitle,
            'RQ_INN' => $sINN,
        ]
    ]
);
```

If the requisites are successfully added, the method will return the record identifier in `$resultRequisite`. If you receive an `error`, review the possible error descriptions in the documentation for the method [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md).

### Add address to requisites

1. Add the field `ENTITY_ID` to the `$arAddress` array — the identifier of the requisite. We pass `$resultRequisite`, which we obtained when creating the requisite.

   ```php
   $arAddress['ENTITY_ID'] = $resultRequisite['result'];
   ```

2. Use the method [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md). We need to pass the array `$arAddress`.

   ```php
   $resultAddress = CRest::call(
       'crm.address.add',
       [
           'fields' =>$arAddress
       ]
   );
   ```

The method returns one of the values in the variable `$resultAddress`:

-  `true` — the address has been added,

-  `false` — the address has not been added.

### **Add a deal**

We create the array `$arDealFields` with the data for the deal.

-  `TITLE`  — the name of the deal. We will specify the name of the company `$sTitle`, which was obtained from the form,

-  `COMPANY_ID` — the identifier of the company associated with the deal. We pass `$result`, which we obtained when creating the company,

-  `REQUISITE_ID` — the identifier of the requisite. If the requisite is created, we pass `$resultRequisite`.

```php
$arDealFields = [
    'TITLE' => $sTitle,
    'COMPANY_ID' => $result['result']
];
if(!empty($resultRequisite['result'])){
    $arDealFields['REQUISITE_ID'] = $resultRequisite['result'];
}
```

To add a deal, we use the method [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md). We pass the array `$arDealFields`.

```php
$resultDeal = CRest::call(
    'crm.deal.add',
    [
        'fields' => $arDealFields
    ]
);
```

If the deal is successfully created, the method will return its identifier. If you receive an `error`, review the possible error descriptions in the documentation for the method [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md).

```json
{
    "result": 1789,
}
```

### Full example of the handler code

```php
<?php
require_once('crest.php');

$iRequisitePresetID = intVal($_POST["REQ_TYPE"]);
$sTitle = htmlspecialchars($_POST["TITLE"]);
$sINN = htmlspecialchars($_POST["INN"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);
$arAddress = [];

foreach($_POST["ADDRESS"] as $key=>$val){
    $arAddress[$key] = htmlspecialchars($val);
}
$arAddress['TYPE_ID'] = 1;//1 is actual address in CRest::call('crm.enum.addresstype');
$arAddress['ENTITY_TYPE_ID'] = 8;//8 - is requisite in CRest::call('crm.enum.ownertype');

$arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();

$result = CRest::call(
    'crm.company.add',
    [
        'fields' =>[
            'TITLE' => $sTitle,
            'COMPANY_TYPE' => 'CUSTOMER',//is Client in CRest::call('crm.status.list',['filter'=>['ENTITY_ID'=>'COMPANY_TYPE']])
            'PHONE' => $arPhone,
        ]
    ]
);
if(!empty($result['result'])){
    $resultRequisite = CRest::call(
        'crm.requisite.add',
        [
            'fields' =>[
                'ENTITY_TYPE_ID' => 4,//4 - is company in CRest::call('crm.enum.ownertype');
                'ENTITY_ID' => $result['result'],//company id
                'PRESET_ID' => $iRequisitePresetID,
                'ACTIVE' => 'Y',
                'NAME' => $sTitle,
                'RQ_INN' => $sINN,
            ]
        ]
    );
    $arDealFields = [
        'TITLE' => $sTitle,
        'COMPANY_ID' => $result['result']
    ];
    if(!empty($resultRequisite['result'])){
        $arDealFields['REQUISITE_ID'] = $resultRequisite['result'];//add requisite to deal is analogue "crm.requisite.link.list"
        $arAddress['ENTITY_ID'] = $resultRequisite['result'];//id requisite
        $resultAddress = CRest::call(
            'crm.address.add',
            [
                'fields' =>$arAddress
            ]
        );
    }
    $resultDeal = CRest::call(
        'crm.deal.add',
        [
            'fields' => $arDealFields
        ]
    );
    echo json_encode(['message' => 'add']);
}elseif(!empty($result['error_description'])){
    echo json_encode(['message' => 'not added: '.$result['error_description']]);
}else{
    echo json_encode(['message' => 'not added']);
}
?>
```

## Continue learning 

- [{#T}](../../../api-reference/crm/companies/crm-company-add.md)
- [{#T}](../../../api-reference/crm/requisites/universal/crm-requisite-add.md)
- [{#T}](../../../api-reference/crm/requisites/addresses/crm-address-add.md)
- [{#T}](../../../api-reference/crm/deals/crm-deal-add.md)
- [{#T}](../../../api-reference/crm/requisites/addresses/crm-address-fields.md)
- [{#T}](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md)
- [{#T}](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md)
- [{#T}](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md)