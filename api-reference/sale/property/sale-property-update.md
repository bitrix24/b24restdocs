# Update Order Property sale.property.update

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the order property.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_property.id`](../data-types.md) | Identifier of the order property ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating the order property ||
|#

### Parameter fields

General parameters applicable to order properties of any type:

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the order property ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the order property ||
|| **active**
[`string`](../../data-types.md) | Indicator of the order property’s activity.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `Y` ||
|| **util**
[`string`](../../data-types.md) | Indicator of whether the order property is a system property. System properties are not displayed in the public part.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **userProps**
[`string`](../../data-types.md) | Indicator of whether the order property is included in the customer profile.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isFiltered**
[`string`](../../data-types.md) | Indicator of whether the order property is available in the filter on the order list page.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **sort**
[`integer`](../../data-types.md) | Sorting ||
|| **description**
[`string`](../../data-types.md) | Description of the order property ||
|| **required**
[`string`](../../data-types.md) | Indicator of whether filling in the order property value is mandatory.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **multiple**
[`string`](../../data-types.md) | Indicator of whether the order property is multiple. For multiple properties, it is possible to specify several values.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the order property ||
|| **defaultValue**
[`any`](../../data-types.md) | Default value of the order property.
For multiple order properties (multiple), an array of values is supported ||
|| **settings**
[`object`](../../data-types.md) | Object in the format {"field_1": "value_1", ... "field_N": "value_N"} for passing additional settings of the order property.

The list of supported keys for this object depends on the property type. For some property types (e.g., Y/N), additional properties are not provided. The description of the **settings** parameter for different property types is provided in the description of the method [`sale.property.add`](sale-property-add.md) ||
|#

Parameters applicable to order properties of type `STRING`

#|
|| **Name**
`type` | **Description** ||
|| **isProfileName**
[`string`](../../data-types.md) | Indicator of whether the value of this order property should be used as the user profile name.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isPayer**
[`string`](../../data-types.md) | Indicator of whether the value of this order property should be used as the payer's name.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isEmail**
[`string`](../../data-types.md) | Indicator of whether the value of this order property should be used as an e-mail (e.g., when registering a new user during order placement).
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isPhone**
[`string`](../../data-types.md) | Indicator of whether the value of this order property should be used as a phone number.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isZip**
[`string`](../../data-types.md) | Indicator of whether the value of this order property should be used as a postal code.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isAddress**
[`string`](../../data-types.md) | Indicator of whether the value of this order property should be used as an address.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|#

Parameters applicable to order properties of type `LOCATION`			

#|
|| **Name**
`type` | **Description** ||
|| **isLocation**
[`string`](../../data-types.md) | Indicator of whether the value of this order property should be used as the buyer's location for calculating delivery costs.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isLocation4tax**
[`string`](../../data-types.md) | Indicator of whether the value of this order property should be used as the buyer's location for determining tax rates.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **inputFieldLocation**
[`string`](../../data-types.md) | Deprecated field. Not used. ||
|#

Parameters applicable to order properties of type `ADDRESS`

#|
|| **Name**
`type` | **Description** ||
|| **isAddressFrom**
[`string`](../../data-types.md) | Indicator of whether the value of this order property should be used as the buyer's address from where the order needs to be picked up for calculating delivery costs.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isAddressTo**
[`string`](../../data-types.md) | Indicator of whether the value of this order property should be used as the buyer's address to which the order needs to be delivered for calculating delivery costs.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":93,"fields":{"personTypeId":3,"propsGroupId":6,"name":"Phone (for contacting the courier)","type":"STRING","code":"PHONE","active":"Y","util":"N","userProps":"Y","isFiltered":"N","sort":500,"description":"property description","required":"Y","multiple":"N","settings":{"multiline":"Y","maxlength":100},"xmlId":"","defaultValue":"","isProfileName":"Y","isPayer":"Y","isEmail":"N","isPhone":"N","isZip":"N","isAddress":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.property.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":93,"fields":{"personTypeId":3,"propsGroupId":6,"name":"Phone (for contacting the courier)","type":"STRING","code":"PHONE","active":"Y","util":"N","userProps":"Y","isFiltered":"N","sort":500,"description":"property description","required":"Y","multiple":"N","settings":{"multiline":"Y","maxlength":100},"xmlId":"","defaultValue":"","isProfileName":"Y","isPayer":"Y","isEmail":"N","isPhone":"N","isZip":"N","isAddress":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.property.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.property.update', {
    			id: 93,
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
    				description: 'property description',
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
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.property.update',
                [
                    'id' => 93,
                    'fields' => [
                        'personTypeId'  => 3,
                        'propsGroupId'  => 6,
                        'name'          => 'Phone (for contacting the courier)',
                        'type'          => 'STRING',
                        'code'          => 'PHONE',
                        'active'        => 'Y',
                        'util'          => 'N',
                        'userProps'     => 'Y',
                        'isFiltered'    => 'N',
                        'sort'          => 500,
                        'description'   => 'property description',
                        'required'      => 'Y',
                        'multiple'      => 'N',
                        'settings'      => [
                            'multiline' => 'Y',
                            'maxlength' => 100,
                        ],
                        'xmlId'         => '',
                        'defaultValue'  => '',
                        'isProfileName' => 'Y',
                        'isPayer'       => 'Y',
                        'isEmail'       => 'N',
                        'isPhone'       => 'N',
                        'isZip'         => 'N',
                        'isAddress'     => 'N',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating sale property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.property.update', {
            id: 93,
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
                description: 'property description',
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.property.update',
        [
            'id' => 93,
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
                'description' => 'property description',
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

HTTP status: **200**

```json
{
   "result":{
      "property":{
         "active":"Y",
         "code":"PHONE",
         "defaultValue":"",
         "description":"property description",
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
      "start":1712818736.235335,
      "finish":1712818736.611224,
      "duration":0.3758888244628906,
      "processing":0.18679594993591309,
      "date_start":"2024-04-11T09:58:56+02:00",
      "date_finish":"2024-04-11T09:58:56+02:00"
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
[`sale_order_property`](../data-types.md) | Object with information about the updated order property ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":200840400001,
   "error_description":"property does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200840400001` | The order property to be updated was not found ||
|| `200850000003` | Internal error updating the property ||
|| `200850000009` | An error occurs when trying to update an order property with the `multiple` parameter set to `Y`, if the `isFiltered` parameter is not provided.
Filtering by multiple order properties is not supported ||
|| `200850000010` | An error occurs when trying to update an order property with the `multiple` parameter set to `Y`, if the `isFiltered` parameter is not equal to `N`.
Filtering by multiple order properties is not supported ||
|| `200850000011` | An error occurs when trying to update an order property of type `LOCATION` with the `isLocation` parameter set to `Y`, if the `multiple` parameter is not specified.
Multiplicity is not supported for order properties marked with the `isLocation` indicator ||
|| `200850000012` | An error occurs when trying to update an order property of type `LOCATION` with the `isLocation` parameter set to `Y`, if the `multiple` parameter is not equal to `N`.
Multiplicity is not supported for order properties marked with the `isLocation` indicator ||
|| `200850000013` | An error occurs when trying to update an order property of type `LOCATION` with the `isLocation4tax` parameter set to `Y`, if the `multiple` parameter is not specified.
Multiplicity is not supported for order properties marked with the `isLocation4tax` indicator ||
|| `200850000014` | An error occurs when trying to update an order property of type `LOCATION` with the `isLocation4tax` parameter set to `Y`, if the `multiple` parameter is not equal to `N`.
Multiplicity is not supported for order properties marked with the `isLocation4tax` indicator ||
|| `200850000015` | An error occurs when trying to update an order property of type `STRING` with the `isProfileName` parameter set to `Y`, if the `required` parameter is not specified.
The profile name is mandatory and cannot be empty ||
|| `200850000016` | An error occurs when trying to update an order property of type `STRING` with the `isProfileName` parameter set to `Y`, if the `required` parameter is not equal to `Y`.
The profile name is mandatory and cannot be empty ||
|| `200040300020` | Insufficient permissions to update the order property ||
|| `100` | The `id` parameter is not specified ||
|| `100` | The `fields` parameter is not specified or is empty ||
|| `0` | Required fields are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-add.md)
- [{#T}](./sale-property-get.md)
- [{#T}](./sale-property-list.md)
- [{#T}](./sale-property-delete.md)
- [{#T}](./sale-property-get-fields-by-type.md)