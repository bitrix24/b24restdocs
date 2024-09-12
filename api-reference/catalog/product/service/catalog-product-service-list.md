# Get a List of Services catalog.product.service.list

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of services based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | 
An array of fields to be selected (see fields of the object [catalog_product_service](../../data-types.md#catalog_product_service)).

Required fields: `id`, `iblockId`
||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected services in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_service](../../data-types.md#catalog_product_service). 

Required fields: `iblockId`.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equals, exact match (used by default)
- `!=` — not equal
- `!` — not equal ||
|| **order**
[`object`](../../../data-types.md) | 
An object for sorting the selected services in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_service](../../data-types.md#catalog_product_service).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number
||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","available","bundle","code","createdBy","dateActiveFrom","dateActiveTo","dateCreate","detailPicture","detailText","detailTextType","iblockSectionId","modifiedBy","previewPicture","previewText","previewTextType","sort","timestampX","type","vatId","vatIncluded","xmlId","property94","property95"],"filter":{"iblockId":23,">id":10,"@vatId":[1,2]},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.service.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","available","bundle","code","createdBy","dateActiveFrom","dateActiveTo","dateCreate","detailPicture","detailText","detailTextType","iblockSectionId","modifiedBy","previewPicture","previewText","previewTextType","sort","timestampX","type","vatId","vatIncluded","xmlId","property94","property95"],"filter":{"iblockId":23,">id":10,"@vatId":[1,2]},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.service.list
    ```

- JS

    ```js
    BX24.callMethod(
        "catalog.product.service.list", {
            "select": [
                "id",
                "iblockId",
                "name",
                "active",
                "available",
                "bundle",
                "code",
                "createdBy",
                "dateActiveFrom",
                "dateActiveTo",
                "dateCreate",
                "detailPicture",
                "detailText",
                "detailTextType",
                "iblockSectionId",
                "modifiedBy",
                "previewPicture",
                "previewText",
                "previewTextType",
                "sort",
                "timestampX",
                "type",
                "vatId",
                "vatIncluded",
                "xmlId",
                "property94",
                "property95",
            ],
            "filter": {
                "iblockId": 23,
                ">id": 10,
                "@vatId": [1, 2],
            },
            "order": {
                "id": "desc",
            }
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
        'catalog.product.service.list',
        [
            'select' => [
                'id',
                'iblockId',
                'name',
                'active',
                'available',
                'bundle',
                'code',
                'createdBy',
                'dateActiveFrom',
                'dateActiveTo',
                'dateCreate',
                'detailPicture',
                'detailText',
                'detailTextType',
                'iblockSectionId',
                'modifiedBy',
                'previewPicture',
                'previewText',
                'previewTextType',
                'sort',
                'timestampX',
                'type',
                'vatId',
                'vatIncluded',
                'xmlId',
                'property94',
                'property95',
            ],
            'filter' => [
                'iblockId' => 23,
                '>id' => 10,
                '@vatId' => [1, 2],
            ],
            'order' => [
                'id' => 'desc',
            ]
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
        "services": [
            {
                "active": "Y",
                "available": "N",
                "bundle": "N",
                "code": "service",
                "createdBy": 1,
                "dateActiveFrom": "2024-05-28T10:00:00+03:00",
                "dateActiveTo": "2024-05-29T10:00:00+03:00",
                "dateCreate": "2024-05-27T10:00:00+03:00",
                "detailPicture": {
                    "id": "6497",
                    "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6497\u0026fields%5BproductId%5D=1265",
                    "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6497\u0026fields%5BproductId%5D=1265"
                },
                "detailText": null,
                "detailTextType": "text",
                "iblockId": 23,
                "iblockSectionId": 47,
                "id": 1265,
                "modifiedBy": 1,
                "name": "Service",
                "previewPicture": {
                    "id": "6496",
                    "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6496\u0026fields%5BproductId%5D=1265",
                    "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6496\u0026fields%5BproductId%5D=1265"
                },
                "previewText": null,
                "previewTextType": "text",
                "sort": 100,
                "timestampX": "2024-06-14T11:59:04+03:00",
                "type": 7,
                "vatId": 1,
                "vatIncluded": "Y",
                "xmlId": "1265"
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1718363637.281945,
        "finish": 1718363637.984081,
        "duration": 0.7021360397338867,
        "processing": 0.2966270446777344,
        "date_start": "2024-06-14T14:13:57+03:00",
        "date_finish": "2024-06-14T14:13:57+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | The root element of the response ||
|| **services**
[`catalog_product_service[]`](../../data-types.md#catalog_product_service) | An array of objects with information about the selected services ||
|| **total**
[`integer`](../../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read the commercial catalog
|| 
|| `0` | Fields `id`, `iblockId` not specified in the selection fields
|| 
|| `0` | Field `iblockId` not specified in the filter
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-service-add.md)
- [{#T}](./catalog-product-service-update.md)
- [{#T}](./catalog-product-service-get.md)
- [{#T}](./catalog-product-service-download.md)
- [{#T}](./catalog-product-service-delete.md)
- [{#T}](./catalog-product-service-get-fields-by-filter.md)