# Register the Service ai.engine.register

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`ai_admin`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `ai.engine.register` registers a custom AI service.

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../data-types.md) | The name of the service that will be displayed in the interface ||
|| **code***
[`string`](../data-types.md) | A unique code for the service.

Only characters `A-Za-z0-9-_` are allowed ||
|| **category***
[`string`](../data-types.md) | The category of the service.

Possible values:
- `text`
- `image`
- `audio`
- `call` ||
|| **completions_url***
[`string`](../data-types.md) | The URL endpoint of the handler, which must respond with HTTP status `200` during registration verification [(detailed description)](#endpoint) ||
|| **settings**
[`object`](../data-types.md) | Additional settings for the service [(detailed description)](#settings) ||
|#

### Settings Parameter {#settings}

The method accepts `settings` as a JSON object without a strict schema. The following fields are used in the service code:

#|
|| **Name**
`type` | **Description** ||
|| **code_alias**
[`string`](../data-types.md) | The alias of the model.

Defaults to `ChatGPT` ||
|| **model_context_type**
[`string`](../data-types.md) | The method of calculating context.

Possible values:
- `token`
- `symbol`

Defaults to `token` ||
|| **model_context_limit**
[`integer`](../data-types.md) | The context limit.

Defaults to `15666` ||
|#

## Endpoint {#endpoint}

`completions_url` must point to your endpoint that accepts requests from Bitrix24 and processes them in the expected format.

{% note info "Attention!" %}

The endpoint code from the examples can be used as a foundation, but for production, it's better to separate the processing into different parts of the application.

{% endnote %}

The [template](https://helpdesk.bitrix24.com/examples/endpoint.zip) endpoint can be used as a basis for your own service.

### Endpoint Requirements

1. The endpoint must quickly accept the request and return a response or queue the task in its internal queue. The initial response time should not exceed 5 seconds — after the timeout, the connection is terminated.
2. For the `image` category, processing should be done asynchronously.
3. The request payload includes `callbackUrl` and `errorCallbackUrl`. After processing, the result should be sent to `callbackUrl`, and error information to `errorCallbackUrl`.
4. The endpoint must correctly return HTTP statuses:

- `200` — request processed immediately
- `202` — request accepted and queued
- `503` — service temporarily unavailable

The `callbackUrl` has a limited lifespan — it is provided in the `ttl` parameter (in seconds). If the endpoint does not send the result before this period expires, the link will become invalid, and the user will not receive a response.

{% note info "Attention!" %}

The endpoint's response to the initial request does not replace the callback mechanism. Upon successful acceptance of the request, the endpoint should return `json_encode(['result' => 'OK'])`.

{% endnote %}

### Features for the Audio Category

For the `audio` category, the `prompt` key receives an object with the following fields:

#|
|| **Name**
`type` | **Description** ||
|| **file**
[`string`](../data-types.md) | Link to the file. The file may come without an extension ||
|| **fields**
[`object`](../data-types.md) | Additional data about the file [(detailed description)](#audio-fields) ||
|| **fileExtension**
[`string`](../data-types.md) | The file extension, if it can be determined. May come as an empty string ||
|#

#### Fields Object {#audio-fields}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../data-types.md) | Content-Type of the file. Especially important if the file is sent without an extension, e.g., `audio/ogg` ||
|| **prompt**
[`string`](../data-types.md) | Auxiliary prompt for file recognition, e.g., company name ||
|#

### Features for the Image Category

For the `image` category, the `prompt` key receives an object with the following fields:

#|
|| **Name**
`type` | **Description** ||
|| **prompt**
[`string`](../data-types.md) | Text description of what needs to be generated ||
|| **style**
[`string`](../data-types.md) | The style of image generation. May be absent if no style was specified ||
|| **format**
[`string`](../data-types.md) | Image format, e.g., `square`, `landscape`. May come as `null` if no format was specified ||
|| **images_number**
[`integer`](../data-types.md) | The number of images to generate. May be absent if no value was specified ||
|#

### Additional Fields in the Request to the Endpoint

#|
|| **Name**
`type` | **Description** ||
|| **auth**
[`object`](../data-types.md) | Authorization data ||
|| **payload_raw**
[`string`](../data-types.md) | Raw value of the prompt. When using BitrixGPT, this may contain the symbolic code of the used prompt ||
|| **payload_provider**
[`string`](../data-types.md) | Symbolic code of the pre-prompt provider. When using BitrixGPT, this may contain `prompt` ||
|| **payload_prompt_text**
[`string`](../data-types.md) | If `payload_provider = prompt`, contains the raw instruction of the pre-prompt ||
|| **payload_markers**
[`object`](../data-types.md) | Additional user markers used in prompt formation ||
|| **payload_role**
[`string`](../data-types.md) | Role or instruction used in prompt formation. In GPT-like systems, this value is usually passed as a system message ||
|| **collect_context**
[`boolean`](../data-types.md) | A flag indicating whether to pass context to the model ||
|| **context**
[`array`](../data-types.md) | An array of previous messages in chronological order. The volume sent depends on the provider's settings and the type of context calculation ||
|| **max_tokens**
[`integer`](../data-types.md) | Maximum number of tokens in the response ||
|| **temperature**
[`number`](../data-types.md) | A parameter that controls the degree of randomness of the output ||
|| **callbackUrl**
[`string`](../data-types.md) | URL to which the result of successful processing should be sent ||
|| **errorCallbackUrl**
[`string`](../data-types.md) | URL to which error information should be sent ||
|| **ttl**
[`integer`](../data-types.md) | The lifespan of the `callbackUrl` link in seconds. After this period, the link will become invalid ||
|#

Context should only be passed to the model if the request includes `collect_context = true`. If the parameter is absent or set to `false`, context can be omitted.

Example structure of a message for a GPT-like model:

```json
[
    {
        "role": "system",
        "content": "$payload_role"
    },
    {
        "role": "user",
        "content": "$prompt"
    }
]
```

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "name": "Acme GPT",
        "code": "acme_gpt",
        "category": "text",
        "completions_url": "https://api.example.com/bitrix24/ai/completions",
        "settings": {
          "code_alias": "ChatGPT",
          "model_context_type": "token",
          "model_context_limit": 15666
        }
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_webhook_id**/**put_your_webhook_code**/ai.engine.register.json
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "name": "Acme GPT",
        "code": "acme_gpt",
        "category": "text",
        "completions_url": "https://api.example.com/bitrix24/ai/completions",
        "settings": {
          "code_alias": "ChatGPT",
          "model_context_type": "token",
          "model_context_limit": 15666
        },
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/ai.engine.register
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'ai.engine.register',
            {
                name: 'Acme GPT',
                code: 'acme_gpt',
                category: 'text',
                completions_url: 'https://api.example.com/bitrix24/ai/completions',
                settings: {
                    code_alias: 'ChatGPT',
                    model_context_type: 'token',
                    model_context_limit: 15666
                }
            }
        );

        const result = response.getData().result;
        console.log('Engine registered:', result);
    }
    catch (error)
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'ai.engine.register',
                [
                    'name' => 'Acme GPT',
                    'code' => 'acme_gpt',
                    'category' => 'text',
                    'completions_url' => 'https://api.example.com/bitrix24/ai/completions',
                    'settings' => [
                        'code_alias' => 'ChatGPT',
                        'model_context_type' => 'token',
                        'model_context_limit' => 15666,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error registering AI engine: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'ai.engine.register',
        {
            name: 'Acme GPT',
            code: 'acme_gpt',
            category: 'text',
            completions_url: 'https://api.example.com/bitrix24/ai/completions',
            settings: {
                code_alias: 'ChatGPT',
                model_context_type: 'token',
                model_context_limit: 15666
            }
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
        'ai.engine.register',
        [
            'name' => 'Acme GPT',
            'code' => 'acme_gpt',
            'category' => 'text',
            'completions_url' => 'https://api.example.com/bitrix24/ai/completions',
            'settings' => [
                'code_alias' => 'ChatGPT',
                'model_context_type' => 'token',
                'model_context_limit' => 15666,
            ],
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
    "result": 12,
    "time": {
        "start": 1774078200,
        "finish": 1774078200.315271,
        "duration": 0.31527090072631836,
        "processing": 0.02,
        "date_start": "2026-03-20T09:50:00+02:00",
        "date_finish": "2026-03-20T09:50:00+02:00",
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
[`integer`](../data-types.md) | Identifier of the registered service ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ENGINE_REGISTER_ERROR_CODE_UNIQUE",
    "error_description": "A record with this `code` already exists."
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ENGINE_REGISTER_ERROR_NAME` | The `name` key with a string value is required | The `name` parameter is missing, an empty value is provided, or the value is not a string ||
|| `ENGINE_REGISTER_ERROR_CODE` | The `code` key with a string value is required | The `code` parameter is missing, an empty value is provided, or the value is not a string ||
|| `ENGINE_REGISTER_ERROR_CODE_FORMAT` | The `code` key must contain only characters `A-Za-z0-9-_` | Invalid characters are present in `code` ||
|| `ENGINE_REGISTER_ERROR_CODE_UNIQUE` | A record with this `code` already exists | A service with this code is already registered in the same category ||
|| `ENGINE_REGISTER_ERROR_CATEGORY` | The `category` key is required | The `category` parameter is missing or an empty value is provided ||
|| `ENGINE_REGISTER_ERROR_CATEGORY_FORMAT` | The `category` key can contain one of the values: `text, image, audio, call` | The provided `category` value is not in the list of available categories ||
|| `ENGINE_REGISTER_ERROR_COMPLETIONS_URL` | The `completions_url` key with a string value is required | The `completions_url` parameter is missing, an empty value is provided, or the value is not a string ||
|| `ENGINE_REGISTER_ERROR_COMPLETIONS_URL_FAIL` | The value of the `completions_url` key must be a valid URL that returns status `200` upon verification | The URL is unavailable, invalid, or returns a status other than `200` upon verification ||
|| `ENGINE_REGISTER_ERROR_SETTINGS_FORMAT` | The value of the `settings` key must be valid JSON | The `settings` parameter is not provided as an object ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./ai-engine-list.md)
- [{#T}](./ai-engine-unregister.md)