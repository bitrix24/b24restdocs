# How to Call REST API Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24's REST API methods are called via HTTP. Authorization and parameters are passed in the request, and the response returns either `JSON` or `XML`.

If this is your first call, start with authorization and the general request scheme. If the integration is already operational, you can jump straight to the materials on coding, `batch`, or list methods.

> Quick navigation: [all materials in the section](#all-materials)

## What to Prepare Before the First Call

**Bitrix24 Address.** Prepare the domain to which you will send requests. If Bitrix24 changes its address, handle the redirect according to the recommendations in the article [on changing the Bitrix24 address](./change-domen.md).

**Method Name and Parameters.** Prepare the method name and request parameters. For your first test, take an example from the page [First REST API Call](../../first-steps/first-rest-api-call.md) or a simple reading method, such as `user.current`.

## How to Choose an Authorization Method

#| 
|| **Scenario** | **What to Choose** ||
|| Quick first call, method check, or local integration without an application | Incoming webhook ||
|| Local or mass-market application, working on behalf of different users, access management | OAuth 2.0 ||
|#

For more details, see the article [Authorization in REST](./authorization.md).

## How to Make the First Call {#first-call}

1. Choose an authorization method: webhook or OAuth 2.0
2. Construct the request URL according to the scheme from the article [How a Request is Made](./general-principles.md)
3. Pass parameters in `GET` or `POST`. For methods supporting `JSON`, use `POST` with the request body in this format
4. Encode special characters and complex parameters according to the rules from the article [Data Encoding](./data-encoding.md)
5. Execute the request and check the `result` field or error description in the response
6. If the method returns a list, process the `total` and `next` fields according to the article [Features of List Methods](./list-methods-pecularities.md)
7. If you need to reduce the number of calls, combine calls using [batch](./batch.md)

## Considerations for Working Integrations

**Application.** If the integration operates as an application, the method of obtaining the token and the workflow scenario depend on the installation and context of the application. For more details, see the section [OAuth 2.0](../oauth/index.md).

**Encoding.** Check URL encoding when passing special characters, such as `&`, `?`, `%`, `[`, `]`, and `#`. Also, verify nested requests in [batch](./batch.md).

**Request Body Format.** For arrays and nested objects, send `POST` with `Content-Type: application/json`. This way, the parameter structure is preserved without being converted to a query string.

**Batch Calls.** The [batch](./batch.md) method executes up to 50 commands in a single request and allows the result of the previous command to be used in the next one.

**Limits.** When making frequent and heavy calls, consider the [REST API limits](../performance/limits.md). This is especially important for lists and batch requests.

## All Materials in the Section {#all-materials}

#| 
|| **Material** | **Description** ||
|| [Authorization in REST](./authorization.md) | Explains how to call REST via incoming webhooks and OAuth 2.0 ||
|| [How a Request is Made](./general-principles.md) | Shows the URL structure, parameter transmission formats, and the general response format ||
|| [Data Encoding](./data-encoding.md) | Discusses URL encoding, passing complex structures, and parameter order ||
|| [How to Execute a Batch Request](./batch.md) | Demonstrates how to execute related commands in one call ||
|| [Features of List Methods](./list-methods-pecularities.md) | Explains pagination and obtaining the next page of results ||
|| [Features of REST Calls When Changing the Bitrix24 Address](./change-domen.md) | Explains how to handle redirects after changing the Bitrix24 address ||
|#