# Error Codes

Bitrix24 reports an error with a JSON structure containing the `error` and `error_description` fields. The structure arrives in the response body instead of the data that the method would return on a successful call. This is how the response to a single call is built — in a `batch` request, subquery errors arrive differently, inside `result`:

```json
{
    "error": "ERROR_HANDLER_ALREADY_EXIST",
    "error_description": "Handler already exists!"
}
```

The `ERROR_HANDLER_ALREADY_EXIST` code from the example was returned by a specific method — it is not part of the system error list.

This page covers the system codes — the ones the REST API returns in response to any method. The codes of a specific method and the codes of the authorization server are listed on other pages: the [Where the Error Code Is Described](#where) section shows where to look for the code you need.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

## Error Response Fields

{% include notitle [error handling](_includes/error-info.md) %}

## How to Recognize an Error in the Response

Recognize an error by the set of fields in the response body, not by the HTTP status: subquery errors of a [batch](settings/how-to-call-rest-api/batch.md) request arrive with status `200`. The status serves another purpose — write it to the log together with the code, it helps when investigating an incident.

#|
|| **What the Response Body Contains** | **What It Means** | **What to Do** ||
|| The `result` field is present, and the `result.result_error` field is empty or absent | The call succeeded | Parse the data from `result` ||
|| The `error` and `error_description` fields are present | The call failed | Parse the error: if the code in `error` is not empty, look it up in the [system error list](#system) or on the method page; if the field is empty, read `error_description` ||
|| The `result.result_error` field is not empty | The [batch](settings/how-to-call-rest-api/batch.md) request was accepted, but some of its subqueries failed | Parse the subquery errors by command keys ||
|#

The `result.result_error` field is always present in a `batch` response: in a successful batch it arrives empty — `"result_error": []`. The sign of an error is the entries in this field, not the field itself. A `batch` request returns a subquery error under the same key the command was passed with in the request. How the `batch` response is built is covered on its [page](settings/how-to-call-rest-api/batch.md).

The `error` field may arrive empty — in that case only `error_description` shows the reason. Errors of a specific method arrive this way, most often parameter validation and object lookup errors. For example, a request for a deal with a non-existent identifier returns status `400` and the body `{"error": "", "error_description": "Not found"}`. Check the response for the presence of the `error` key, not for its value:

```javascript
// build the url according to the rules from the "How a Request Is Executed" section,
// the method name is needed here only for the log
async function callMethod(method, url) {
    const response = await fetch(url);
    const data = await response.json();

    // the call failed: check for the error key, not for its value
    if ('error' in data) {
        // the code goes into a separate field so that the calling code can branch on it,
        // while the status and the method name are kept for the log
        const error = new Error(data.error_description);
        Object.assign(error, { code: data.error, status: response.status, method });
        throw error;
    }

    // batch subquery errors are stored under the command keys
    // in a successful batch the field arrives empty, so count the entries instead of checking for the field
    const batchErrors = data.result?.result_error ?? {};

    // whether to retry the subqueries or abort the scenario is up to the calling code
    return {
        result: data.result,
        batchErrors,
        hasBatchErrors: Object.keys(batchErrors).length > 0
    };
}
```

You do not always have to parse the response manually: [Bitrix24 SDKs](sdk/index.md) take the field checks over and report the error by the means of the language. The form in which the error code arrives depends on the library — see the page of the SDK you use. An integration defines its reaction to a particular code itself, so the rules below apply to work through an SDK as well.

In the `XML` format, the same data arrives as the `<error>` and `<error_description>` elements inside `<response>`. How to choose the response format is described on the [How a Request Is Executed](settings/how-to-call-rest-api/general-principles.md) page.

## Where the Error Code Is Described {#where}

The list of codes depends on which part of Bitrix24 returned the error.

#|
|| **Error Source** | **When It Occurs** | **Where the Codes Are Described** ||
|| Bitrix24 REST API | During the operation of the REST API itself: invalid authorization data, insufficient permissions, exceeded [limits](limits.md), REST API unavailable | [Statuses and System Error Codes](#system) on this page ||
|| A specific method | When the rules of the method itself are violated: invalid parameter format, missing object, unacceptable field value | The "Error Handling" section on the method page — for example, the [crm.item.add](api-reference/crm/universal/crm-item-add.md) method lists there the codes specific to creating an SPA item ||
|| The `oauth.bitrix.info` authorization server | When exchanging the authorization code for tokens and when renewing them | [Authorization Server Error Codes](settings/oauth/error-codes.md) ||
|#

Method errors are described on the method page because they depend on the set of parameters and the state of the object.

## Statuses and System Error Codes {#system}

{% include notitle [system errors](_includes/system-errors.md) %}

## How to Handle Errors in an Integration

Branch on the code from the `error` field, not on the text from `error_description`. The error text of a method arrives in the language of the Bitrix24 interface and may change, so it cannot serve as the basis for branching.

### Reacting to System Codes

The row order matches the order in the table of system errors above, and the causes of the errors are described there.

#|
|| **Code** | **What to Do** ||
|| `INTERNAL_SERVER_ERROR`, `ERROR_UNEXPECTED_ANSWER` | Retry the call in a few seconds. If the error persists, contact the server administrator in the on-premise version or [technical support](bitrix-support.md) ||
|| `QUERY_LIMIT_EXCEEDED` | Reduce the request intensity and retry the call later. The intensity allowed on each plan is listed in the [REST API limits](limits.md) ||
|| `OPERATION_TIME_LIMIT` | Pause the calls to this method. Use the `operating_reset_at` field from the [`time`](api-reference/data-types.md#time) block of the last successful response as the reference point for the first retry. If the retry hits the limit again, wait for the next reset: the mechanics are described in the [REST API limits](limits.md) ||
|| `NO_AUTH_FOUND` | Pass an access token or a webhook code in the request: the current request has neither ||
|| `INVALID_REQUEST` | Replace `http` with `https` in the request URL ||
|| `OVERLOAD_LIMIT` | Do not retry the call, and contact [technical support](bitrix-support.md): the block is manual and will not be lifted on its own ||
|| `ACCESS_DENIED` with status `401` | Switch to a commercial plan. With any other status, this code comes from a method — see the explanation after the table ||
|| `INVALID_CREDENTIALS` | Check the user identifier and the webhook code in the request URL: no active webhook with that pair was found ||
|| `ERROR_METHOD_NOT_FOUND` | Check the spelling of the method name and the presence of the required [scope](api-reference/scopes/permissions.md) ||
|| `insufficient_scope` | Add the missing [scope](api-reference/scopes/permissions.md) to the application or to the webhook ||
|| `expired_token` | [Renew the tokens](settings/oauth/auto-renewal.md) and retry the call ||
|| `user_access_error` | Ask the Bitrix24 administrator to grant the user access to the application ||
|| `PORTAL_DELETED` | Open the public part of the site: while it is closed, the REST API will not respond ||
|#

The same code can appear both in the system list and in the list of a specific method — in that case the HTTP status tells them apart. System authorization and access errors arrive with status `401`, method errors with `400` or `403`. For example, `ACCESS_DENIED` with status `401` means that the REST API is not available on the current plan, while the same code with status `403` and the text `Access denied! Application context required` came from a method that requires an application context.

Retrying a call that returned a method error gives the same result — fix the request first.

### Similar Codes Returned by Methods

Three more codes come from individual methods rather than from the REST API as a whole. The [batch](settings/how-to-call-rest-api/batch.md) request returns `ERROR_BATCH_METHOD_NOT_ALLOWED` when a subquery cannot be executed in a batch, and `ERROR_BATCH_LENGTH_EXCEEDED` for every subquery beyond 50 — both are covered on its page. The `configuration.import.register` method returns `ERROR_MANIFEST_IS_NOT_AVAILABLE` when the request contains no manifest code or the import is not allowed for that manifest.

### What to Write to the Log

Write `error`, `error_description`, the HTTP status, and the name of the called method to the integration log. Do not write authorization data — tokens and webhook codes — to the log: they give access to Bitrix24 data.

## Continue Learning

- [{#T}](settings/how-to-call-rest-api/general-principles.md)
- [{#T}](settings/how-to-call-rest-api/batch.md)
- [{#T}](limits.md)
- [{#T}](settings/oauth/error-codes.md)
- [{#T}](bitrix-support.md)
