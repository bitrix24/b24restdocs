# Add Property Binding sale.propertyRelation.add

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.propertyRelation.add` adds a property binding.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating the property binding ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **entityId***
[`integer`](../../data-types.md) | Object identifier ||
|| **entityType***
[`string`](../../data-types.md) | Object type:
- `P` — payment system
- `D` — delivery
- `L` — landing
- `T` — trading platform ||
|| **propertyId***
[`sale_order_property.id`](../data-types.md) | Property identifier ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"entityId":6,"entityType":"D","propertyId":40}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyRelation.add
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"entityId":6,"entityType":"D","propertyId":40},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyRelation.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.propertyRelation.add',
    		{
    			fields: {
    				entityId: 6,
    				entityType: 'D',
    				propertyId: 40
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
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
                'sale.propertyRelation.add',
                [
                    'fields' => [
                        'entityId'    => 6,
                        'entityType'  => 'D',
                        'propertyId'  => 40,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding property relation: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.propertyRelation.add',
        {
            fields: {
                entityId: 6,
                entityType: 'D',
                propertyId: 40
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.propertyRelation.add',
        [
            'fields' => [
                'entityId' => 6,
                'entityType' => 'D',
                'propertyId' => 40
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "propertyRelation": {
            "entityId": 6,
            "entityType": "D",
            "propertyId": 40
        }
    },
    "time": {
        "start": 1712244475.495277,
        "finish": 1712244476.402808,
        "duration": 0.9075310230255127,
        "processing": 0.08538603782653809,
        "date_start": "2024-04-04T18:27:55+02:00",
        "date_finish": "2024-04-04T18:27:56+02:00"
    }
}
```

### Returned Data

#|
|| **Value** / **Type** | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response
 ||
|| **propertyRelations**
[`sale_order_property_relation`](../data-types.md) | Object with information about the created binding ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":0,
    "error_description":"Required fields: entityId"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201650000001` | Binding with the specified values `entityId`, `entityType`, `propertyId` already exists
 ||
|| `201650000002` | Property does not exist. Invalid value for the provided parameter `propertyId` || 
|| `200040300020` | Insufficient permissions to create property binding || 
|| `100` | Parameter `fields` is not specified or is empty || 
|| `0` | Required fields are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-property-relation-list.md)
- [{#T}](./sale-property-relation-delete-by-filter.md)
- [{#T}](./sale-property-relation-get-fields.md)
- [{#T}](../../../tutorials/sale/delivery-in-crm.md)