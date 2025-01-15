# Get Available Languages for Translation catalog.priceTypeLang.getLanguages

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of available languages for translation.

No parameters.

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.priceTypeLang.getLanguages
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.priceTypeLang.getLanguages
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.priceTypeLang.getLanguages',
        {},
        function(result)
        {
            if (result.error())
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
        'catalog.priceTypeLang.getLanguages',
        []
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
        "languages": [
            {
                "active": "Y",
                "lid": "ar",
                "name": "ar"
            },
            {
                "active": "Y",
                "lid": "br",
                "name": "br"
            },
            {
                "active": "Y",
                "lid": "en",
                "name": "English"
            },
            {
                "active": "Y",
                "lid": "fr",
                "name": "fr"
            },
            {
                "active": "Y",
                "lid": "id",
                "name": "id"
            },
            {
                "active": "Y",
                "lid": "it",
                "name": "it"
            },
            {
                "active": "Y",
                "lid": "ja",
                "name": "ja"
            },
            {
                "active": "Y",
                "lid": "kz",
                "name": "kz"
            },
            {
                "active": "Y",
                "lid": "la",
                "name": "la"
            },
            {
                "active": "Y",
                "lid": "ms",
                "name": "ms"
            },
            {
                "active": "Y",
                "lid": "pl",
                "name": "pl"
            },
            {
                "active": "Y",
                "lid": "de",
                "name": "German"
            },
            {
                "active": "Y",
                "lid": "sc",
                "name": "sc"
            },
            {
                "active": "Y",
                "lid": "tc",
                "name": "tc"
            },
            {
                "active": "Y",
                "lid": "th",
                "name": "th"
            },
            {
                "active": "Y",
                "lid": "tr",
                "name": "tr"
            },
            {
                "active": "Y",
                "lid": "vn",
                "name": "vn"
            }
        ]
    },
    "total": 17,
    "time": {
        "start": 1733844860.95129,
        "finish": 1733844861.27092,
        "duration": 0.319633007049561,
        "processing": 0.0122489929199219,
        "date_start": "2024-12-10T17:34:20+02:00",
        "date_finish": "2024-12-10T17:34:21+02:00",
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
|| **languages**
[`catalog_language[]`](../../data-types.md#catalog_language) | Array of objects with information about available languages for translation ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 200040300010,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-lang-add.md)
- [{#T}](./catalog-price-type-lang-update.md)
- [{#T}](./catalog-price-type-lang-get.md)
- [{#T}](./catalog-price-type-lang-list.md)
- [{#T}](./catalog-price-type-lang-delete.md)
- [{#T}](./catalog-price-type-lang-get-fields.md)