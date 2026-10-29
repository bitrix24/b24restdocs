# How to Use Examples in the Documentation

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

On method pages in the [API Reference](../api-reference/index.md), examples are grouped in the "Code Examples" block. The tabs show a method call via cURL and via the official Bitrix24 SDKs — libraries for different programming languages. Most often, the code in a tab is a call fragment rather than a complete project. Such a fragment does not include library installation, connection to Bitrix24, or real parameter values.

To make an example work, choose a tab for your environment, replace placeholders like `**put_your_webhook_here**` with your own values, and create a connection object for the SDK.

The [{#T}](../settings/how-to-call-rest-api/general-principles.md) article explains how an HTTP request to the REST API is structured.

## Which Tab to Choose

#|
|| **Tab** | **Tool** | **Where code is executed** | **Authorization** ||
|| `cURL (Webhook)` | Without libraries | Any environment with curl: terminal, script, request testing service | [inbound webhook](../local-integrations/local-webhooks.md) ||
|| `cURL (OAuth)` | Without libraries | Any environment with curl: terminal, script, request testing service | [OAuth 2.0](../settings/oauth/index.md) ||
|| `JS (TS)` | [B24JsSDK](../sdk/b24jssdk/index.md) | Project with a bundler, code in TypeScript. Examples are designed for an application in a Bitrix24 frame | In examples — [OAuth 2.0, simplified method for frames](../settings/oauth/simple-way.md) via `B24Frame`. In server-side code — [inbound webhook](../local-integrations/local-webhooks.md) or [OAuth 2.0](../settings/oauth/index.md), depending on the connection class ||
|| `JS (UMD)` | [B24JsSDK](../sdk/b24jssdk/index.md), UMD build | HTML page without a bundler | [OAuth 2.0, simplified method for frames](../settings/oauth/simple-way.md): in examples, `B24Frame` is created ||
|| `BX24.js` | [BX24.js](../sdk/bx24-js-sdk/index.md) | Only an application opened in a frame within the Bitrix24 interface | [OAuth 2.0, simplified method for frames](../settings/oauth/simple-way.md): the library provides the data automatically ||
|| `PHP` | [B24PhpSDK](../sdk/b24phpsdk/index.md) | Server-side PHP, most often a method call by name | [inbound webhook](../local-integrations/local-webhooks.md) or [OAuth 2.0](../settings/oauth/index.md) ||
|| `PHP CRest` | [CRest PHP SDK](../sdk/crest-php-sdk/index.md) | Server-side PHP, calls via a single `CRest::call` method | [inbound webhook](../local-integrations/local-webhooks.md) or [OAuth 2.0](../settings/oauth/index.md) ||
|| `Python` | [B24PySDK](../sdk/b24pysdk/index.md) | Server-side Python | [inbound webhook](../local-integrations/local-webhooks.md) or [OAuth 2.0](../settings/oauth/index.md) ||
|| `Go` | [B24GoSDK](../sdk/b24gosdk/index.md) | Server-side Go, a method call by name | [inbound webhook](../local-integrations/local-webhooks.md) or [OAuth 2.0](../settings/oauth/index.md) ||
|#

The set of tabs varies across pages: each page contains only the examples prepared for that specific method. If there is no B24PhpSDK example, use CRest or construct the call based on the method parameter descriptions.

Tab labels are not formatted identically everywhere: a tab with a B24JsSDK example might be labeled `JS`.

Check the label against the code: you can recognize a tab by the object used for the call. The objects of each SDK are listed in the table in the [What to Add to Make an Example Work](#what-to-add) section.

`cURL` tabs are suitable when you need to test a method, view a raw response, or call the API where you cannot connect an SDK, such as from a terminal or a third-party service. For a production integration, use an SDK: it handles authorization and parses the response.

Additional capabilities differ between SDKs: for example, B24JsSDK refreshes tokens and respects request limits on its own. B24GoSDK retries a request by default when the limit is exceeded and refreshes tokens once the client is configured. B24PhpSDK, BX24.js, and CRest do not support [REST 3.0](../api-reference/rest-v3.md#sdk) methods. If multiple SDKs fit, compare them in the [SDK Overview](../sdk/index.md).

## What to Substitute for Placeholders

Instead of real values, examples use placeholders in double asterisks, such as `**put_your_webhook_here**`. Replace the entire placeholder, including the asterisks: they must not be part of the value.

#|
|| **Placeholder** | **What to replace it with** | **Where to get the value** ||
|| `**put_your_bitrix24_address**` | Bitrix24 address, for example `your-company.bitrix24.com` | Browser address bar ||
|| `**put_your_user_id_here**` | User identifier who created the webhook | inbound webhook URL ||
|| `**put_your_webhook_here**` | inbound webhook secret code | inbound webhook URL ||
|| `**put_access_token_here**` | Valid application access token | [OAuth 2.0](../settings/oauth/index.md) ||
|| `**put_your_client_id_here**`, `**put_your_client_secret_here**` | Application identifier and secret key | Application card ||
|| Handler address such as `https://example.com/handler.php` | Address of your handler, accessible from the internet via HTTPS | Your web server ||
|| Other placeholders, for example `**put_id_here**`, `**put_attach_id**`, `**put_file_name**` | Method parameter value | Data in Bitrix24 ||
|#

An inbound webhook URL looks like this: `https://your-company.bitrix24.com/rest/1/8v5m0dmbxs2ky7wq/`. Here `1` is the user identifier, and `8v5m0dmbxs2ky7wq` is the secret code.

{% note warning "" %}

The webhook secret code and access token provide access to Bitrix24 data. Do not publish them in client-side code, repositories, or screenshots. On a server, store them in environment variables or secure storage; in an application frame, the SDK provides authorization.

{% endnote %}

## How to Execute an Example Without Libraries

Examples in the `cURL (Webhook)` and `cURL (OAuth)` tabs do not require installation: simply replace the placeholders with your own values. The commands are written for Bash. In PowerShell on Windows, call `curl.exe` instead of `curl`, and adapt the `\` line breaks and quotes to PowerShell rules.

The example below creates a deal with the [crm.item.add](../api-reference/crm/universal/crm-item-add.md) method. It is a simplified version of the `cURL (Webhook)` tab from the method page. The `entityTypeId` parameter sets the CRM object type: the value `2` means a deal. The `fields` parameter contains only the `title` field. Bitrix24 fills in fields that have default values on its own.

The call requires the `crm` scope and permission to add deals. If the *Validate required custom fields in REST* option is enabled in the CRM settings, add the required custom fields to `fields`. Each run creates a new deal, so run the example on a test Bitrix24.

Request with placeholders:

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{"entityTypeId":2,"fields":{"title":"New deal"}}' \
https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.add
```

The same request with substituted values:

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{"entityTypeId":2,"fields":{"title":"New deal"}}' \
https://your-company.bitrix24.com/rest/1/8v5m0dmbxs2ky7wq/crm.item.add
```

On the `cURL (OAuth)` tab, the address is shorter — it does not contain the user identifier or the webhook code, and the token is passed in the request body via the `auth` parameter:

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{"entityTypeId":2,"fields":{"title":"New deal"},"auth":"**put_access_token_here**"}' \
https://**put_your_bitrix24_address**/rest/crm.item.add
```

The response is the same for both tabs. If the request succeeds, Bitrix24 returns the created deal in the `result.item` field and its identifier in the `result.item.id` field. The full response is on the method page.

Abbreviated response:

```json
{
    "result": {
        "item": {
            "id": 16,
            "title": "New deal",
            "categoryId": 0,
            "stageId": "NEW",
            "assignedById": 1
        }
    },
    "time": {
        "duration": 1.9278500080108643
    }
}
```

## What to Add to Make an Example Work {#what-to-add}

Most SDK examples begin immediately with a method call and refer to a connection object that is not present in the example itself. Create the connection object following the instructions on the relevant SDK page, then insert the fragment from the documentation. What the example contains and what to prepare yourself:

#|
|| **Tab** | **What the example contains** | **What to prepare** ||
|| `JS (TS)`, `JS` | A call via the `$b24` object; in rare examples, also the creation of `$b24` | The `$b24` connection, if the example does not create it — [{#T}](../sdk/b24jssdk/index.md) ||
|| `JS (UMD)` | A complete HTML page with the `script` tag and the creation of `$b24` | An application that opens this page in a frame within the Bitrix24 interface. Replace placeholders, if any, with your values ||
|| `BX24.js` | A call via the global `BX24` object, for example [BX24.callMethod](../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-method.md) | Library connection — [{#T}](../sdk/bx24-js-sdk/index.md) ||
|| `PHP` | A call by name via `$b24Service->core->call`, less frequently typed services via `$serviceBuilder` | The installed SDK and a connection in the variable used in the example — [{#T}](../sdk/b24phpsdk/index.md) ||
|| `PHP CRest` | The `require_once('crest.php')` line and a `CRest::call` call | SDK files on the server and a completed `settings.php` — [{#T}](../sdk/crest-php-sdk/index.md). To work through an application, install it: CRest receives the tokens during installation ||
|| `Python` | A call via `client` | A client — [{#T}](../sdk/b24pysdk/index.md) ||
|| `Go` | A call via `client` and `ctx` | A client and a context — [{#T}](../sdk/b24gosdk/index.md) ||
|#

Examples in the `JS (TS)`, `JS (UMD)`, and `JS` tabs are designed for an application opened in a frame inside the Bitrix24 interface. In these examples, `$b24` is a `B24Frame` object created by the `initializeB24Frame` function. Server-side code on Node.js needs a different connection class — `B24Hook` or `B24OAuth`. They are described in the [{#T}](../sdk/b24jssdk/index.md) article.

In almost all examples, the link to the UMD build is pinned to the first major version, `@bitrix24/b24jssdk@1`. If the project is running on the second version, replace the number in the link with `@2`.

If you install B24JsSDK from npm, check that its version matches the example. Version 3.0.0 removed the `$b24.callMethod`, `$b24.callListMethod`, `$b24.fetchListMethod`, `$b24.callBatch`, and `$b24.callBatchByChunk` methods, while examples on many pages still use them. To make such an example work, install the second version of the SDK or replace the call with `$b24.actions.v2`, as described in the [Method Invocation Methods](../sdk/b24jssdk/index.md#method-invocation-methods) section.

Response parsing also differs between tools. A request via cURL returns raw JSON, whereas each SDK wraps it in its own way. The response structure is described on the method page in the "Response Handling" section. The SDK page explains how to access the data.

## Field and Parameter Names

Copy field names from the example and from the "Method Parameters" section without changes. Different method groups follow different naming conventions:

- [Universal CRM Methods](../api-reference/crm/universal/index.md), such as [crm.item.add](../api-reference/crm/universal/crm-item-add.md), use camelCase — `title`, `stageId`, `entityTypeId`
- Earlier methods, such as [crm.deal.add](../api-reference/crm/deals/crm-deal-add.md), use uppercase with underscores — `TITLE`, `STAGE_ID`

Universal methods `crm.item.*` also accept uppercase names with underscores, but you should not rely on this.

CRM methods from both groups belong to the previous API version and ignore an unknown field name in `fields`: the value of such a field is not saved, and no error is returned because of it. [REST 3.0](../api-reference/rest-v3.md) methods return an error for such a field.

## Errors and Diagnostics {#errors}

First, check whether the method is marked as deprecated. If it is, its page names a replacement — use it.

The [Errors and Diagnostics](./first-rest-api-call.md#errors) section of the first-request article covers common call errors, such as an incorrect webhook URL or a missing scope. When running examples, the following errors are more common:

#|
|| **What Bitrix24 returned** | **Cause** | **What to do** ||
|| The `expired_token` error | The application access token has expired | Get a new token using the refresh token, as described in the [{#T}](../settings/oauth/auto-renewal.md) article ||
|| An error with status `403` whose description contains `Application context required`, for example `WRONG_AUTH_TYPE` | The method works only in an application context and is not available via a webhook | Call the method from an application ||
|| An access error from the "Error Handling" section of the method | The user on whose behalf the request is executed lacks permissions. For a webhook, this is its creator; for an application, the token owner | Grant the user permissions or execute the request on behalf of another user with the required permissions ||
|| An error in a parameter value, or some of the passed fields are missing from the response | A typo in a field name or an incorrect parameter structure | Check the names against the "Method Parameters" section. Rules for passing arrays and nested structures are described in the [{#T}](../settings/how-to-call-rest-api/general-principles.md) and [{#T}](../settings/how-to-call-rest-api/data-encoding.md) articles ||
|| The `QUERY_LIMIT_EXCEEDED` error | Too many requests in a short time | Reduce the request rate. Limits are described in the [{#T}](../settings/performance/limits.md) article ||
|#

Other codes are listed in the [{#T}](../error-codes.md) article.

## Continue Learning

- [{#T}](./first-rest-api-call.md)
- [{#T}](../settings/how-to-call-rest-api/general-principles.md)
- [{#T}](../settings/how-to-call-rest-api/batch.md)
- [{#T}](../sdk/index.md)
- [{#T}](../tutorials/index.md)
