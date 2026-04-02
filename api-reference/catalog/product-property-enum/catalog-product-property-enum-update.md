# Update the Value of the List Property catalog.productPropertyEnum.update

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" permission and the right to modify the information block property.

The method `catalog.productPropertyEnum.update` updates the value of a list property.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the list property value.

The identifier can be obtained using the [catalog.productPropertyEnum.list](./catalog-product-property-enum-list.md) method. ||
|| **fields***
[`object`](../../data-types.md) | Set of fields for the updated list value [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **propertyId***
[`catalog_product_property.id`](../data-types.md#catalog_product_property) | Identifier of the product or variation property.

Property identifiers can be obtained using the [catalog.productProperty.list](../product-property/catalog-product-property-list.md) method. ||
|| **value***
[`string`](../../data-types.md) | Value of the list item. ||
|| **xmlId***
[`string`](../../data-types.md) | External identifier of the list value. Must be unique within the property. ||
|| **def**
[`char`](../../data-types.md) | Indicator of the default value. Acceptable values:
- `Y` — default
- `N` — not default ||
|| **sort**
[`integer`](../../data-types.md) | Sort index. ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":1739,"fields":{"propertyId":431,"value":"Medium","xmlId":"M","def":"N","sort":110}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productPropertyEnum.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":1739,"fields":{"propertyId":431,"value":"Medium","xmlId":"M","def":"N","sort":110},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productPropertyEnum.update
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod('catalog.productPropertyEnum.update', {
            id: 1739,
            fields: {
                propertyId: 431,
                value: 'Medium',
                xmlId: 'M',
                def: 'N',
                sort: 110,
            }
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
                'catalog.productPropertyEnum.update',
                [
                    'id' => 1739,
                    'fields' => [
                        'propertyId' => 431,
                        'value' => 'Medium',
                        'xmlId' => 'M',
                        'def' => 'N',
                        'sort' => 110,
                    ],
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
        'catalog.productPropertyEnum.update',
        {
            id: 1739,
            fields: {
                propertyId: 431,
                value: 'Medium',
                xmlId: 'M',
                def: 'N',
                sort: 110,
            }
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
        'catalog.productPropertyEnum.update',
        [
            'id' => 1739,
            'fields' => [
                'propertyId' => 431,
                'value' => 'Medium',
                'xmlId' => 'M',
                'def' => 'N',
                'sort' => 110,
            ]
        ]
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
        "def": "N",
        "id": 1739,
        "propertyId": 431,
        "sort": 110,
        "value": "Medium",
        "xmlId": "M"
        }
    },
    "time": {
        "start": 1774339029,
        "finish": 1774339030.119726,
        "duration": 1.1197259426116943,
        "processing": 1,
        "date_start": "2026-03-24T10:57:09+01:00",
        "date_finish": "2026-03-24T10:57:10+01:00",
        "operating_reset_at": 1774339629,
        "operating": 0.1804349422454834
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root object of the response. ||
|| **productPropertyEnum**
[`catalog_product_property_enum`](../data-types.md#catalog_product_property_enum) | Object of the updated list property value. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
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
|| `0` | Access Denied | Insufficient permissions to modify the information block property. ||
|| `0` | productPropertyEnum does not exist. | The list property value with the provided `id` was not found or does not belong to the product catalog. ||
|| `0` | The specified property does not belong to a product catalog | The property with the provided `propertyId` does not belong to the product catalog. ||
|| `0` | Required fields: xmlId | The required field `xmlId` was not provided. ||
|| `0` | Internal error updating enumeration value. Try updating again. | An internal error occurred while updating the list value. ||
|| `100` | Could not find value for parameter {id} | The required parameter `id` was not provided. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-enum-add.md)
- [{#T}](./catalog-product-property-enum-get.md)
- [{#T}](./catalog-product-property-enum-list.md)
- [{#T}](./catalog-product-property-enum-delete.md)
- [{#T}](./catalog-product-property-enum-get-fields.md)