# OAuth 2.0 token automatic renewal

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

`access_token` lives for one hour. To avoid asking the user to authorize again, the application retains the `refresh_token` on its side and exchanges it for a new pair of tokens without the user. The lifetime of `refresh_token` is 180 days.

The application obtains the first pair of tokens through the [Complete OAuth 2.0 Authorization Protocol](index.md) or the [simplified method](simple-way.md). Renewal works the same way in both cases.

At any time before the expiration of `refresh_token`, the application can make a GET request to the authorization server:

```bash
https://oauth.bitrix.info/oauth/token/?
    grant_type=refresh_token
    &client_id=app.573ad8a0346747.09223434
    &client_secret=LJSl0lNB76B5YY6u0YVQ3AW0DrVADcRTwVr4y99PXU1BWQybWK
    &refresh_token=4f9k4jpmg13usmybzuqknt2v9fh0q6rl
```

Parameters:

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **grant_type*** | The type of authorization data. To renew the tokens, pass the value `refresh_token` ||
|| **client_id*** | The application code from the partner area or from the local application form ||
|| **client_secret*** | The secret key of the application from the partner area or from the local application form ||
|| **refresh_token*** | The retained authorization renewal token ||
|#

{% note warning %}

The secret key `client_secret` is used only in requests to the authorization server **oauth.bitrix.info**. Do not place it in code that runs in the browser.

{% endnote %}

The authorization server responds with the status `200 OK` and a body in the `application/json` format:

```json
{
    "access_token": "ydtj8pho532wydb5ixk78ol7uqlb7sch",
    "client_endpoint": "https://portal.bitrix24.com/rest/",
    "domain": "oauth.bitrix.info",
    "expires": 1780319382,
    "expires_in": 3600,
    "member_id": "a223c6b3710f85df22e9377d6c4f7553",
    "refresh_token": "3s6lr4kr3cv2od4v853gvrchb875bwxb",
    "scope": "crm,entity,im,task",
    "server_endpoint": "https://oauth.bitrix.info/rest/",
    "status": "F",
    "user_id": 67
}
```

Response data:

#|
|| **Parameter** | **Description** ||
|| **access_token** | The new main authorization token for accessing the REST API. Lifetime — one hour ||
|| **refresh_token** | The new value of the renewal token. Retain it instead of the previous one ||
|| **expires** | The expiration moment of `access_token` in Unix time format ||
|| **expires_in** | The lifetime of `access_token` in seconds ||
|| **client_endpoint** | The address of the Bitrix24 REST interface. All method calls start with it ||
|| **server_endpoint** | The address of the REST interface of the authorization server ||
|| **domain** | The domain of the authorization server ||
|| **member_id** | The unique identifier of Bitrix24 ||
|| **scope** | A comma-separated list of access permissions granted to the application ||
|| **status** | The status of the application in Bitrix24. The values match the `STATUS` field of the [app.info](../../api-reference/common/system/app-info.md) method ||
|| **user_id** | The identifier of the user for whom the token was issued ||
|#

At this step, the application may receive an error. For example, if the trial or paid period has expired or the application was removed from Bitrix24.

```json
{
    "error": "PAYMENT_REQUIRED",
    "error_description": "Payment required"
}
```

Other authorization server errors are covered in the article [Authorization Server Error Codes](error-codes.md).

## When to Renew Retained Tokens

If the application constantly exchanges data with Bitrix24 without user involvement, plan the storage of the renewal tokens and a mechanism for their use.

`refresh_token` remains valid for 180 days. If the application does not contact Bitrix24 on its own more frequently, it is enough to request a new pair of tokens once every 180 days so that the retained authorization does not expire.

{% note alert "" %}

Do not renew the token before every REST API request, and do not schedule the renewal for once an hour or once a day. Such scenarios create unnecessary load on the authorization server, and the application may be blocked by automation.

{% endnote %}

## Recommended token handling logic

1. Retain the received pair of `access_token` and `refresh_token` on your side. Keep them in storage that is not accessible from the browser.
2. Call REST API methods with the retained `access_token`.
3. Wait for the `expired_token` error with the status `401` — it means that the token has expired.
4. Request a new pair of tokens from the authorization server using the retained `refresh_token`.
5. Retain the new pair instead of the previous one.
6. Repeat the method call with the same parameters and the new `access_token`.

## What to Do Next

- obtain the first pair of tokens — [Complete OAuth 2.0 Authorization Protocol](index.md)
- handle an authorization server error — [Authorization Server Error Codes](error-codes.md)
- call a method with the new token — [How to Call REST API Methods](../how-to-call-rest-api/index.md)