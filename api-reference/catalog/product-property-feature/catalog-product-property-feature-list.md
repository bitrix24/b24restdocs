# Get a List of Product Property Feature Parameters or Variations catalog.productPropertyFeature.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" access permission

The method `catalog.productPropertyFeature.list` returns a list of product property feature parameters and variations based on the filter.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see the fields of the object [catalog_product_property_features](../data-types.md#catalog_product_property_features)).

If the parameter is not provided, all fields will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected parameters in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_property_features](../data-types.md#catalog_product_property_features).

An additional prefix can be assigned to the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value
- `=%` — LIKE, substring search. The `%` symbol should be included in the value
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equals, exact match (used by default)
- `!=` — not equal
- `!` — not equal

If `propertyId` is not provided, the method selects parameters for all properties of the trade catalogs. If a `propertyId` is provided that does not exist or does not relate to the trade catalog, the method will return an empty list. ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected parameters in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_property_features](../data-types.md#catalog_product_property_features).

Possible values for `order`:
- `ASC` — in ascending order
- `DESC` — in descending order

If the parameter is not provided, sorting `id ASC` is applied. ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — `100`, and so on.

The formula for calculating the `start` parameter value: `start = (N - 1) * 50`, where `N` is the desired page number.

If the value `-1` is passed, the response will not include the `total` field. ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"select":["id","propertyId","moduleId","featureId","isEnabled"],"filter":{"propertyId":901},"order":{"id":"ASC"}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productPropertyFeature.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"select":["id","propertyId","moduleId","featureId","isEnabled"],"filter":{"propertyId":901},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productPropertyFeature.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory load.

    try {
    const response = await $b24.callListMethod(
        'catalog.productPropertyFeature.list',
        {
        select: ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
        filter: { propertyId: 901 },
        order: { id: 'ASC' }
        },
        (progress: number) => { console.log('Progress:', progress) }
    );
    const items = response.getData() || [];
    for (const entity of items) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // fetchListMethod: Selects data in parts using an iterator. Use for large volumes of data for efficient memory consumption.

    try {
    const generator = $b24.fetchListMethod('catalog.productPropertyFeature.list', {
        select: ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
        filter: { propertyId: 901 },
        order: { id: 'ASC' }
    }, 'ID');
    for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
    }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.

    try {
    const response = await $b24.callMethod('catalog.productPropertyFeature.list', {
        select: ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
        filter: { propertyId: 901 },
        order: { id: 'ASC' }
    }, 0);
    const result = response.getData().result || [];
    for (const entity of result) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.productPropertyFeature.list',
                [
                    'select' => ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
                    'filter' => [
                        'propertyId' => 901,
                    ],
                    'order' => ['id' => 'ASC'],
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
        'catalog.productPropertyFeature.list',
        {
            select: ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
            filter: {
                propertyId: 901
            },
            order: { id: 'ASC' }
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
        'catalog.productPropertyFeature.list',
        [
            'select' => ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
            'filter' => ['propertyId' => 901],
            'order' => ['id' => 'ASC'],
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
        "productPropertyFeatures": [
        {
            "featureId": "DETAIL_PAGE_SHOW",
            "id": 99,
            "isEnabled": "Y",
            "moduleId": "iblock",
            "propertyId": 901
        },
        {
            "featureId": "LIST_PAGE_SHOW",
            "id": 101,
            "isEnabled": "Y",
            "moduleId": "iblock",
            "propertyId": 901
        }
        ]
    },
    "total": 2,
    "time": {
        "start": 1774254804,
        "finish": 1774254805.034701,
        "duration": 1.0347011089324951,
        "processing": 1,
        "date_start": "2026-03-23T11:33:24+01:00",
        "date_finish": "2026-03-23T11:33:25+01:00",
        "operating_reset_at": 1774255404,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root object of the response ||
|| **productPropertyFeatures**
[`catalog_product_property_features[]`](../data-types.md#catalog_product_property_features) | An array of objects containing information about the selected parameters ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page. This field is returned if there are more records ||
|| **total**
[`integer`](../../data-types.md) | The total number of records. This field is not returned if the request is executed with `start = -1` ||
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

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access Denied | Insufficient rights to view the catalog ||
|| `100` | Invalid value {wrong_type} to match with parameter {filter}. Should be value of type array. | Invalid data type for the `filter` parameter value ||
|| `100` | Invalid value {wrong_type} to match with parameter {select}. Should be value of type array. | Invalid data type for the `select` parameter value ||
|| `100` | Invalid value {wrong_type} to match with parameter {order}. Should be value of type array. | Invalid data type for the `order` parameter value ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-feature-add.md)
- [{#T}](./catalog-product-property-feature-update.md)
- [{#T}](./catalog-product-property-feature-get.md)
- [{#T}](./catalog-product-property-feature-get-available-features-by-property.md)
- [{#T}](./catalog-product-property-feature-get-fields.md)