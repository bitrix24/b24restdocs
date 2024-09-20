# Register the service ai.engine.register

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent
- links to pages that have not yet been created are not specified.

{% endnote %}

{% endif %}

> Scope: [`ai_admin`](../scopes/permissions.md)
>
> Who can execute the method: administrator

REST method for adding a custom service. This method registers an **engine** and updates it upon subsequent calls. This is not quite an embedding location, as the **endpoint** of the partner must adhere to strict formats.

#|
|| **Parameter** | **Description** | **Version** ||
|| **name**
[`unknown`](../data-types.md) | A meaningful and concise name that will appear in the user interface. | | ||
|| **code**
[`unknown`](../data-types.md) | Unique engine code | | ||
|| **category**
[`unknown`](../data-types.md) | Can be either text (text generation), image (image generation), or audio (text recognition). | | ||
|| **completions_url**
[`unknown`](../data-types.md) | endpoint for processing user requests. | | ||
|| **settings**
[`unknown`](../data-types.md) | Type of AI (see description below). Optional. | 23.800 | ||
|#

The method will return the ID of the added **engine** upon success.

## Type of AI

Array of parameters:

#|
|| **Parameter** | **Description** | **Version** ||
|| **code_alias**
[`unknown`](../data-types.md) | Type of AI. Available value: ChatGPT (Open AI). | | ||
|| **model_context_type**
[`unknown`](../data-types.md) | Type of context counting. Available values: token - tokens, symbol - symbols. Default is token. | | ||
|| **model_context_limit**
[`unknown`](../data-types.md) | Volume of context (default is 16K). Before sending your user request, the context limit is checked according to the counting type. | | ||
|#

## Examples

```javascript
BX24.callMethod(
    'ai.engine.register',
    {
        name: 'Smith GPT',
        code: 'smith_gpt',
        category: 'text',
        completions_url: 'https://antonds.com/ai/aul/completions/',
        settings: {
            code_alias: 'ChatGPT',
            model_context_type: 'token',
            model_context_limit: 16*1024,
        },
    },
    function(result)
    {
        if(result.error())
        {
            console.error(result.error());
        }
        else
        {
            console.info(result.data());
        }
    }
);
```

## Endpoint

{% note info "Attention!" %}

In the script, everything is in a single code flow; this is for example purposes. In **production** mode, it is necessary to separate the lines of code into a distinct section.

{% endnote %}

[Template](https://dev.bitrix.com/docs/chm_files/endpoint.rar) for creating a custom endpoint can be used to customize your own service.

## Important points:

1. The script must accept the request, process it quickly, and add it to its internal queue.
2. It should be able to return various response statuses (as shown in the example):
  - 200 — standard link transition;
  - 202 — if you accepted the request and added it to the queue;
  - 503 — if the service is unavailable.

A response is expected within a certain timeframe; after that, the callback becomes invalid.

{% note info "Attention!" %}

In addition to the response code, in case of successful generation, the handler must return `json_encode(['result' => 'OK'])`.

{% endnote %}

When working with the provider category **audio**, in the `prompt` key, you receive an array that includes the following elements:

- **file**: Link to the file. It is important to note that the file may have no extension.
- **fields**: An auxiliary internal array that contains:
  - **type**: Content-type of the file, which is especially important if the file has no extension (e.g., "audio/ogg").
  - **prompt**: An auxiliary prompt for the audio file, which may contain key information to assist in recognizing the file, such as your company name.

The provider also receives additional fields in the response:

#|
|| **Fields** | **Description** | **Version** ||
|| **auth** | Authorization data, | 23.600.0 ||
|| **payload_raw** | Raw value of the prompt (when using Copilot, there will be a character code of the used prompt) | 23.600.0 ||
|| **payload_provider** | Character code of the provider pre-prompt (when using Copilot, there will be prompt). | 23.600.0 ||
|| **payload_prompt_text** | If `payload_provider = prompt`, it will contain the raw instruction of the pre-prompt. This is the unprocessed pre-prompt for independent analysis. More details in the documentation on [prompts](.). | 23.800.0 ||
|| **payload_markers** | Array of additional markers from the user (`original_message`, `user_message`, `language`), used in forming the prompt. More details in the documentation on [prompts](.). | 23.800.0 ||
|| **payload_role** | Role (instruction) used in forming the prompt. In GPT-like systems, you should send this role as a system role in the message array. | 23.800.0 ||
|| **context** | Array of preceding messages in chronological order. For example, a list of comments on a post. The first in such a context list is the author's message (the post itself). Important: The volume of context sent to your provider depends on the volume specified by you and the counting type (more details in the provider documentation). By default, the counting method is "tokens," with a volume of 16K. You should send context to the neural network only if the parameter collect_context is set to true (1). In other cases, it is sent as additional information at your discretion. | 23.800.0 ||
|| **max_tokens** | Maximum number of tokens. This parameter controls the length of the output. Optional. | | ||
|| **temperature**^*^ | Temperature. This parameter controls the randomness of the output (lower values make the output more focused and deterministic). Required. | | ||
|#

\* - Required parameters 

**Example**

Suppose you receive (in addition to other information) three arrays of data.

- prompt - contains the current request, which is just text;
- payload_role - some text containing instructions;
- context - an array (let's say, also not empty).

In this case, the resulting array we obtain is:

```json
[
    [
        "role": "system",
        "content": "$payload_role"
    ],
    [
        // the entire context array, or part of it if you want to save the request
        // but remember that it goes in chronological order (the most recent messages are at the bottom)
    ],
    [
        "role": "user",
        "content": "$prompt" // this is the current request, and it is NOT included in the context
    ]
]
```

{% include [Footnote on examples](../../_includes/examples.md) %}