# Update the Translation of Price Type Name catalog.priceTypeLang.update

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the translation of the price type name based on its identifier.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **Id***
[`catalog_price_type_lang.id`](../../data-types.md#catalog_price_type_lang) | Identifier of the price type name translation ||
|| **fields***
[`object`](../../../data-types.md) | Field values for updating the price type name translation ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **catalogGroupId**
[`catalog_price_type.id`](../../data-types.md#catalog_price_type) | Identifier of the price type.

You can obtain the identifiers of price types using the method [catalog.priceType.list](../catalog-price-type-list.md)
||
|| **name**
[`string`](../../../data-types.md) | Translation of the price type name ||
|| **lang**
[`catalog_language.lid`](../../data-types.md#catalog_language) | Language identifier.

You can obtain language identifiers using the method [catalog.priceTypeLang.getLanguages](./catalog-price-type-lang-get-languages.md)
||
|#

At least one field must be specified in the `fields` parameter.

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6,"fields":{"name":"Base Price"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.priceTypeLang.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6,"fields":{"name":"Base Price"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.priceTypeLang.update
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.priceTypeLang.update', 
        {
            id: 6,
            fields: {
                name: "Base Price",
            }
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
        'catalog.priceTypeLang.update',
        [
            'id' => 6,
            'fields' => [
                'name' => 'Base Price'
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
        "priceTypeLang": {
            "catalogGroupId": 1,
            "id": 6,
            "lang": "de",
            "name": "Base Price"
        }
    },
    "time": {
        "start": 1733843735.3651,
        "finish": 1733843735.75101,
        "duration": 0.385910987854004,
        "processing": 0.0111708641052246,
        "date_start": "2024-12-10T17:15:35+02:00",
        "date_finish": "2024-12-10T17:15:35+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **priceTypeLang**
[`catalog_price_type_lang`](../../data-types.md#catalog_price_type_lang) | Object containing information about the updated price type name translation ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description":"Required fields: name"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to edit
|| 
|| `201200000000` | No translation of the price type name with such an identifier exists
|| 
|| `201200000010` | No language with the specified identifier exists
|| 
|| `201000000000` | No price type with the specified identifier exists
|| 
|| `400` | Translation for the specified pair: price type and language identifier already exists
|| 
|| `100` | Parameter `id` not specified
|| 
|| `100` | Parameter `fields` not specified or empty
|| 
|| `0` | Required fields in the `fields` structure not provided
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-lang-add.md)
- [{#T}](./catalog-price-type-lang-get.md)
- [{#T}](./catalog-price-type-lang-list.md)
- [{#T}](./catalog-price-type-lang-delete.md)
- [{#T}](./catalog-price-type-lang-get-languages.md)
- [{#T}](./catalog-price-type-lang-get-fields.md)