# Create status sale.status.add

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method creates a status for an order or delivery.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating the status ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_status.id`](../data-types.md) | Symbolic identifier of the status.

The status identifier must be unique regardless of its type. This means you cannot add two statuses with the same identifier (one for an order and another for delivery).
||
|| **type***
[`string`](../../data-types.md) | Type of status:
- `O` — order status
- `D` — delivery status ||
|| **notify**
[`string`](../../data-types.md) | Indicator of whether to send an email notification to the user when the order or delivery transitions to this status.

Possible values:
- `Y` — notify
- `N` — do not notify
 ||
|| **sort**
[`integer`](../../data-types.md) | Sorting ||
|| **color**
[`string`](../../data-types.md) | HEX color code of the status (e.g., `#FF0000`) ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the status.

Can be used for synchronization with the order or delivery status by identifier in an external system.
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"id":"MS","type":"O","notify":"Y","sort":500,"color":"#FF0000","xmlId":"myStatusXmlId"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.status.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"id":"MS","type":"O","notify":"Y","sort":500,"color":"#FF0000","xmlId":"myStatusXmlId"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.status.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.status.add', {
    			fields: {
    				id: 'MS',
    				type: 'O',
    				notify: 'Y',
    				sort: 500,
    				color: '#FF0000',
    				xmlId: 'myStatusXmlId',
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
                'sale.status.add',
                [
                    'fields' => [
                        'id'     => 'MS',
                        'type'   => 'O',
                        'notify' => 'Y',
                        'sort'   => 500,
                        'color'  => '#FF0000',
                        'xmlId'  => 'myStatusXmlId',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding status: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.status.add', {
            fields: {
                id: 'MS',
                type: 'O',
                notify: 'Y',
                sort: 500,
                color: '#FF0000',
                xmlId: 'myStatusXmlId',
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

    $result = CRest::call('sale.status.add', [
        'fields' => [
            'id' => 'MS',
            'type' => 'O',
            'notify' => 'Y',
            'sort' => 500,
            'color' => '#FF0000',
            'xmlId' => 'myStatusXmlId',
        ]
    ]);

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
        "status": {
            "color": "#FF0000",
            "id": "MS",
            "notify": "Y",
            "sort": 500,
            "type": "O",
            "xmlId": "myStatusXmlId"
        }
    },
    "time": {
        "start": 1712137817.343984,
        "finish": 1712137817.605804,
        "duration": 0.26182007789611816,
        "processing": 0.018325090408325195,
        "date_start": "2024-04-03T12:50:17+02:00",
        "date_finish": "2024-04-03T12:50:17+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **status**
[`sale_status`](../data-types.md) | Object with information about the added status ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 201350000001,
    "error_description": "Duplicate entry for key [id]"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201350000001` | Status with the provided identifier already exists ||
|| `201350000003` | Status type value not provided or the provided value is incorrect ||
|| `201350000004` | Empty status identifier value provided ||
|| `201350000005` | Status identifier length limit exceeded ||
|| `200040300020` | Insufficient permissions to add status ||
|| `100` | Parameter `fields` not specified or empty ||
|| `0` | Required fields not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-status-update.md)
- [{#T}](./sale-status-get.md)
- [{#T}](./sale-status-list.md)
- [{#T}](./sale-status-delete.md)
- [{#T}](./sale-status-get-fields.md)