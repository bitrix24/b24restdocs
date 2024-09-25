# Update Fields of the Trade Catalog catalog.catalog.update

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the fields of the trade catalog.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the trade catalog ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the trade catalog ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **yandexExport**
[`string`](../../data-types.md) | Deprecated parameter. Whether to export to *Yandex.Market*. Possible values:
- `Y` — yes
- `N` — no
||
|| **subscription**
[`string`](../../data-types.md) | Whether content is being sold. Possible values:
- `Y` — yes
- `N` — no

This parameter is used only in the on-premise version
||
|| **vatId**
[`catalog_vat.id`](../data-types.md#catalog_vat) | VAT identifier.

To obtain existing VAT identifiers, use [catalog.vat.list](../vat/catalog-vat-list.md)
||
|| **productIblockId**
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the parent information block of the trade catalog. Filled only for the trade catalog of trade offers.

To obtain existing information block identifiers, use [catalog.catalog.list](./catalog-catalog-list.md)
||
|| **skuPropertyId**
[`catalog_product_property.id`](../data-types.md#catalog_product_property) | Identifier of the property that stores the identifier of the parent product. Filled only for the trade catalog of trade offers.

To obtain existing property identifiers, use [catalog.productProperty.list](../product-property/catalog-product-property-list.md)
||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":24,"fields":{"vatId":2,"productIblockId":23,"skuPropertyId":97}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.catalog.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":24,"fields":{"vatId":2,"productIblockId":23,"skuPropertyId":97},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.catalog.update
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.catalog.update', 
        {
            id: 24,
            fields: {
                vatId: 2,
                productIblockId: 23,
                skuPropertyId: 97,
            },
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.catalog.update',
        [
            'id' => 24,
            'fields' => [
                'vatId' => 2,
                'productIblockId' => 23,
                'skuPropertyId' => 97,
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
        "catalog": {
            "iblockId": 24,
            "iblockTypeId": 0,
            "id": 24, 
            "lid": "s1",
            "name": "Trade Catalog CRM (offers)",
            "productIblockId": 23,
            "skuPropertyId": 97,
            "subscription": "N",
            "vatId": 2
        }
    },
    "time": {
        "start": 1716806984.460591,
        "finish": 1716806984.92558,
        "duration": 0.46498894691467285,
        "processing": 0.03661203384399414,
        "date_start": "2024-05-27T13:49:44+02:00",
        "date_finish": "2024-05-27T13:49:44+02:00"
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
[`catalog_catalog`](../data-types.md#catalog_catalog) | Object containing information about the updated trade catalog ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":0,
    "error_description":"Catalog does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ERROR_CORE` | The specified binding property does not exist or is inactive
|| 
|| `ERROR_CORE` | The identifier of the binding property to the product information block is specified, but the identifier of the product information block is not specified
|| 
|| `ERROR_CORE` | The identifier of the product information block is specified, but the identifier of the binding property to the product information block is not specified
|| 
|| `ERROR_CORE` | Invalid identifier of the binding property to the product information block
|| 
|| `ERROR_CORE` | Cannot make the trade catalog an information block of trade offers for itself
|| 
|| `ERROR_CORE` | The specified product information block does not exist
||
|| `ERROR_CORE` | Invalid identifier of the product information block
|| 
|| `ERROR_CORE` | The identifier of the trade catalog and the identifier of the binding property must be specified together
|| 
|| `ERROR_CORE` | Invalid identifier of the trade catalog
|| 
|| `200040300020` | Insufficient permissions to update the trade catalog
|| 
|| `200040300010` | Insufficient permissions to update the trade catalog
|| 
|| `200040300030` | Insufficient permissions to update the trade catalog
|| 
|| `100` | The `id` parameter is not specified
|| 
|| `100` | The `fields` parameter is not specified or is empty
|| 
|| `0` | The trade catalog with the specified identifier does not exist
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-catalog-add.md)
- [{#T}](./catalog-catalog-get.md)
- [{#T}](./catalog-catalog-list.md)
- [{#T}](./catalog-catalog-is-offers.md)
- [{#T}](./catalog-catalog-delete.md)
- [{#T}](./catalog-catalog-get-fields.md)