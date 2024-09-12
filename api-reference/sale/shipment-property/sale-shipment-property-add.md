# Add Shipment Property sale.shipmentproperty.add

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a shipment property.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a shipment property ||
|#

### Parameter fields

Common parameters applicable to shipment properties of any type:

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **personTypeId***
[`sale_person_type.id`](../data-types.md) | Identifier of the payer type ||
|| **propsGroupId***
[`sale_order_property_group.id`](../data-types.md) | Identifier of the property group ||
|| **name***
[`string`](../../data-types.md) | Name of the shipment property ||
|| **type***
[`string`](../../data-types.md) | Type of the shipment property.
Possible values:
- `STRING`
- `Y/N`
- `NUMBER`
- `ENUM`
- `FILE`
- `DATE`
- `LOCATION`
- `ADDRESS`
 ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the shipment property ||
|| **active**
[`string`](../../data-types.md) | Indicator of the shipment property’s activity.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `Y` (the property is added as active) ||
|| **util**
[`string`](../../data-types.md) | Indicator of whether the shipment property is a system property. System properties are not displayed in the public part.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N`
 ||
|| **userProps**
[`string`](../../data-types.md) | Indicator of whether the shipment property is included in the customer profile.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isFiltered**
[`string`](../../data-types.md) | Indicator of whether the shipment property is available in the filter on the shipment list page.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **sort**
[`integer`](../../data-types.md) | Sorting ||
|| **description**
[`string`](../data-types.md) | Description of the shipment property ||
|| **required**
[`string`](../../data-types.md) | Indicator of whether the shipment property value is required.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **multiple**
[`string`](../../data-types.md) | Indicator of whether the shipment property is multiple. For multiple properties, several values can be specified.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the shipment property ||
|| **defaultValue**
[`any`](../../data-types.md) | Default value of the shipment property.
For multiple shipment properties (multiple), an array of values can be passed  ||
|| **settings**
[`object`](../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}` for passing additional settings for the shipment property.

The list of supported keys for this object depends on the property type. For some property types (e.g., Y/N), additional properties are not provided. The description of the **settings** parameter for different property types is provided [below](#parameter-settings) ||
|#

Parameters applicable to shipment properties of type `STRING`

#|
|| **Name**
`type` | **Description** ||
|| **isProfileName**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as the user profile name.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isPayer**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as the payer's name.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N`||
|| **isEmail**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as an e-mail (e.g., when registering a new user during order placement).
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N`||
|| **isPhone**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as a phone number.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N`||
|| **isZip**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as a postal code.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N`||
|| **isAddress**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as an address.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N`||
|#

Parameters applicable to shipment properties of type `LOCATION`			

#|
|| **Name**
`type` | **Description** ||
|| **isLocation**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as the buyer's location for calculating delivery costs.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isLocation4tax**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as the buyer's location for determining tax rates.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **inputFieldLocation**
[`string`](../../data-types.md) | Deprecated field. Not used ||
|#

Parameters applicable to shipment properties of type `ADDRESS`			

#|
|| **Name**
`type` | **Description** ||
|| **isAddressFrom**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as the buyer's address from where the order needs to be picked up for calculating delivery costs.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isAddressTo**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as the buyer's address to which the order needs to be delivered for calculating delivery costs.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|#

### Parameter settings

Parameters applicable to shipment properties of type `STRING`

#|
|| **Name**
`type` | **Description** ||
|| **minlength**
[`integer`](../../data-types.md) | Minimum allowable length (number of characters) of the shipment property value ||
|| **maxlength**
[`integer`](../../data-types.md) | Maximum allowable length (number of characters) of the shipment property value ||
|| **pattern**
[`string`](../../data-types.md) | Regular expression for validating the shipment property value.
Examples:
Regular expression for validating a phone number ```^((8\|\+1)[\- ]?)?(\(?\d{3}\)?[\- ]?)?[\d\- ]{7,10}$```
Regular expression for validating the date format DD/MM/YYYY:
```^(0?[1-9]|[12][0-9]|3[01])[\/\-](0?[1-9]|1[012])[\/\-]\d{4}$``` ||
|| **multiline**
[`string`](../../data-types.md) | Indicator of whether to display a multiline input field for the shipment property value. Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **cols**
[`integer`](../../data-types.md) | Deprecated parameter. Not used ||
|| **rows**
[`integer`](../../data-types.md) | Deprecated parameter. Not used ||
|| **size**
[`integer`](../../data-types.md) | Deprecated parameter. Not used ||
|#

Parameters applicable to shipment properties of type `NUMBER`

#|
|| **Name**
`type` | **Description** ||
|| **min**
[`integer`](../../data-types.md) | Minimum allowable value for this shipment property ||
|| **max**
[`integer`](../../data-types.md) | Maximum allowable value for this shipment property ||
|| **step**
[`integer`](../../data-types.md) | Step for changing the value. Used in some user interfaces for convenience in changing the shipment property value ||
|#

Parameters applicable to shipment properties of type `ENUM`

#|
|| **Name**
`type` | **Description** ||
|| **multielement**
[`string`](../../data-types.md) | Indicator of whether to display the shipment property as a list of checkboxes.
Value is used in some user interfaces.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **size**
[`integer`](../../data-types.md) | Number of displayed values of the shipment property. Value is used in some user interfaces ||
|#

Parameters applicable to shipment properties of type `FILE`

#|
|| **Name**
`type` | **Description** ||
|| **maxsize**
[`integer`](../../data-types.md) | Maximum allowable size of the uploaded file in bytes ||
|| **accept**
[`string`](../../data-types.md) | List of file extensions that can be uploaded as the value of this shipment property. Example: png, doc, zip ||
|#

Parameters applicable to shipment properties of type `DATE`

#|
|| **Name**
`type` | **Description** ||
|| **time**
[`string`](../../data-types.md) | Indicator of whether to add the option to select time when working with the value of this shipment property. Value is used in some user interfaces.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"personTypeId":3,"propsGroupId":6,"name":"Phone (for contacting the courier)","type":"STRING","code":"PHONE","active":"Y","util":"N","userProps":"Y","isFiltered":"N","sort":500,"description":"description of the property","required":"Y","multiple":"N","settings":{"multiline":"Y","maxlength":100},"xmlId":"","defaultValue":"","isProfileName":"Y","isPayer":"Y","isEmail":"N","isPhone":"N","isZip":"N","isAddress":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentproperty.add
    ```

- cURL (OAuth)

    ```http
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"personTypeId":3,"propsGroupId":6,"name":"Phone (for contacting the courier)","type":"STRING","code":"PHONE","active":"Y","util":"N","userProps":"Y","isFiltered":"N","sort":500,"description":"description of the property","required":"Y","multiple":"N","settings":{"multiline":"Y","maxlength":100},"xmlId":"","defaultValue":"","isProfileName":"Y","isPayer":"Y","isEmail":"N","isPhone":"N","isZip":"N","isAddress":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentproperty.add
    ```

- JS

    ```js
    BX24.callMethod(
    'sale.shipmentproperty.add', {
        fields: {
            personTypeId: 3,
            propsGroupId: 6,
            name: 'Phone (for contacting the courier)',
            type: 'STRING',
            code: 'PHONE',
            active: 'Y',
            util: 'N',
            userProps: 'Y',
            isFiltered: 'N',
            sort: 500,
            description: 'description of the property',
            required: 'Y',
            multiple: 'N',
            settings: {
                multiline: 'Y',
                maxlength: 100
            },
            xmlId: '',
            defaultValue: '',
            isProfileName: 'Y',
            isPayer: 'Y',
            isEmail: 'N',
            isPhone: 'N',
            isZip: 'N',
            isAddress: 'N',
        }
    },
    function(result) {
        if (result.error()) {
            console.error(result.error());
        } else {
            console.info(result.data());
        }
    }
    );

    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.shipmentproperty.add',
        [
            'fields' => [
                'personTypeId' => 3,
                'propsGroupId' => 6,
                'name' => 'Phone (for contacting the courier)',
                'type' => 'STRING',
                'code' => 'PHONE',
                'active' => 'Y',
                'util' => 'N',
                'userProps' => 'Y',
                'isFiltered' => 'N',
                'sort' => 500,
                'description' => 'description of the property',
                'required' => 'Y',
                'multiple' => 'N',
                'settings' => [
                    'multiline' => 'Y',
                    'maxlength' => 100
                ],
                'xmlId' => '',
                'defaultValue' => '',
                'isProfileName' => 'Y',
                'isPayer' => 'Y',
                'isEmail' => 'N',
                'isPhone' => 'N',
                'isZip' => 'N',
                'isAddress' => 'N',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP Status: **200**

```json
{
   "result":{
      "property":{
         "active":"Y",
         "code":"PHONE",
         "defaultValue":"",
         "description":"description of the property",
         "id":96,
         "inputFieldLocation":"0",
         "isAddress":"N",
         "isAddressFrom":"N",
         "isAddressTo":"N",
         "isEmail":"N",
         "isFiltered":"N",
         "isLocation":"N",
         "isLocation4tax":"N",
         "isPayer":"Y",
         "isPhone":"N",
         "isProfileName":"Y",
         "isZip":"N",
         "multiple":"N",
         "name":"Phone (for contacting the courier)",
         "personTypeId":3,
         "propsGroupId":6,
         "required":"Y",
         "settings":{
            "maxlength":"100",
            "multiline":"Y"
         },
         "sort":500,
         "type":"STRING",
         "userProps":"Y",
         "util":"N",
         "xmlId":""
      }
   },
   "time":{
      "start":1712818563.754118,
      "finish":1712818566.840385,
      "duration":3.0862669944763184,
      "processing":1.0286660194396973,
      "date_start":"2024-04-11T09:56:03+03:00",
      "date_finish":"2024-04-11T09:56:06+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **property**
[`sale_shipment_property`](../data-types.md) | Object containing information about the added shipment property ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
   "error":0,
   "error_description":"Required fields: propsGroupId"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200850000005` | An empty value is specified for the payer type ||
|| `200850000006` | Internal error adding property ||
|| `200850000009` | Error occurs when trying to create a shipment property with the `multiple` parameter set to `Y`, if the `isFiltered` parameter is not provided.
Filtering by multiple shipment properties is not supported ||
|| `200850000010` | Error occurs when trying to create a shipment property with the `multiple` parameter set to `Y`, if the `isFiltered` parameter is not equal to `N`.
Filtering by multiple shipment properties is not supported ||
|| `200850000011` | Error occurs when trying to create a shipment property of type `LOCATION` with the `isLocation` parameter set to `Y`, if the `multiple` parameter is not specified. 
Multiplicity is not supported for shipment properties marked with the `isLocation` indicator ||
|| `200850000012` | Error occurs when trying to create a shipment property of type `LOCATION` with the `isLocation` parameter set to `Y`, if the `multiple` parameter is not equal to `N`. 
Multiplicity is not supported for shipment properties marked with the `isLocation` indicator ||
|| `200850000013` | Error occurs when trying to create a shipment property of type `LOCATION` with the `isLocation4tax` parameter set to `Y`, if the `multiple` parameter is not specified. 
Multiplicity is not supported for shipment properties marked with the `isLocation4tax` indicator ||
|| `200850000014` | Error occurs when trying to create a shipment property of type `LOCATION` with the `isLocation4tax` parameter set to `Y`, if the `multiple` parameter is not equal to `N`. 
Multiplicity is not supported for shipment properties marked with the `isLocation4tax` indicator ||
|| `200850000015` | Error occurs when trying to create a shipment property of type `STRING` with the `isProfileName` parameter set to `Y`, if the `required` parameter is not specified. 
Profile name is required and cannot be empty ||
|| `200850000016` | Error occurs when trying to create a shipment property of type [`STRING`](../../data-types.md) with the `isProfileName` parameter set to `Y`, if the `required` parameter is not equal to `Y`. 
Profile name is required and cannot be empty ||
|| `200040300020` | Insufficient permissions to add the shipment property ||
|| `100` | The `fields` parameter is not specified or is empty ||
|| `0` | Required fields are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-property-update.md)
- [{#T}](./sale-shipment-property-get.md)
- [{#T}](./sale-shipment-property-list.md)
- [{#T}](./sale-shipment-property-delete.md)
- [{#T}](./sale-shipment-property-get-fields-by-type.md)