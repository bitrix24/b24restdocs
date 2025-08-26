# Remove Property Relation with sale.propertyRelation.deleteByFilter

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.propertyRelation.deleteByFilter` removes the property relation.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for removing the property relation ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **entityId***
[`integer`](../../data-types.md) | Entity identifier ||
|| **entityType***
[`string`](../../data-types.md) | Entity type:
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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyRelation.deleteByFilter
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"entityId":6,"entityType":"D","propertyId":40},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyRelation.deleteByFilter
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.propertyRelation.deleteByFilter', 
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
    catch(error)
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
                'sale.propertyRelation.deleteByFilter',
                [
                    'fields' => [
                        'entityId'    => 6,
                        'entityType'  => 'D',
                        'propertyId'  => 40
                    ]
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
        echo 'Error deleting property relation: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.propertyRelation.deleteByFilter', 
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
        'sale.propertyRelation.deleteByFilter',
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
    "result": true,
    "time": {
        "start": 1712301886.43654,
        "finish": 1712301886.884087,
        "duration": 0.44754719734191895,
        "processing": 0.040498971939086914,
        "date_start": "2024-04-05T10:24:46+02:00",
        "date_finish": "2024-04-05T10:24:46+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the property relation removal ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":201640400004,
    "error_description":"property relation does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201640400004` | Property relation with the specified parameters not found ||
|| `200040300010` | Insufficient permissions to delete the property relation ||
|| `100` | Parameter `fields` is not specified or is empty ||
|| `0` | Required parameter not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-property-relation-add.md)
- [{#T}](./sale-property-relation-list.md)
- [{#T}](./sale-property-relation-get-fields.md)