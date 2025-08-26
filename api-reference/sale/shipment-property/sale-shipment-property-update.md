# Update Shipment Property sale.shipmentproperty.update

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the shipment property.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_shipment_property.id`](../data-types.md) | Identifier of the shipment property ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the shipment property ||
|#

### Parameter fields

General parameters applicable to shipment properties of any type:

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the shipment property ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the shipment property ||
|| **active**
[`string`](../../data-types.md) | Indicator of whether the shipment property is active.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `Y` ||
|| **util**
[`string`](../../data-types.md) | Indicator of whether the shipment property is a system property. System properties are not displayed in the public part.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
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
[`string`](../../data-types.md) | Description of the shipment property ||
|| **required**
[`string`](../../data-types.md) | Indicator of whether filling in the shipment property value is mandatory.
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
[`string`](../../data-types.md) | External identifier of the shipment property ||
|| **defaultValue**
[`any`](../data-types.md) | Default value of the shipment property.
For multiple shipment properties (multiple), an array of values is supported ||
|| **settings**
[`object`](../../data-types.md) | An object in the format {"field_1": "value_1", ... "field_N": "value_N"} for passing additional settings for the shipment property.

The list of supported keys for this object depends on the property type. For some property types (e.g., Y/N), additional properties are not provided. The description of the **settings** parameter for different property types is provided in the description of the method [`sale.shipmentproperty.add`](sale-shipment-property-add.md) ||
|#

Parameters applicable to order properties of type `STRING`

#|
|| **Name**
`type` | **Description** ||
|| **isProfileName**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as the user's profile name.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isPayer**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as the payer's name.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isEmail**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as an e-mail (e.g., when registering a new user during order placement).
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isPhone**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as a phone number.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isZip**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as a postal code.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isAddress**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as an address.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|#

Parameters applicable to shipment properties of type [`LOCATION`](../data-types.md)

#|
|| **Name**
`type` | **Description** ||
|| **isLocation**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as the customer's location for calculating delivery costs.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isLocation4tax**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as the customer's location for determining tax rates.
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
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as the customer's address from which the order needs to be picked up for calculating delivery costs.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isAddressTo**
[`string`](../../data-types.md) | Indicator of whether the value of this shipment property should be used as the customer's address to which the order needs to be delivered for calculating delivery costs.
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
    https://**put_your_bitrix24_address**/rest/sale.shipmentproperty.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":93,"fields":{"personTypeId":3,"propsGroupId":6,"name":"Phone (for contacting the courier)","type":"STRING","code":"PHONE","active":"Y","util":"N","userProps":"Y","isFiltered":"N","sort":500,"description":"property description","required":"Y","multiple":"N","settings":{"multiline":"Y","maxlength":100},"xmlId":"","defaultValue":"","isProfileName":"Y","isPayer":"Y","isEmail":"N","isPhone":"N","isZip":"N","isAddress":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentproperty.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.shipmentproperty.update', {
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
                'sale.shipmentproperty.update',
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
                            'maxlength' => 100
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
        echo 'Error updating shipment property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.shipmentproperty.update', {
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
        'sale.shipmentproperty.update',
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
[`sale_shipment_property`](../data-types.md) | Object with information about the updated shipment property ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
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
|| `200840400001` | The updated shipment property was not found ||
|| `200850000003` | Internal error updating the property ||
|| `200850000009` | Error occurs when trying to update a shipment property with the `multiple` parameter set to `Y`, if the `isFiltered` parameter is not provided.
Filtering by multiple shipment properties is not supported ||
|| `200850000010` | Error occurs when trying to update a shipment property with the `multiple` parameter set to `Y`, if the `isFiltered` parameter value is not `N`.
Filtering by multiple shipment properties is not supported ||
|| `200850000011` | Error occurs when trying to update a shipment property of type `LOCATION` with the `isLocation` parameter set to `Y`, if the `multiple` parameter value is not specified.
Multiplicity is not supported for shipment properties marked with the `isLocation` indicator ||
|| `200850000012` | Error occurs when trying to update a shipment property of type `LOCATION` with the `isLocation` parameter set to `Y`, if the `multiple` parameter value is not `N`.
Multiplicity is not supported for shipment properties marked with the `isLocation` indicator ||
|| `200850000013` | Error occurs when trying to update a shipment property of type `LOCATION` with the `isLocation4tax` parameter set to `Y`, if the `multiple` parameter value is not specified.
Multiplicity is not supported for shipment properties marked with the `isLocation4tax` indicator ||
|| `200850000014` | Error occurs when trying to update a shipment property of type `LOCATION` with the `isLocation4tax` parameter set to `Y`, if the `multiple` parameter value is not `N`.
Multiplicity is not supported for shipment properties marked with the `isLocation4tax` indicator ||
|| `200850000015` | Error occurs when trying to update a shipment property of type `STRING` with the `isProfileName` parameter set to `Y`, if the `required` parameter value is not specified.
Profile name is mandatory and cannot be empty ||
|| `200850000016` | Error occurs when trying to update a shipment property of type `STRING` with the `isProfileName` parameter set to `Y`, if the `required` parameter value is not `Y`.
Profile name is mandatory and cannot be empty ||
|| `200040300020` | Insufficient permissions to update the shipment property ||
|| `100` | The `id` parameter is not specified ||
|| `100` | The `fields` parameter is not specified or is empty ||
|| `0` | Required fields are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-property-add.md)
- [{#T}](./sale-shipment-property-get.md)
- [{#T}](./sale-shipment-property-list.md)
- [{#T}](./sale-shipment-property-delete.md)
- [{#T}](./sale-shipment-property-get-fields-by-type.md)