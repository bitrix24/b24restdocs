# Get the list of languages for localization sale.statusLang.getListLangs

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of possible languages for localizations.

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.statuslang.getlistlangs
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.statuslang.getlistlangs
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.statuslang.getlistlangs',
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.statuslang.getlistlangs',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting list of languages for sale status: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.statuslang.getlistlangs',
        {},
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.statuslang.getlistlangs',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "langs": [
            {
                "active": "Y",
                "lid": "ar",
                "name": "عربي"
            },
            {
                "active": "Y",
                "lid": "br",
                "name": "Português (Brasil)"
            },
            {
                "active": "Y",
                "lid": "de",
                "name": "Deutsch"
            },
            {
                "active": "Y",
                "lid": "en",
                "name": "English"
            },
            {
                "active": "Y",
                "lid": "fr",
                "name": "Français"
            },
            {
                "active": "Y",
                "lid": "hi",
                "name": "हिन्दी"
            },
            {
                "active": "Y",
                "lid": "id",
                "name": "Bahasa Indonesia"
            },
            {
                "active": "Y",
                "lid": "in",
                "name": "English (India)"
            },
            {
                "active": "Y",
                "lid": "it",
                "name": "Italiano"
            },
            {
                "active": "Y",
                "lid": "ja",
                "name": "日本語"
            },
            {
                "active": "Y",
                "lid": "la",
                "name": "Español"
            },
            {
                "active": "Y",
                "lid": "ms",
                "name": "Bahasa Melayu"
            },
            {
                "active": "Y",
                "lid": "pl",
                "name": "Polski"
            },
            {
                "active": "Y",
                "lid": "de",
                "name": "Русский"
            },
            {
                "active": "Y",
                "lid": "sc",
                "name": "中文（简体）"
            },
            {
                "active": "Y",
                "lid": "tc",
                "name": "中文（繁体）"
            },
            {
                "active": "Y",
                "lid": "th",
                "name": "ภาษาไทย"
            },
            {
                "active": "Y",
                "lid": "tr",
                "name": "Türkçe"
            },
            {
                "active": "Y",
                "lid": "ua",
                "name": "Українська"
            },
            {
                "active": "Y",
                "lid": "vn",
                "name": "Tiếng Việt"
            }
        ]
    },
    "total": 20,
    "time": {
        "start": 1712230480.81481,
        "finish": 1712230480.8460569,
        "duration": 0.03124690055847168,
        "processing": 0.0043120384216308594,
        "date_start": "2024-04-04T14:34:40+02:00",
        "date_finish": "2024-04-04T14:34:40+02:00",
        "operating_reset_at": 1712231080,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **langs**
[`sale_lang[]`](../data-types.md) | Array of objects with information about selected languages ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error": 0,
   "error_description": "error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read the list of possible languages for localizations ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-status-lang-add.md)
- [{#T}](./sale-status-lang-list.md)
- [{#T}](./sale-status-lang-delete-by-filter.md)
- [{#T}](./sale-status-lang-get-fields.md)