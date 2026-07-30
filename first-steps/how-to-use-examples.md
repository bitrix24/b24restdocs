# How to Use Examples in the Documentation

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

On method pages in the [API Reference](../api-reference/index.md), examples are grouped in the "Code Examples" block. The tabs show the same call for a cURL request and for the official Bitrix24 SDKs. Most often, the code in a tab is a call fragment rather than a complete project. It does not include library installation, connection to Bitrix24, or your specific parameter values.

Below is a description of how to choose a tab for your environment, what to substitute for placeholders like `**put_your_webhook_here**`, and what code to add to make an example work. After reading this, you will be able to take any example from a method page and execute it in your project.

How an HTTP request to the Bitrix24 REST API is structured — including the address, parameters, and response format — is described in the [{#T}](../settings/how-to-call-rest-api/general-principles.md) article.

## Which Tab to Choose

#|
|| **Tab** | **Tool** | **Where code is executed** | **Authorization** ||
|| `cURL (Webhook)` | Without libraries | Any environment with curl: terminal, script, request testing service | [incoming webhook](../local-integrations/local-webhooks.md) ||
|| `cURL (OAuth)` | Without libraries | Any environment with curl: terminal, script, request testing service | [OAuth 2.0](../settings/oauth/index.md) ||
|| `JS (TS)` | [B24JsSDK](../sdk/b24jssdk/index.md) | Project with a bundler or Node.js, code in TypeScript | [incoming webhook](../local-integrations/local-webhooks.md) or [OAuth 2.0](../settings/oauth/index.md), depending on the connection type ||
|| `JS (UMD)` | [B24JsSDK](../sdk/b24jssdk/index.md), UMD build | HTML page without a bundler | [OAuth 2.0](../settings/oauth/index.md): in examples, `B24Frame` is created ||
|| `BX24.js` | [BX24.js](../sdk/bx24-js-sdk/index.md) | Only an application opened in a frame within the Bitrix24 interface | [OAuth 2.0](../settings/oauth/index.md), the library provides the data automatically ||
|| `PHP` | [B24PhpSDK](../sdk/b24phpsdk/index.md) | Server-side PHP, typed services for each scope | [incoming webhook](../local-integrations/local-webhooks.md) or [OAuth 2.0](../settings/oauth/index.md) ||
|| `PHP CRest` | [CRest PHP SDK](../sdk/crest-php-sdk/index.md) | Server-side PHP, calls via a single `CRest::call` method | [incoming webhook](../local-integrations/local-webhooks.md) or [OAuth 2.0](../settings/oauth/index.md) ||
|| `Python` | [B24PySDK](../sdk/b24pysdk/index.md) | Server-side Python | [incoming webhook](../local-integrations/local-webhooks.md) or [OAuth 2.0](../settings/oauth/index.md) ||
|#

The set of tabs varies across different pages. Each page only contains examples prepared for that specific method.

Tab labels are not formatted identically everywhere. Consider the following variations:

- A tab with a B24JsSDK example might simply be labeled `JS`
- A B24PhpSDK example might be missing — in that case, use CRest or construct the call based on the method parameter descriptions

Do not rely solely on the label; look at the code. For B24PhpSDK, calls are made via `$b24Service` or `$serviceBuilder`; for CRest — via `CRest::call`; for BX24.js — via `BX24.callMethod`; for B24JsSDK — via `$b24`.

`cURL` tabs are suitable when you need to test a method, view a raw response, or call the API from an environment where you cannot install a library, such as a console or a third-party service. For a production integration, use an SDK: it automatically handles authorization, refreshes tokens, respects rate limits, and parses the response.

If multiple SDKs are available, compare them in the [SDK Overview](../sdk/index.md).

## What to Substitute for Placeholders

Instead of real values, examples use placeholders highlighted with double asterisks, such as `**put_your_webhook_here**`. The asterisks are for bold formatting in the page markup and are not part of the value. Replace the entire placeholder, including the asterisks.

#|
|| **Placeholder** | **What to replace it with** | **Where to get the value** ||
|| `**put_your_bitrix24_address**` | Your Bitrix24 address, for example `your-company.bitrix24.com` | Browser address bar ||
|| `**put_your_user_id_here**` | User identifier who created the webhook | incoming webhook URL ||
|| `**put_your_webhook_here**` | incoming webhook secret code | incoming webhook URL ||
|| `**put_access_token_here**` | Valid application access token | [OAuth 2.0](../settings/oauth/index.md) ||
|| `**put_your_client_id_here**`, `**put_your_client_secret_here**` | Application identifier and secret key | Application card ||
|| `**your_handler_url_here**` | Your handler address, accessible from the internet via HTTPS | Your web server ||
|| Other placeholders, for example `**put_id_here**`, `**put_attach_id**`, `**put_file_name**` | Method parameter value | Your data in Bitrix24 ||
|#

An incoming webhook URL looks like this: `https://your-company.bitrix24.com/rest/1/8v5m0dmbxs2ky7wq/`. Here `1` is the user identifier, and `8v5m0dmbxs2ky7wq` is the secret code.

{% note warning "" %}

The webhook secret code and access token provide access to Bitrix24 data. Do not publish them in client-side code, repositories, or screenshots — store them in environment variables on your server.

{% endnote %}

## How to Execute an Example Without Libraries

Examples in the `cURL (Webhook)` and `cURL (OAuth)` tabs do not require installation. Simply replace the placeholders with your own values.

A simplified example based on the `cURL (Webhook)` tab from the [crm.item.add](../api-reference/crm/universal/crm-item-add.md) method page — the set of fields is reduced to one:

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

## What to Add to Make an Example Work

Most SDK examples begin immediately with a method call. The connection object is assumed to be already created. Create it yourself following the instructions on the relevant SDK page, then insert the fragment from the documentation.

Some examples already include the connection — there is no need to duplicate it:

- `JS (UMD)` — a complete, ready-to-use HTML page, including the `script` tag and the creation of `$b24`
- `PHP CRest` — the `require_once('crest.php')` string is usually already present in the example; you only need the SDK files on your server and a completed `settings.php`
- `JS` — occasionally, you will find self-contained examples that include the creation of `$b24`

#|
|| **Tab** | **Object already present in the example** | **What to do** ||
|| `JS (TS)`, `JS` | `$b24` | Create a connection — [{#T}](../sdk/b24jssdk/index.md) ||
|| `JS (UMD)` | `$b24` along with the connection code | Replace placeholders with your values and open the page in the application ||
|| `BX24.js` | Global object `BX24` | Connect the library — [{#T}](../sdk/bx24-js-sdk/index.md) ||
|| `PHP` | `$b24Service`, less frequently `$serviceBuilder` | Install and configure the SDK, assigning the connection to the name used in the example — [{#T}](../sdk/b24phpsdk/index.md) ||
|| `PHP CRest` | Class `CRest` | Install and configure the SDK — [{#T}](../sdk/crest-php-sdk/index.md) ||
|| `Python` | `client` | Create a client — [{#T}](../sdk/b24pysdk/index.md) ||
|#

Examples in the `JS (TS)`, `JS (UMD)`, and `JS` tabs are designed for an application opened within a frame inside the Bitrix24 interface. In these cases, the `$b24` object is created by the `initializeB24Frame` function. For server-side code on Node.js, a different connection class will be required. Classes `B24Hook` and `B24OAuth` are described in the article [{#T}](../sdk/b24jssdk/index.md).

The link to the UMD build in the examples is pinned to the first major version, `@bitrix24/b24jssdk@1`. If the project is running on the second version, replace the number in the link with `@2`.

Response parsing also differs between tools. A request via cURL returns raw JSON, whereas each SDK wraps it in its own way. The composition of the response itself for a specific method is described on its page in the "Response Handling" section, while the method to access the data is described on the SDK page.

## Field and Parameter Names

Copy field names from the example and from the "Method Parameters" section without changes. Different method groups follow different naming conventions:

- Universal CRM methods, such as [crm.item.add](../api-reference/crm/universal/crm-item-add.md), use camelCase — `title`, `stageId`, `entityTypeId`
- Earlier methods, such as [crm.deal.add](../api-reference/crm/deals/crm-deal-add.md), use uppercase with underscores — `TITLE`, `STAGE_ID`

Bitrix24 ignores unknown field names — the item will be saved without that value, and no error will be returned. Universal methods `crm.item.*` also understand uppercase names with underscores, but you should not rely on this. Other methods do not support this conversion.

## If an Example Does Not Work

Check the following in order:

1. **The method is marked as DEPRECATED.** The current replacement is specified on the method page — use it.
2. **The application has not been granted the required scope.** The required scope is specified at the beginning of the method page; the list of values can be found in the [{#T}](../api-reference/scopes/permissions.md) reference.
3. **The user lacks sufficient permissions.** The request is executed on behalf of the entity whose authorization credentials are used: for a webhook, the creator of the webhook; for an application, the owner of the token, typically the user who opened the application.
4. **An error in the parameter structure.** Rules for passing arrays and nested structures are described in the articles [{#T}](../settings/how-to-call-rest-api/general-principles.md) and [{#T}](../settings/how-to-call-rest-api/data-encoding.md).
5. **Too many requests.** Rate limits are described in the article [{#T}](../settings/performance/limits.md).

## Continue Learning

- [{#T}](./first-rest-api-call.md)
- [{#T}](../settings/how-to-call-rest-api/general-principles.md)
- [{#T}](../settings/how-to-call-rest-api/batch.md)
- [{#T}](../sdk/index.md)
- [{#T}](../tutorials/index.md)
