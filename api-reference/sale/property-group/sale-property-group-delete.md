# Delete Property Group sale.propertygroup.delete

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a property group.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_property_group.id`](../data-types.md) | Identifier of the property group ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertygroup.delete
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertygroup.delete
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.propertygroup.delete", {
            "id": 15
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
        'sale.propertygroup.delete',
        [
            'id' => 15
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
    "result":true,
    "time":{
        "start":1711454254.569172,
        "finish":1711454254.795907,
        "duration":0.22673511505126953,
        "processing":0.03125,
        "date_start":"2024-03-26T14:57:34+03:00",
        "date_finish":"2024-03-26T14:57:34+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the property group deletion ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{ 
    "error":200940400001,
    "error_description":"property group does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200940400001` | The property group to be deleted was not found ||
|| `200040300020` | Insufficient permissions to delete the property group ||
|| `100` | The `id` parameter is missing ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-group-add.md)
- [{#T}](./sale-property-group-update.md)
- [{#T}](./sale-property-group-get.md)
- [{#T}](./sale-property-group-list.md)
- [{#T}](./sale-property-group-get-fields.md)