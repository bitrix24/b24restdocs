# Get Values of All Fields in the Trade Catalog catalog.catalog.get

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method returns the values of all fields in the trade catalog.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the trade catalog ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":23}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.catalog.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":23,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.catalog.get
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.catalog.get', {
            'id': 23
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
        'catalog.catalog.get',
        [
            'id' => 23
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "catalog": {
            "iblockId": 23,
            "iblockTypeId": 0,
            "id": 23,
            "lid": "s1",
            "name": "Products QuickBooks and other similar platforms",
            "productIblockId": null,
            "skuPropertyId": null,
            "subscription": "N",
            "vatId": null
        }
    },
    "time": {
        "start": 1716390151.446282,
        "finish": 1716390151.902625,
        "duration": 0.4563431739807129,
        "processing": 0.016014814376831055,
        "date_start": "2024-05-22T18:02:31+02:00",
        "date_finish": "2024-05-22T18:02:31+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **catalog**
[`catalog_catalog`](../data-types.md#catalog_catalog) | Object containing information about the trade catalog ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read the trade catalog
|| 
|| `200040300030` | Insufficient permissions to read the trade catalog
|| 
|| `100` | Parameter `id` is missing
|| 
|| `0` | Trade catalog with the specified identifier does not exist
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-catalog-add.md)
- [{#T}](./catalog-catalog-update.md)
- [{#T}](./catalog-catalog-list.md)
- [{#T}](./catalog-catalog-is-offers.md)
- [{#T}](./catalog-catalog-delete.md)
- [{#T}](./catalog-catalog-get-fields.md)