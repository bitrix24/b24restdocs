# Update Shipment Property Values sale.shipmentpropertyvalue.modify

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the shipment property values.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | The root element that carries the request parameters ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **shipment***
[`object`](../../data-types.md) | The object containing the shipment ID and shipment property values (detailed description provided [below](#shipment)) ||
|#

### Parameter shipment {#shipment}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_shipment.id`](../data-types.md#sale_order_shipment) | Shipment ID ||
|| **propertyValues***
[`object[]`](../../data-types.md) | An array of objects containing the shipment property ID and property value (detailed description provided [below](#propertyvalues)) ||
|#

### Parameter propertyValues {#propertyvalues}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **shipmentPropsId***
[`sale_shipment_property.id`](../data-types.md#sale_shipment_property) | Shipment property ID ||
|| **value***
[`string`](../../data-types.md) | Property value ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"shipment":{"id":4120,"propertyValues":[{"shipmentPropsId":105,"value":"Comments value"},{"shipmentPropsId":106,"value":"Description value"}]}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentpropertyvalue.modify
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"shipment":{"id":4120,"propertyValues":[{"shipmentPropsId":105,"value":"Comments value"},{"shipmentPropsId":106,"value":"Description value"}]}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentpropertyvalue.modify
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.shipmentpropertyvalue.modify', {
    			fields: {
    				shipment: {
    					id: 4120,
    					propertyValues: [{
    							shipmentPropsId: 105,
    							value: 'Comments value'
    						},
    						{
    							shipmentPropsId: 106,
    							value: 'Description value'
    						},
    					],
    				},
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
                'sale.shipmentpropertyvalue.modify',
                [
                    'fields' => [
                        'shipment' => [
                            'id'            => 4120,
                            'propertyValues' => [
                                [
                                    'shipmentPropsId' => 105,
                                    'value'           => 'Comments value'
                                ],
                                [
                                    'shipmentPropsId' => 106,
                                    'value'           => 'Description value'
                                ],
                            ],
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error modifying shipment property value: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.shipmentpropertyvalue.modify', {
            fields: {
                shipment: {
                    id: 4120,
                    propertyValues: [{
                            shipmentPropsId: 105,
                            value: 'Comments value'
                        },
                        {
                            shipmentPropsId: 106,
                            value: 'Description value'
                        },
                    ],
                },
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
        'sale.shipmentpropertyvalue.modify',
        [
            'fields' => [
                'shipment' => [
                    'id' => 4120,
                    'propertyValues' => [
                        [
                            'shipmentPropsId' => 105,
                            'value' => 'Comments value'
                        ],
                        [
                            'shipmentPropsId' => 106,
                            'value' => 'Description value'
                        ],
                    ],
                ],
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
    "result": {
        "propertyValues": [
            {
                "code": null,
                "id": 38164,
                "name": "Comments",
                "value": "Comments value"
            },
            {
                "code": null,
                "id": 38165,
                "name": "Description",
                "value": "Description value"
            }
        ]
    },
    "time": {
        "start": 1718022201.149589,
        "finish": 1718022201.726496,
        "duration": 0.5769069194793701,
        "processing": 0.38397693634033203,
        "date_start": "2024-06-10T15:23:21+02:00",
        "date_finish": "2024-06-10T15:23:21+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **propertyValues**
[`sale_shipment_property_value[]`](../data-types.md#sale_shipment_property_value) | An array of objects containing information about shipment property values ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 100,
    "error_description": "Could not find value for parameter {fields}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to modify property values ||
|| `100` | Required parameters are missing ||
|| `201040400006` | Invalid shipment ID ||
|| `201040400007` | Shipment not found ||
|| `201040400008` | Error saving order ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-shipment-property-value-get.md)
- [{#T}](./sale-shipment-property-value-list.md)
- [{#T}](./sale-shipment-propertyvalue-delete.md)
- [{#T}](./sale-shipment-property-value-get-fields.md)