# Delete Trade Catalog catalog.catalog.delete

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a trade catalog.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the trade catalog ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":24}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.catalog.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":24,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.catalog.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.catalog.delete',
        {
            id: 24,
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
        'catalog.catalog.delete',
        [
            'id' => 24
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
        "start": 1716369501.544535,
        "finish": 1716369507.424352,
        "duration": 5.879817008972168,
        "processing": 5.454736948013306,
        "date_start": "2024-05-22T12:18:21+03:00",
        "date_finish": "2024-05-22T12:18:27+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the trade catalog deletion ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{	
    "error":200040300020,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to delete the trade catalog
|| 
|| `200040300020` | Insufficient permissions to delete the trade catalog
|| 
|| `200040300030` | Insufficient permissions to delete the trade catalog
|| 
|| `0` | The trade catalog is a trade catalog of trade offers
|| 
|| `0` | The trade catalog does not exist
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-catalog-add.md)
- [{#T}](./catalog-catalog-update.md)
- [{#T}](./catalog-catalog-get.md)
- [{#T}](./catalog-catalog-list.md)
- [{#T}](./catalog-catalog-is-offers.md)
- [{#T}](./catalog-catalog-get-fields.md)