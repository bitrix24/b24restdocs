# How to Call REST API Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24's REST API methods are called via HTTP. Authorization data and method parameters are passed in the request, and the response returns either `JSON` or `XML`. You can send a request from any language or tool that works with the HTTP protocol.

The materials in this section explain how to build such a request and what to consider in a production integration. Descriptions of individual methods and their parameters are in the [Method Reference](../../api-reference/index.md).

If this is your first call, start with authorization and the general request scheme. If the integration is already operational, you can jump straight to the materials on coding, `batch`, or list methods.

> Quick navigation: [all materials in the section](#all-materials)

## What to Prepare Before the First Call {#before-start}

**Bitrix24 Address.** Prepare the domain to which you will send requests. If Bitrix24 changes its address, handle the redirect according to the recommendations in the article [on changing the Bitrix24 address](./change-domen.md).

**Method Name and Parameters.** Prepare the method name and request parameters. For your first test, take an example from the page [First REST API Call](../../first-steps/first-rest-api-call.md) or a simple reading method, such as `user.current`.

## How to Choose an Authorization Method {#auth}

#| 
|| **Scenario** | **What to Choose** ||
|| Quick first call, method check, or local integration without an application | Incoming webhook ||
|| Local or mass-market application, working on behalf of different users, access management | OAuth 2.0 ||
|#

The methods differ only in how the authorization data gets into the request:

- for a webhook, the user ID and the secret code are built into the path — `https://your-domain.bitrix24.com/rest/USER_ID/WEBHOOK_CODE/method`
- for an application, a temporary token is passed in the `auth` parameter — `https://your-domain.bitrix24.com/rest/method?auth=ACCESS_TOKEN`

For more details, see the article [Authorization in REST](./authorization.md).

## How to Make the First Call {#first-call}

1. Choose an authorization method: webhook or OAuth 2.0
2. Construct the request URL according to the scheme from the article [How a Request is Made](./general-principles.md)
3. Pass parameters in `GET` or `POST`. For methods supporting `JSON`, use `POST` with the request body in this format
4. Encode special characters and complex parameters according to the rules from the article [Data Encoding](./data-encoding.md)
5. Execute the request and check the `result` field or error description in the response
6. If the method returns a list, process the `total` and `next` fields according to the article [Features of List Methods](./list-methods-pecularities.md)
7. If you need to reduce the number of calls, combine calls using [batch](./batch.md)

**Call example.** The `user.current` method returns the data of the user on whose behalf the request is executed. It takes no parameters, which makes it convenient for checking authorization:

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{}' \
https://your-domain.bitrix24.com/rest/1/8g9l071eismy9q2l/user.current.json
```

The address contains the Bitrix24 domain, the ID of the user who created the webhook, the webhook secret code, and the method name. A successful response contains the data in the `result` field and the execution time in the `time` object:

```json
{
    "result": {
        "ID": "1",
        "ACTIVE": true,
        "NAME": "Klaus",
        "LAST_NAME": "Weber"
    },
    "time": {
        "start": 1721722262.960948,
        "finish": 1721722262.985244,
        "duration": 0.024296045303344727,
        "processing": 0.0012989044189453125,
        "date_start": "2024-07-23T08:11:02+00:00",
        "date_finish": "2024-07-23T08:11:02+00:00",
        "operating": 0
    }
}
```

The response example keeps only the key fields. For the full set of data, see the [user.current](../../api-reference/user/user-current.md) method page.

If the authorization data is incorrect or the user lacks permissions, an error description arrives instead of `result`. For the explanations, see the article [Error Codes](../../error-codes.md).

## Permissions and Security {#security}

**User permissions.** A method always runs on behalf of a specific Bitrix24 user: for a webhook, on behalf of the user who created it; for an application, on behalf of the token owner. Through REST you can access exactly what that user can access in the interface.

**Scopes.** An application is additionally limited by the set of [scopes](../../api-reference/scopes/permissions.md) requested at installation. For a webhook, the scopes are selected when it is created. If a method returns an access error, check both the user permissions and the granted scopes.

**Secrets.** The webhook code and the application token grant access to Bitrix24 data. Do not publish them, pass them to client-side code, or write them to logs. An OAuth 2.0 token has a limited lifetime and is refreshed — the procedure is described in the article [OAuth 2.0 Token Automatic Renewal](../oauth/auto-renewal.md).

**Administrative access.** A local application is added by a Bitrix24 administrator. A webhook can be created by any user within their own permissions.

## Considerations for Working Integrations {#production}

**Application.** If the integration operates as an application, the method of obtaining the token and the workflow scenario depend on the installation and context of the application. For more details, see the section [OAuth 2.0](../oauth/index.md).

**Encoding.** Check URL encoding when passing special characters, such as `&`, `?`, `%`, `[`, `]`, and `#`. Also, verify nested requests in [batch](./batch.md).

**Request Body Format.** For arrays and nested objects, send `POST` with `Content-Type: application/json`. This way, the parameter structure is preserved without being converted to a query string.

**Batch Calls.** The [batch](./batch.md) method executes up to 50 commands in a single request and allows the result of the previous command to be used in the next one.

**Limits.** When making frequent and heavy calls, consider the [REST API limits](../performance/limits.md). This is especially important for lists and batch requests. How to reduce the load is described in the article [Performance Recommendations](../performance/index.md).

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

## Continue Learning

- [{#T}](../../first-steps/first-rest-api-call.md)
- [{#T}](../oauth/index.md)
- [{#T}](../performance/limits.md)
- [{#T}](../../api-reference/index.md)