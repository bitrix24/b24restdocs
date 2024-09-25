# Add Trade Catalog catalog.catalog.add

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a trade catalog.

The method is used only in the on-premise version.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a trade catalog ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`integer`](../../data-types.md) | Identifier of the information block.

Existing identifiers of information blocks cannot be retrieved using REST.
||
|| **subscription**
[`string`](../../data-types.md) | Is content being sold? Possible values:
- `Y` — yes
- `N` — no

Defaults to `N`.

This parameter is used only in the on-premise version.
||
|| **vatId**
[`catalog_vat.id`](../data-types.md#catalog_vat) | VAT identifier.

To retrieve existing VAT identifiers, use [catalog.vat.list](../vat/catalog-vat-list.md).
||
|| **productIblockId**
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the parent information block of the trade catalog. Filled only for the trade catalog of trade offers.

To retrieve existing identifiers of information blocks, use [catalog.catalog.list](./catalog-catalog-list.md).
||
|| **skuPropertyId**
[`catalog_product_property.id`](../data-types.md#catalog_product_property) | Identifier of the property that stores the identifier of the parent product. Filled only for the trade catalog of trade offers.

To retrieve existing property identifiers, use [catalog.productProperty.list](../product-property/catalog-product-property-list.md).
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":24,"vatId":0,"productIblockId":23,"skuPropertyId":97}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.catalog.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":24,"vatId":0,"productIblockId":23,"skuPropertyId":97},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.catalog.add
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.catalog.add',
        {
            fields: {
                iblockId: 24,
                vatId: 0,
                productIblockId: 23,
                skuPropertyId: 97,
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
        'catalog.catalog.add',
        [
            'fields' => [
                'iblockId' => 24,
                'vatId' => 0,
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
            "name": "CRM Trade Catalog (offers)",
            "productIblockId": 23,
            "skuPropertyId": 97,
            "subscription": "N",
            "vatId": null
        }
    },
    "time": {
        "start": 1716362957.846316,
        "finish": 1716363143.374618,
        "duration": 185.5283019542694,
        "processing": 185.12314414978027,
        "date_start": "2024-05-22T10:29:17+02:00",
        "date_finish": "2024-05-22T10:32:23+02:00"
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
[`catalog_catalog`](../data-types.md#catalog_catalog) | Object with information about the added trade catalog ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200040300020,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ERROR_CORE` | Information block not specified
|| 
|| `ERROR_CORE` | Invalid information block identifier
|| 
|| `ERROR_CORE` | The specified information block does not exist
|| 
|| `ERROR_CORE` | Invalid product information block identifier
|| 
|| `ERROR_CORE` | The specified product information block does not exist
|| 
|| `ERROR_CORE` | Cannot make the trade catalog an information block of trade offers for itself
|| 
|| `ERROR_CORE` | Invalid binding property identifier for the product information block
|| 
|| `ERROR_CORE` | Product information block identifier specified, but binding property identifier for the product information block not specified
|| 
|| `ERROR_CORE` | Binding property identifier for the product information block specified, but product information block identifier not specified
|| 
|| `200040300010` | Insufficient permissions to add the trade catalog
|| 
|| `200040300020` | Insufficient permissions to add the trade catalog
|| 
|| `200040300030` | Insufficient permissions to add the trade catalog
|| 
|| `100` | Parameter `fields` not specified or empty
|| 
|| `0` | Required fields not provided
|| 
|| `0` | Trade catalog with the specified information block already exists
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-catalog-update.md)
- [{#T}](./catalog-catalog-get.md)
- [{#T}](./catalog-catalog-list.md)
- [{#T}](./catalog-catalog-is-offers.md)
- [{#T}](./catalog-catalog-delete.md)
- [{#T}](./catalog-catalog-get-fields.md)