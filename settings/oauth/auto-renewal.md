# OAuth 2.0 token automatic renewal

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

By retaining the `refresh_token` refresh token value on its side, the application may subsequently use it to gain access to the REST API without user intervention. Lifetime `refresh_token` is 180 days.

At any time before the expiration of `refresh_token`, the application can make the following request:

```bash
https://oauth.bitrix.info/oauth/token/?
    grant_type=refresh_token
    &client_id=app.573ad8a0346747.09223434
    &client_secret=LJSl0lNB76B5YY6u0YVQ3AW0DrVADcRTwVr4y99PXU1BWQybWK
    &refresh_token=nfhxkzk3gvrg375wl7u7xex9awz6o3k8
```

**Parameters:**

{% include [Note on parameters](../../_includes/required.md) %}

- **grant_type*** — a parameter indicating the type of authorization data to be validated. It must have the value `refresh_token`;
- **client_id*** — the application code obtained in the partner cabinet during application registration, or on the portal in the case of a local application;
- **client_secret*** — the application secret key obtained in the partner cabinet during application registration, or on the portal in the case of a local application;
- **refresh_token*** — the value of the retained refresh token.

In response, the application will receive `json` of the following type:

```json
GET /oauth/token/

HTTP/1.1 200 OK
Content-Type: application/json

{
    "access_token": "ydtj8pho532wydb5ixk78ol7uqlb7sch",
    "client_endpoint": "https://portal.bitrix24.com/rest/",
    "domain": "oauth.bitrix.info",
    "expires": 1780319382,
    "expires_in": 3600,
    "member_id": "a223c6b3710f85df22e9377d6c4f7553",
    "refresh_token": "3s6lr4kr3cv2od4v853gvrchb875bwxb",
    "scope": "app",
    "server_endpoint": "https://oauth.bitrix.info/rest/",
    "status": "T",
    "user_id": 67
}
```

Significant response data:

- **access_token** — the primary authorization token required to access the REST API (lifetime — 1 hour)
- **refresh_token** — a new value of the additional authorization token used to renew the retained authorization
- **client_endpoint** — the portal's REST interface address
- **server_endpoint** — the server's REST interface address
- **status** — the application status on the portal
- **expires** — the expiration moment of `access_token` in Unix time format
- **expires_in** — the lifetime of `access_token` in seconds
- **scope** — a comma-separated list of REST API access rights granted to the application
- **domain** — the authorization server domain
- **member_id** — the unique identifier of the portal
- **user_id** — the identifier of the user for whom the token was issued

At this stage, the application may receive an authorization error. For example, if the trial or paid period has expired, or the application was removed from the portal.

```json
{
    "error": "PAYMENT_REQUIRED",
    "error_description": "Payment required"
}
```

## When should you renew retained tokens?

If your application scenario requires constant background data exchange with Bitrix24 without user involvement, you must plan the storage of refresh tokens and a mechanism for their use.

As previously stated, `refresh_token` remains valid for 180 days. This means that if your application, for some reason, does not contact Bitrix24 "on its own" more frequently to implement application functionality, it is sufficient to contact the authorization server once every 180 days just to renew the retained tokens.

{% note alert "" %}

We have encountered cases where application developers "just in case" first "ping" the authorization server to renew the token before every REST request. This is an incorrect scenario that creates unnecessary load. The same applies to scenarios with automatic "token renewal" once an hour, or once a day — this is redundant.

Moreover, by creating unnecessary load on the authorization server, you risk your application being blocked by automation at some point.

{% endnote %}

## Recommended token handling logic

- Retain the received `access_token` and `refresh_token` token pairs on your side;
- Use the saved `access_token` to perform requests;
- If a specific `access_token` becomes invalid, you will receive a corresponding error in response. At this point, you must:
  - Make a request to the authorization server to obtain a new token pair using the saved `refresh_token`;
  - Retain the new token pair on your side;
  - Repeat the REST method request with the same parameters as before, but with the new `access_token`.