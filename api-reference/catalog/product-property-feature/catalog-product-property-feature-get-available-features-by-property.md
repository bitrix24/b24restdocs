# Get Available Product Property Features or Variations catalog.productPropertyFeature.getAvailableFeaturesByProperty

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" access permission

The method `catalog.productPropertyFeature.getAvailableFeaturesByProperty` returns a list of available parameters for the specified product property or variation.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **propertyId***
[`catalog_product_property.id`](../data-types.md#catalog_product_property) | Identifier of the product property or variation.

Property identifiers can be obtained using the [catalog.productProperty.list](../product-property/catalog-product-property-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"propertyId":901}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productPropertyFeature.getAvailableFeaturesByProperty
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"propertyId":901,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productPropertyFeature.getAvailableFeaturesByProperty
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'catalog.productPropertyFeature.getAvailableFeaturesByProperty',
            {
                propertyId: 901
            }
        );

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
                'catalog.productPropertyFeature.getAvailableFeaturesByProperty',
                [
                    'propertyId' => 901
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
        'catalog.productPropertyFeature.getAvailableFeaturesByProperty',
        {
            propertyId: 901
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
        'catalog.productPropertyFeature.getAvailableFeaturesByProperty',
        [
            'propertyId' => 901
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
        "features": [
        {
            "featureId": "LIST_PAGE_SHOW",
            "featureName": "Show on the list page of items",
            "moduleId": "iblock"
        },
        {
            "featureId": "DETAIL_PAGE_SHOW",
            "featureName": "Show on the detail page of the item",
            "moduleId": "iblock"
        }
        ]
    },
    "time": {
        "start": 1774250910,
        "finish": 1774250910.453544,
        "duration": 0.45354390144348145,
        "processing": 0,
        "date_start": "2026-03-23T10:28:30+02:00",
        "date_finish": "2026-03-23T10:28:30+02:00",
        "operating_reset_at": 1774251510,
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
|| **features**
[`array`](../../data-types.md) | Array of objects in the format `{"featureId": "value", "featureName": "value", "moduleId": "value"}`, where:
- `featureId` — code of the property parameter
- `featureName` — name of the property parameter
- `moduleId` — identifier of the module to which the property parameter belongs ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "productPropertyFeature does not exist."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access Denied | Insufficient permissions to view the trade catalog ||
|| `0` | productPropertyFeature does not exist. | The property with the provided `propertyId` was not found or does not belong to the trade catalog ||
|| `100` | Invalid value {abc} to match with parameter {propertyId}. Should be value of type int. | An invalid type value was provided for `propertyId` ||
|| `100` | Could not find value for parameter {propertyId} | The required parameter `propertyId` was not provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-feature-add.md)
- [{#T}](./catalog-product-property-feature-update.md)
- [{#T}](./catalog-product-property-feature-get.md)
- [{#T}](./catalog-product-property-feature-list.md)
- [{#T}](./catalog-product-property-feature-get-fields.md)