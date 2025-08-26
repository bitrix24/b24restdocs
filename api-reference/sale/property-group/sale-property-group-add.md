# Create Property Group sale.propertygroup.add

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method creates a property group.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a property group ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **personTypeId***
[`sale_person_type.id`](../data-types.md) | Identifier of the payer type ||
|| **name***
[`string`](../../data-types.md) | Name of the property group ||
|| **sort**
[`integer`](../../data-types.md) | Sorting ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"personTypeId":3,"name":"New Property Group","sort":100}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertygroup.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"personTypeId":3,"name":"New Property Group","sort":100},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertygroup.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.propertygroup.add", {
    			"fields": {
    				"personTypeId": 3,
    				"name": "New Property Group",
    				"sort": 100,
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
                'sale.propertygroup.add',
                [
                    'fields' => [
                        'personTypeId' => 3,
                        'name'        => 'New Property Group',
                        'sort'        => 100,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding property group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.propertygroup.add", {
            "fields": {
                "personTypeId": 3,
                "name": "New Property Group",
                "sort": 100,
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
        'sale.propertygroup.add',
        [
            'fields' => [
                'personTypeId' => 3,
                'name' => 'New Property Group',
                'sort' => 100,
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
        "propertyGroup": {
            "id": 16,
            "name": "New Property Group",
            "personTypeId": 3,
            "sort": 100
        }
    },
    "time": {
        "start": 1711451989.729911,
        "finish": 1711451989.907491,
        "duration": 0.1775798797607422,
        "processing": 0.008534908294677734,
        "date_start": "2024-03-26T14:19:49+02:00",
        "date_finish": "2024-03-26T14:19:49+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **propertyGroup**
[`sale_order_property_group`](../data-types.md) | Object with information about the added property group ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description": "Required fields: name"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200950000006` | An empty value was passed for one of the required fields ||
|| `200040300020` | Insufficient permissions to add a property group ||
|| `100` | The `fields` parameter is not specified or is empty ||
|| `0` | Required fields are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-group-update.md)
- [{#T}](./sale-property-group-get.md)
- [{#T}](./sale-property-group-list.md)
- [{#T}](./sale-property-group-delete.md)
- [{#T}](./sale-property-group-get-fields.md)