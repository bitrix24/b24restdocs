# Automatic Renewal of OAuth 2.0 Tokens

By storing the authorization `refresh_token` value on your side, the application can later use it to access the REST API without user involvement. The lifespan of the `refresh_token` is 180 days.

At any time before the `refresh_token` expires, the application can make the following request:

```bash
https://oauth.bitrix.info/oauth/token/?
    grant_type=refresh_token
    &client_id=app.573ad8a0346747.09223434
    &client_secret=LJSl0lNB76B5YY6u0YVQ3AW0DrVADcRTwVr4y99PXU1BWQybWK
    &refresh_token=nfhxkzk3gvrg375wl7u7xex9awz6o3k8
```

**Parameters:**

{% include [Note on required parameters](../../_includes/required.md) %}

- **grant_type*** — parameter indicating the type of authorization data to be validated. Must be set to `refresh_token`;
- **client_id*** — application code obtained in the partner's account upon application registration, or on the account for local applications;
- **client_secret*** — secret key of the application, obtained in the partner's account upon application registration or on the account for local applications;
- **refresh_token*** — value of the stored authorization renewal token.

In response, the application will receive a `json` of the following format:

```json
GET /oauth/token/

HTTP/1.1 200 OK
Content-Type: application/json

{
    "access_token": "ydtj8pho532wydb5ixk78ol7uqlb7sch",
    "client_endpoint": "https://account.bitrix24.com/rest/",
    "domain": "oauth.bitrix.info",
    "expires_in": 3600,
    "member_id": "a223c6b3710f85df22e9377d6c4f7553",
    "refresh_token": "3s6lr4kr3cv2od4v853gvrchb875bwxb",
    "scope": "app",
    "server_endpoint": "https://oauth.bitrix.info/rest/",
    "status": "T"
}
```

Significant data in the response:

- **access_token** — the main authorization token required for accessing the REST API (lifespan — 1 hour)
- **refresh_token** — new value of the additional authorization token used to extend the stored authorization
- **client_endpoint** — address of the portal's REST interface
- **server_endpoint** — address of the server's REST interface
- **status** — status of the application on the account

At this stage, the application may encounter an authorization error. For example, if the trial or paid period has expired, or if the application has been removed from the account.

```json
{
    "error": "PAYMENT_REQUIRED",
    "error_description": "Payment required"
}
```

## When to Update Stored Tokens?

If your application's scenario requires constant background data exchange with Bitrix24 without user involvement, you need to consider storing refresh tokens and the mechanism for their use.

As mentioned earlier, the `refresh_token` remains valid for 180 days. This means that if your application does not access Bitrix24 frequently "on its own" to implement application functionality, you only need to contact the authorization server once every 180 days to refresh the stored tokens.

{% note alert "" %}

We have encountered cases where application developers "just in case" called the authorization server to refresh the token before every REST request. This is an incorrect scenario that creates unnecessary load. The same applies to scenarios with automatic "token refresh" every hour or every day — this is excessive.

Moreover, by creating unnecessary load on the authorization server, you risk having your application blocked by automation at some point.

{% endnote %}

## Recommended Logic for Working with Tokens

- Store the obtained pairs of `access_token` and `refresh_token` on your side;
- Use the stored `access_token` to make requests;
- If a specific `access_token` has become invalid, you will receive the corresponding error in response. At this point, you should:
  - Make a request to the authorization server to obtain a new pair of tokens using the stored `refresh_token`;
  - Save the new pair of tokens on your side;
  - Repeat the request to the REST method with the same parameters as before, but with the new `access_token`.
