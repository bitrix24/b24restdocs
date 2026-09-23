# AI in Bitrix24: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `ai.engine.*` methods connect your own AI service to Bitrix24. You can use them to register new services, retrieve their list, and remove selected ones.

{% note warning "" %}

Before registration, check the availability of the external handler URL in the `completions_url` parameter and the requirements for processing requests.

{% endnote %}

> Quick navigation: [all methods](#all-methods)

## What to Consider Before Registration

- Specify a valid URL in `completions_url` that returns an HTTP status of `200` upon verification
- Process requests asynchronously: acknowledge receipt with HTTP status `202`, then send the result through a callback
- Choose the `category` value based on the service scenario: `text`, `image`, `audio`, `call`, `vision`, or `classify`
- Pass additional parameters in `settings`

## How to Get Started

1. Prepare the external endpoint
2. Register the service via [ai.engine.register](./ai-engine-register.md)
3. Verify the registration and service parameters through [ai.engine.list](./ai-engine-list.md)
4. Remove the service via [ai.engine.unregister](./ai-engine-unregister.md) if it is no longer needed

## How Integration Works

**Endpoint.** Bitrix24 sends POST requests to the external endpoint via `completions_url`.

**Callback Mechanism.** After processing, the service sends the result to `callbackUrl` and error information to `errorCallbackUrl`.

Example request from Bitrix24 to `completions_url` for the `text` category:

```json
{
    "prompt": "Prepare a brief meeting summary",
    "payload_raw": "Prepare a brief meeting summary",
    "payload_provider": "text",
    "payload_role": "You are an assistant that prepares summaries",
    "context": [
        {
            "role": "user",
            "content": "We discussed the launch timeline and assignees"
        }
    ],
    "payload_markers": {
        "language": "en"
    },
    "payload_prompt_text": null,
    "auth": null,
    "category": "text",
    "ttl": 14400,
    "callbackUrl": "https://example.bitrix24.com/bitrix/services/main/ajax.php?action=ai.controller.integration.thirdparty.callbackSuccess&hash=example&rid=example",
    "errorCallbackUrl": "https://example.bitrix24.com/bitrix/services/main/ajax.php?action=ai.controller.integration.thirdparty.callbackError&hash=example&rid=example"
}
```

The endpoint must accept the POST request within five seconds and return HTTP status `202` with a JSON response:

```json
{
    "result": "OK"
}
```

After processing, send the result to `callbackUrl` in a POST request:

```json
{
    "result": "The launch is scheduled for September 15. The assignee is Klaus Weber."
}
```

If processing fails, send the error to `errorCallbackUrl`:

```json
{
    "message": "Provider temporarily unavailable",
    "code": 503,
    "api_request_completed": false
}
```

The `api_request_completed` parameter indicates whether the request to the AI provider was completed. If the value is `false`, Bitrix24 restores the deducted limit. The `ttl` parameter sets the planned job lifetime in seconds. After it expires, an error callback is not accepted; a successful result may still be accepted while the job record exists.

### Common Registration Errors

#|
|| **Code** | **Cause** | **How to Fix** ||
|| `ENGINE_REGISTER_ERROR_COMPLETIONS_URL_FAIL` | `completions_url` is unavailable, invalid, or returns a status other than `200` during verification | Check the URL and configure a `200` response to the verification GET request ||
|| `ENGINE_REGISTER_ERROR_CATEGORY_FORMAT` | `category` contains a value outside `text`, `image`, `audio`, `call`, `vision`, `classify` | Pass one of the supported categories ||
|#

The complete list of errors and registration parameters is provided in the [ai.engine.register](./ai-engine-register.md) method description.

{% note tip "" %}

You can use this template as a basis for your own service.

[Download Template](https://helpdesk.bitrix24.com/examples/endpoint.zip)

{% endnote %}

## Connection with the Application and Access

**Application.** The service is linked to the application via the `APP_CODE` field. This can be obtained in the response from the [ai.engine.list](./ai-engine-list.md) method. In the context of an OAuth application, the list contains only the services of the current application.

**Webhook.** You can filter services by any `APP_CODE` through the webhook.

## Overview of Methods {#all-methods}

> Scope: [`ai_admin`](../scopes/permissions.md)
>
> Who can execute the method: administrator

#| 
|| **Method** | **Description** ||
|| [ai.engine.register](./ai-engine-register.md) | Registers a custom AI service ||
|| [ai.engine.list](./ai-engine-list.md) | Retrieves a list of registered AI services ||
|| [ai.engine.unregister](./ai-engine-unregister.md) | Removes a registered AI service ||
|#
