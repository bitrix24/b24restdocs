# Get a List of Section Settings for catalog.productPropertySection.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" access permission

The method `catalog.productPropertySection.list` returns a list of section settings for product properties and variations based on a filter.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **select** 
[`array`](../../data-types.md) | An array containing the list of fields to be selected.

Available fields:

- `displayExpanded` — whether the property is displayed expanded in the filter
- `displayType` — the type of property in the smart filter
- `filterHint` — hint in the smart filter for visitors
- `propertyId` — the identifier of the product or variation property
- `smartFilter` — whether the property is displayed in the smart filter

By default, all available fields are returned. ||
|| **filter** 
[`object`](../../data-types.md) | An object for filtering the selected settings in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields available in the `select` parameter.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
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
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

If `propertyId` is not provided, the method selects section settings for all properties in the trade catalogs. If a `propertyId` is provided that does not exist or does not belong to the trade catalog, the method will return an empty list. ||
|| **order** 
[`object`](../../data-types.md) | An object for sorting the selected settings in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields available in the `select` parameter.

Possible values for `order`:
- `ASC` — in ascending order
- `DESC` — in descending order

If the parameter is not provided, sorting `propertyId ASC` is applied. ||
|| **start** 
[`integer`](../../data-types.md) | The parameter is used to control pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — `100`, and so on.

The formula for calculating the `start` parameter value: `start = (N - 1) * 50`, where `N` is the desired page number.

If the value `-1` is passed, the response will not include the `total` field. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"select":["propertyId","smartFilter","displayType","displayExpanded","filterHint"],"filter":{"propertyId":901},"order":{"propertyId":"ASC"}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productPropertySection.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"select":["propertyId","smartFilter","displayType","displayExpanded","filterHint"],"filter":{"propertyId":901},"order":{"propertyId":"ASC"},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productPropertySection.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory load.

    try {
    const response = await $b24.callListMethod(
        'catalog.productPropertySection.list',
        {
        select: ['propertyId', 'smartFilter', 'displayType', 'displayExpanded', 'filterHint'],
        filter: { propertyId: 901 },
        order: { propertyId: 'ASC' }
        },
        (progress: number) => { console.log('Progress:', progress) }
    );
    const items = response.getData() || [];
    for (const entity of items) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // fetchListMethod: Retrieves data in parts using an iterator. Use for large volumes of data for efficient memory consumption.

    try {
    const generator = $b24.fetchListMethod('catalog.productPropertySection.list', {
        select: ['propertyId', 'smartFilter', 'displayType', 'displayExpanded', 'filterHint'],
        filter: { propertyId: 901 },
        order: { propertyId: 'ASC' }
    }, 'PROPERTY_ID');
    for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
    }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.

    try {
    const response = await $b24.callMethod('catalog.productPropertySection.list', {
        select: ['propertyId', 'smartFilter', 'displayType', 'displayExpanded', 'filterHint'],
        filter: { propertyId: 901 },
        order: { propertyId: 'ASC' }
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
                'catalog.productPropertySection.list',
                [
                    'select' => ['propertyId', 'smartFilter', 'displayType', 'displayExpanded', 'filterHint'],
                    'filter' => [
                        'propertyId' => 901,
                    ],
                    'order' => ['propertyId' => 'ASC'],
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
        'catalog.productPropertySection.list',
        {
            select: ['propertyId', 'smartFilter', 'displayType', 'displayExpanded', 'filterHint'],
            filter: {
                propertyId: 901
            },
            order: { propertyId: 'ASC' }
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
        'catalog.productPropertySection.list',
        [
            'select' => ['propertyId', 'smartFilter', 'displayType', 'displayExpanded', 'filterHint'],
            'filter' => ['propertyId' => 901],
            'order' => ['propertyId' => 'ASC'],
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
        "productPropertySections": [
        {
            "displayExpanded": "N",
            "displayType": "F",
            "filterHint": "Filter hint",
            "propertyId": 901,
            "smartFilter": "Y"
        }
        ]
    },
    "total": 1,
    "time": {
        "start": 1774266816,
        "finish": 1774266816.883621,
        "duration": 0.8836209774017334,
        "processing": 0,
        "date_start": "2026-03-23T14:53:36+01:00",
        "date_finish": "2026-03-23T14:53:36+01:00",
        "operating_reset_at": 1774267416,
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
|| **productPropertySections** 
[`catalog_product_property_section[]`](../data-types.md#catalog_product_property_section) | An array of objects for section property settings ||
|| **next** 
[`integer`](../../data-types.md) | Offset for the next page. The field is returned if there are more records ||
|| **total** 
[`integer`](../../data-types.md) | Total number of records. The field is not returned if the request is executed with `start = -1` ||
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
|| `0` | Access Denied | Insufficient rights to view the catalog ||
|| `100` | Invalid value {wrong_type} to match with parameter {filter}. Should be value of type array. | Invalid data type for the `filter` parameter value ||
|| `100` | Invalid value {wrong_type} to match with parameter {select}. Should be value of type array. | Invalid data type for the `select` parameter value ||
|| `100` | Invalid value {wrong_type} to match with parameter {order}. Should be value of type array. | Invalid data type for the `order` parameter value ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-section-set.md)
- [{#T}](./catalog-product-property-section-get.md)