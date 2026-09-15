# Forcefully Refresh Authorization Data BX24.refreshAuth

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.refreshAuth([someCallback: function]): void;
```

The `BX24.refreshAuth` function forcefully refreshes the [OAuth 2.0](../../../settings/oauth/index.md) authorization data: Bitrix24 issues the application a new pair of `access_token` and `refresh_token`. The refreshed data is passed to the `someCallback` handler function.

The function works only after [BX24.init](./bx24-init.md) and only inside an [application](../../../settings/app-installation/index.md) frame. It does not require a scope of its own.

## When to Call the Function

Requests to Bitrix24 do not need a forced refresh: before a [BX24.callMethod](../how-to-call-rest-methods/bx24-call-method.md) call, the library checks the token expiration by itself, and when a response returns the `expired_token` error, it refreshes the token and repeats the request.

Call `refreshAuth` only if your code needs a fresh token sooner — for example, to pass it to your own server. An `access_token` is valid for one hour. A server that works with Bitrix24 continuously needs both tokens: retain them and replace the previous values with the new ones.

From that point on, the server renews the authorization by itself, without the application frame — with a request to the [authorization server](../../../settings/oauth/index.md). This scenario is described in [OAuth 2.0 Token Automatic Renewal](../../../settings/oauth/auto-renewal.md). The same article explains why authorization should not be renewed on a schedule.

## Function Parameters

#|
|| **Name**
`type` | **Description** ||
|| **someCallback**
[`function`](../../../api-reference/data-types.md) | A handler that runs after the tokens are refreshed. It receives an object with authorization data — the same one that [BX24.getAuth](./bx24-get-auth.md) returns. Without a handler the tokens are refreshed as well, but the application does not receive them ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(() => {
    const button = document.createElement('button');
    button.textContent = 'Refresh auth';
    button.addEventListener('click', () => {
        BX24.refreshAuth((refreshedAuthInfo) => {
            // pass the fresh tokens to your own server so that it works with Bitrix24 on behalf of the application.
            // tokens are secret: do not log them and do not retain them in the browser
            fetch('https://example.com/b24/tokens', {
                method: 'POST',
                headers: {'Content-Type': 'application/json'},
                body: JSON.stringify({
                    memberId: refreshedAuthInfo.member_id,
                    accessToken: refreshedAuthInfo.access_token,
                    refreshToken: refreshedAuthInfo.refresh_token
                })
            })
                .then((response) => console.log('B24: tokens sent, status: ', response.status))
                .catch((error) => console.error('B24: tokens are not sent: ', error));
        })
    });
    document.body.appendChild(button);
});
```

## Response Handling

The function returns no data (`void`). The result is passed to `someCallback`: the tokens and their expiration time in it are new, while `domain` and `member_id` stay the same.

```json
{
    "access_token": "7f2ab466006efd82005fdecc00000000a1c4de93bb6d11e0a3c25f7b91d4c2a8",
    "refresh_token": "b03fd266006efd82005fdecc0000000041e7c85d0f2a44b8a6d1e3c7594fb102",
    "expires_in": 1720015327002,
    "domain": "mycompany.bitrix24.com",
    "member_id": "42bc01fbd89dd1d45d13506933f6f4fc"
}
```

The set of fields, their types, and their purpose are described in [Returned Data of BX24.getAuth](./bx24-get-auth.md#returns).

## Error Handling

The function has no error codes of its own: it does not call the REST API. In case of any failure, the handler does not run.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| The function is called before the library has finished initializing | The call is silently ignored: no request is sent, `someCallback` does not run, and there is no error in the console | Move the call into the [BX24.init](./bx24-init.md) handler ||
|| Bitrix24 could not issue a new token — for example, the application's trial or paid period has ended | Bitrix24 shows a browser `alert` with the text `Unable to get new token! Reload page, please!`, and `someCallback` does not run | Check the application status in Bitrix24. Provide for the application behavior in case the handler does not run ||
|| The page is opened outside the application frame | The library does not initialize: on load it throws the exception `Unable to initialize Bitrix24 JS library!`, and the `BX24` object becomes `null` | Open the page as a Bitrix24 application ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-init.md)
- [{#T}](./bx24-install.md)
- [{#T}](./bx24-install-finish.md)
- [{#T}](./bx24-get-auth.md)
- [{#T}](../../../settings/oauth/auto-renewal.md)
