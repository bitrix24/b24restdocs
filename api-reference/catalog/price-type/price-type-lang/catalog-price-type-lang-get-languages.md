# Get Available Languages for Translation: catalog.priceTypeLang.getLanguages

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" or "Manage Price Types" access permission

The method `catalog.priceTypeLang.getLanguages` returns a list of languages available for translation.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type GetLanguagesResult = {
      languages: {
        active: string
        lid: string
        name: string
      }[]
    }

    try {
      const response = await $b24.actions.v2.call.make<GetLanguagesResult>({
        method: 'catalog.priceTypeLang.getLanguages',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.languages)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getLanguages() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.priceTypeLang.getLanguages',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.languages)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getLanguages)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.price_type_lang.get_languages().response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.priceTypeLang.getLanguages',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.priceTypeLang.getLanguages", nil)
    if err != nil {
    	return fmt.Errorf("catalog.priceTypeLang.getLanguages: %w", err)
    }

    // The method wraps the response in an object with the "languages" key.
    raw, ok := b24.Unwrap(res.Result, "languages")
    if !ok {
    	return fmt.Errorf("no languages key in the response")
    }

    var items []struct {
    	Active string `json:"active"`
    	Lid    string `json:"lid"`
    	Name   string `json:"name"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.Active)
    }
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
                "name": "اَلْعَرَبِيَّةُ"
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
                "active": "N",
                "lid": "hi",
                "name": "हिन्दी"
            },
            {
                "active": "Y",
                "lid": "id",
                "name": "Bahasa Indonesia"
            },
            {
                "active": "N",
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
                "lid": "kz",
                "name": "Қазақша"
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
                "lid": "sc",
                "name": "中文（简体）"
            },
            {
                "active": "Y",
                "lid": "tc",
                "name": "中文（繁體）"
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
                "lid": "uz",
                "name": "Oʻzbek tili"
            },
            {
                "active": "Y",
                "lid": "vn",
                "name": "Tiếng Việt"
            }
        ]
    },
    "total": 21,
    "time": {
        "start": 1793700371,
        "finish": 1793700371.783719,
        "duration": 0.7837190628051758,
        "processing": 0,
        "date_start": "2026-11-03T13:06:11+03:00",
        "date_finish": "2026-11-03T13:06:11+03:00",
        "operating_reset_at": 1793700960,
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
|| **result.languages**
[`catalog_language[]`](../../data-types.md#catalog_language) | Array of objects containing information about available languages for translation ||
|| **total**
[`integer`](../../../data-types.md) | Total number of languages ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "200040300010",
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user does not have the "View product catalog" or "Manage price types" access permission ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-lang-add.md)
- [{#T}](./catalog-price-type-lang-update.md)
- [{#T}](./catalog-price-type-lang-get.md)
- [{#T}](./catalog-price-type-lang-list.md)
- [{#T}](./catalog-price-type-lang-delete.md)
- [{#T}](./catalog-price-type-lang-get-fields.md)
