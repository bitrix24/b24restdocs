# Delete the Value of the List Property catalog.productPropertyEnum.delete

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" access permission

The method `catalog.productPropertyEnum.delete` removes the value of a list property.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the list property value.

The identifier can be obtained using the [catalog.productPropertyEnum.list](./catalog-product-property-enum-list.md) method ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":122}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productPropertyEnum.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":122,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productPropertyEnum.delete
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod('catalog.productPropertyEnum.delete', {
            id: 122,
        });

        console.log(response.getData().result);
    } catch (error) {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.productPropertyEnum.delete',
                [
                    'id' => 122,
                ]
            );

        print_r($response->getResponseData()->getResult());
    } catch (\Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.productPropertyEnum.delete',
        {
            id: 122,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.productPropertyEnum.delete',
        [
            'id' => 122,
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1774257804,
        "finish": 1774257804.239771,
        "duration": 0.23977112770080566,
        "processing": 0,
        "date_start": "2026-03-23T12:23:24+01:00",
        "date_finish": "2026-03-23T12:23:24+01:00",
        "operating_reset_at": 1774258404,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the deletion operation. `true` — value deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "productPropertyEnum does not exist."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access Denied | Insufficient rights to view the product catalog ||
|| `0` | productPropertyEnum does not exist. | The list property value with the provided `id` was not found or does not belong to the product catalog ||
|| `0` | Internal error deleting enumeration value. Try deleting again. | Internal error while deleting the list value ||
|| `100` | Could not find value for parameter {id} | Required parameter `id` was not provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-enum-add.md)
- [{#T}](./catalog-product-property-enum-update.md)
- [{#T}](./catalog-product-property-enum-get.md)
- [{#T}](./catalog-product-property-enum-list.md)
- [{#T}](./catalog-product-property-enum-get-fields.md)