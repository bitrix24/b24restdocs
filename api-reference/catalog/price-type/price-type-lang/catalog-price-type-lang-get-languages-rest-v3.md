# Get Available Languages for Translation v 3.0 catalog.pricetype.lang.getlanguages

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "Manage price types" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `catalog.pricetype.lang.getlanguages` returns the list of languages into which a price type name can be translated.

## Method Parameters

No parameters.

The method always returns all Bitrix24 languages; the `filter` parameter is ignored.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.pricetype.lang.getlanguages`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.pricetype.lang.getlanguages
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.pricetype.lang.getlanguages
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PriceTypeLangGetLanguagesResult = {
      languages: Array<{
        ACTIVE: 'Y' | 'N'
        NAME: string
        LID: string
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<PriceTypeLangGetLanguagesResult>({
        method: 'catalog.pricetype.lang.getlanguages',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        const active = result.languages.filter(language => language.ACTIVE === 'Y')
        console.info('Active languages:', active.map(language => language.LID))
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getLanguages() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.pricetype.lang.getlanguages',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          const active = result.languages.filter(language => language.ACTIVE === 'Y')
          console.info('Active languages:', active.map(language => language.LID))
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
        bitrix_response = client.catalog.pricetype.lang.getlanguages().response
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.pricetype.lang.getlanguages',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.pricetype.lang.getlanguages',
        {},
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.pricetype.lang.getlanguages',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.pricetype.lang.getlanguages", b24.Params{}, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.pricetype.lang.getlanguages: %w", err)
    }

    var result struct {
    	Languages []struct {
    		Active string `json:"ACTIVE"`
    		Name   string `json:"NAME"`
    		LID    string `json:"LID"`
    	} `json:"languages"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    for _, language := range result.Languages {
    	fmt.Println(language.LID, language.Name, language.Active)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "languages": [
            {
                "ACTIVE": "Y",
                "NAME": "Deutsch",
                "LID": "de"
            },
            {
                "ACTIVE": "Y",
                "NAME": "English",
                "LID": "en"
            },
            {
                "ACTIVE": "Y",
                "NAME": "Français",
                "LID": "fr"
            },
            {
                "ACTIVE": "N",
                "NAME": "हिन्दी",
                "LID": "hi"
            },
            {
                "ACTIVE": "Y",
                "NAME": "Italiano",
                "LID": "it"
            },
            {
                "ACTIVE": "Y",
                "NAME": "Polski",
                "LID": "pl"
            }
        ]
    },
    "time": {
        "start": 1792668096,
        "finish": 1792668096.518782,
        "duration": 0.5187819004058838,
        "processing": 0,
        "date_start": "2026-10-22T14:21:36+02:00",
        "date_finish": "2026-10-22T14:21:36+02:00",
        "operating_reset_at": 1792668660,
        "operating": 0
    }
}
```

The example shows part of the list. The method returns all Bitrix24 languages sorted by language code.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the response data ||
|| **result.languages**
[`array`](../../../data-types.md) | Array of languages. [Element description](#languages) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Languages Element {#languages}

The element keys come in uppercase.

#|
|| **Name**
`type` | **Description** ||
|| **ACTIVE**
[`string`](../../../data-types.md) | Language activity: `Y` — active, `N` — inactive. A translation can also be added for an inactive language ||
|| **NAME**
[`string`](../../../data-types.md) | Language name ||
|| **LID**
[`string`](../../../data-types.md) | Language code. It is passed in the `lang` field of the [catalog.pricetype.lang.add](./catalog-price-type-lang-add-rest-v3.md) and [catalog.pricetype.lang.update](./catalog-price-type-lang-update-rest-v3.md) methods ||
|#

## Error Handling

HTTP status: **404**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION",
        "message": "Method \"catalog.pricetype.lang.getLanguages\" was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Method Name Error

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `catalog.pricetype.lang.getLanguages` was not found | Call the method in lowercase: `catalog.pricetype.lang.getlanguages` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View product catalog" or "Manage price types" access permission ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-lang-add-rest-v3.md)
- [{#T}](./catalog-price-type-lang-update-rest-v3.md)
- [{#T}](./catalog-price-type-lang-delete-rest-v3.md)
