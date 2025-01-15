# Delete Status sale.status.delete

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes an order or delivery status.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_status.id`](../data-types.md) | Status identifier ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"MS"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.status.delete
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"MS","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.status.delete
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.status.delete", {
            "id": "MS"
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

    $result = CRest::call('sale.status.delete', [
        'id' => 'MS'
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
    "result":true,
    "time":{
        "start":1712144469.892859,
        "finish":1712144470.162582,
        "duration":0.26972293853759766,
        "processing":0.013709068298339844,
        "date_start":"2024-04-03T14:41:09+02:00",
        "date_finish":"2024-04-03T14:41:10+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the status deletion ||
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
|| `201340400001` | The status to be deleted was not found ||
|| `200040300020` | Insufficient permissions to delete the status ||
|| `201350000002` | An error occurs when attempting to delete certain [system statuses](./index.md):
- `N`
- `F`
- `DN`
- `DF`
||
|| `100` | The `id` parameter is missing ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-status-add.md)
- [{#T}](./sale-status-update.md)
- [{#T}](./sale-status-get.md)
- [{#T}](./sale-status-list.md)
- [{#T}](./sale-status-get-fields.md)