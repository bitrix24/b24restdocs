# Get Values of All Status Fields by ID sale.status.get

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves the values of all fields of the status.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_status.id`](../data-types.md) | Character identifier of the status ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"N"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.status.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"N","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.status.get
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.status.get", {
            "id": "N"
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

    $result = CRest::call('sale.status.get', [
        'id' => 'N'
    ]);

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result":{
        "status":{
            "color":"#BEEDF1",
            "id":"N",
            "notify":"Y",
            "sort":10,
            "type":"O",
            "xmlId":null
        }
    },
    "time":{
        "start":1712144798.388861,
        "finish":1712144798.636187,
        "duration":0.24732613563537598,
        "processing":0.012362003326416016,
        "date_start":"2024-04-03T14:46:38+03:00",
        "date_finish":"2024-04-03T14:46:38+03:00"
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
[`sale_status`](../data-types.md) | Status information ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":201340400001,
    "error_description":"status does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201340400001` | Status not found ||
|| `200040300010` | Insufficient permissions to read the status ||
|| `100` | Parameter `id` not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-status-add.md)
- [{#T}](./sale-status-update.md)
- [{#T}](./sale-status-list.md)
- [{#T}](./sale-status-delete.md)
- [{#T}](./sale-status-get-fields.md)