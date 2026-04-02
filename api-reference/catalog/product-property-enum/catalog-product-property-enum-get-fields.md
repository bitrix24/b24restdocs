# Get Fields of List Property Values catalog.productPropertyEnum.getFields

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" access permission

The method `catalog.productPropertyEnum.getFields` returns the description of the fields for list property values.

## Method Parameters

No parameters.

## Code Examples

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productPropertyEnum.getFields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productPropertyEnum.getFields
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod('catalog.productPropertyEnum.getFields', {});
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
                'catalog.productPropertyEnum.getFields',
                []
            );

        print_r($response->getResponseData()->getResult());
    } catch (\Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.productPropertyEnum.getFields',
        {},
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
        'catalog.productPropertyEnum.getFields',
        []
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "productPropertyEnum": {
        "def": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "char"
        },
        "id": {
            "isImmutable": false,
            "isReadOnly": true,
            "isRequired": false,
            "type": "integer"
        },
        "propertyId": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": true,
            "type": "integer"
        },
        "sort": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "integer"
        },
        "value": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": true,
            "type": "string"
        },
        "xmlId": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": true,
            "type": "string"
        }
        }
    },
    "time": {
        "start": 1774339251,
        "finish": 1774339251.898047,
        "duration": 0.8980469703674316,
        "processing": 0,
        "date_start": "2026-03-24T11:00:51+01:00",
        "date_finish": "2026-03-24T11:00:51+01:00",
        "operating_reset_at": 1774339851,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root object of the response ||
|| **productPropertyEnum**
[`object`](../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the identifier of the object [catalog_product_property_enum](../data-types.md#catalog_product_property_enum), and `value` is an object of type [rest_field_description](../data-types.md#rest_field_description) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access Denied | Insufficient rights to view the product catalog ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-enum-add.md)
- [{#T}](./catalog-product-property-enum-update.md)
- [{#T}](./catalog-product-property-enum-get.md)
- [{#T}](./catalog-product-property-enum-list.md)
- [{#T}](./catalog-product-property-enum-delete.md)