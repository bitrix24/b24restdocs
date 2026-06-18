# Get the List of Services ai.engine.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`ai_admin`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `ai.engine.list` returns a list of registered AI services.

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`array`](../data-types.md) | An array in the following format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — the name of the field to filter by
- `value_n` — the filter value

You can add a prefix to the keys `field_n` to specify the filter operation.
Possible prefix values:
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
- `%=` — LIKE, similar to `=%`
- `=` — equals, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The list of fields available for filtering is provided in the section [(detailed description)](#filter) ||
|| **limit**
[`integer`](../data-types.md) | Maximum number of elements in the response ||
|#

### Filter Parameter {#filter}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Service identifier ||
|| **APP_CODE**
[`string`](../data-types.md) | Application code to which the service belongs.

Without application context, such as through a webhook, you can filter by any `APP_CODE`.

In the context of an OAuth application, the method always shows only the services of the current application ||
|| **NAME**
[`string`](../data-types.md) | Service name ||
|| **CODE**
[`string`](../data-types.md) | Symbolic code of the service ||
|| **CATEGORY**
[`string`](../data-types.md) | Service category ||
|| **COMPLETIONS_URL**
[`string`](../data-types.md) | Service endpoint URL ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Date and time of service creation in ISO 8601 format, e.g., 2026-03-20T09:50:00+03:00 ||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "filter": {
          "=CATEGORY": "text"
        },
        "limit": 2
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_webhook_id**/**put_your_webhook_code**/ai.engine.list.json
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "filter": {
          "=CATEGORY": "text"
        },
        "limit": 2,
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/ai.engine.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each AiEngineItem returned in result[]
    type AiEngineItem = {
      id: number
      app_code: string | null
      name: string
      code: string
      category: string
      completions_url: string
      settings: Record<string, unknown>
      date_create: number
    }

    try {
      // ai.engine.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<AiEngineItem[]>({
        method: 'ai.engine.list',
        params: {
          filter: {
            '=CATEGORY': 'text',
          },
          limit: 2,
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('AI engines found:', result.length, result[0]?.name)
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
      async function fetchAiEngines() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // ai.engine.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'ai.engine.list',
            params: {
              filter: {
                '=CATEGORY': 'text',
              },
              limit: 2,
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('AI engines found:', result.length, result[0]?.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchAiEngines)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'ai.engine.list',
                [
                    'filter' => [
                        '=CATEGORY' => 'text',
                    ],
                    'limit' => 2,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting AI engine list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'ai.engine.list',
        {
            filter: {
                '=CATEGORY': 'text'
            },
            limit: 2
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'ai.engine.list',
        [
            'filter' => [
                '=CATEGORY' => 'text',
            ],
            'limit' => 2,
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
    "result": [
        {
            "id": 12,
            "app_code": "rest_app_123456",
            "name": "Acme GPT",
            "code": "acme_gpt",
            "category": "text",
            "completions_url": "https://api.example.com/bitrix24/ai/completions",
            "settings": {
                "code_alias": "ChatGPT",
                "model_context_type": "token",
                "model_context_limit": 15666
            },
            "date_create": 1774078200
        }
    ],
    "time": {
        "start": 1774078200,
        "finish": 1774078200.315271,
        "duration": 0.31527090072631836,
        "processing": 0.02,
        "date_start": "2026-03-20T09:50:00+03:00",
        "date_finish": "2026-03-20T09:50:00+03:00",
        "operating_reset_at": 1774078800,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | An array of registered services [(detailed description)](#engine-item) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Service Object {#engine-item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Service identifier ||
|| **app_code**
[`string`](../data-types.md) | Application code to which the service belongs.

May return `null` if the value is not set ||
|| **name**
[`string`](../data-types.md) | Service name ||
|| **code**
[`string`](../data-types.md) | Symbolic code of the service ||
|| **category**
[`string`](../data-types.md) | Service category ||
|| **completions_url**
[`string`](../data-types.md) | Service endpoint URL ||
|| **settings**
[`object`](../data-types.md) | Service settings saved during registration ||
|| **date_create**
[`integer`](../data-types.md) | Service creation date in Unix Timestamp format ||
|#

## Error Handling

{% include notitle [error handling](../../_includes/error-info.md) %}

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./ai-engine-register.md)
- [{#T}](./ai-engine-unregister.md)