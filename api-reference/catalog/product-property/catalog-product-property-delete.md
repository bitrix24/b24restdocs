# Delete product property or variation catalog.productProperty.delete

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: user with permission to view the catalog

The method `catalog.productProperty.delete` removes a product property or variation.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_property.id`](../data-types.md#catalog_product_property) | Identifier of the property. 

Property identifiers can be obtained using the method [catalog.productProperty.list](./catalog-product-property-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":659}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productProperty.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":659,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productProperty.delete
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod('catalog.productProperty.delete', { id: 659 });
        console.log(response.getData().result);
    }
    catch (error) {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.productProperty.delete',
                [
                    'id' => 659
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
        'catalog.productProperty.delete',
        {
            id: 659
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
        'catalog.productProperty.delete',
        ['id' => 659]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773951201,
        "finish": 1773951201.704125,
        "duration": 0.704124927520752,
        "processing": 0,
        "date_start": "2026-03-19T23:13:21+02:00",
        "date_finish": "2026-03-19T23:13:21+02:00",
        "operating_reset_at": 1773951801,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the property was successfully deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "productProperty does not exist."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Access Denied | Insufficient permissions to view the catalog ||
|| `400` | Empty value | productProperty does not exist | Property with the specified `id` not found ||
|| `400` | `0` | The specified property does not belong to a product catalog | Property does not belong to the product catalog ||
|| `400` | `0` | Error deleting product property | Internal error while deleting the property ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-add.md)
- [{#T}](./catalog-product-property-update.md)
- [{#T}](./catalog-product-property-get.md)
- [{#T}](./catalog-product-property-list.md)
- [{#T}](./catalog-product-property-get-fields.md)