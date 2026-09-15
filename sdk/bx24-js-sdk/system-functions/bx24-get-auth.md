# Get Data for OAuth 2.0 BX24.getAuth

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.getAuth(): object | false;
```

The `BX24.getAuth` function synchronously returns the application's current authorization data for the OAuth 2.0 protocol: the access and refresh tokens, their expiration time, and the details of the Bitrix24 account the application is opened in.

The function works only after [BX24.init](./bx24-init.md) and only inside an [application](../../../settings/app-installation/index.md) frame. It does not require a scope of its own.

The library inserts the token into requests to Bitrix24 by itself, so there is no need to call `getAuth` before [BX24.callMethod](../how-to-call-rest-methods/bx24-call-method.md). The data is retrieved for other purposes: to link an application installation to your own server by `member_id` and `domain`, or to pass the tokens to a server that calls Bitrix24 on its own.

## Function Parameters

The function takes no parameters.

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(() => {
    const authInfo = BX24.getAuth();

    if (!authInfo) {
        console.error('B24: authorization data is not available');
        return;
    }

    console.log('B24: member_id: ', authInfo.member_id);
    console.log('B24: token expires at: ', new Date(authInfo.expires_in));
});
```

## Response Handling

The function takes no handler. A successful result is an object with five fields.

```json
{
    "access_token": "cd4b8566006efd82005fdecc0000000dccbb3dcc7411d1e5878338535115c7e0",
    "refresh_token": "8e1cd566006efd82005fdecc00000000e5a2fd2b1e16dc6a2b3b5a8b1d7a4f19",
    "expires_in": 1720011727002,
    "domain": "mycompany.bitrix24.com",
    "member_id": "42bc01fbd89dd1d45d13506933f6f4fc"
}
```

### Returned Data {#returns}

#|
|| **Name**
`type` | **Description** ||
|| **access_token**
[`string`](../../../api-reference/data-types.md) | Access token. The library inserts it into requests to Bitrix24 by itself ||
|| **refresh_token**
[`string`](../../../api-reference/data-types.md) | Refresh token. It is used to retrieve a new `access_token` after the current one expires ||
|| **expires_in**
[`integer`](../../../api-reference/data-types.md) | The moment the `access_token` expires — the number of milliseconds elapsed since January 1, 1970. Compare it with `Date.now()`. Its meaning differs from the `expires_in` field in the [OAuth server](../../../settings/oauth/index.md) response, where the value is the token lifetime in seconds ||
|| **domain**
[`string`](../../../api-reference/data-types.md) | Address of the Bitrix24 account the application is opened in, for example `mycompany.bitrix24.com` ||
|| **member_id**
[`string`](../../../api-reference/data-types.md) | Permanent identifier of the Bitrix24 account. It is used to link an application installation to a record on your side ||
|#

## Error Handling

The function has no error codes of its own: it does not call the REST API but reads the data that the application frame received from Bitrix24 during initialization. Instead of an error, the function returns `false`.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| The function is called before the library has finished initializing | Returns `false` | Move the call into the [BX24.init](./bx24-init.md) handler ||
|| The `access_token` has expired | Returns `false` | Call any Bitrix24 method: the library refreshes the token by itself. If your code needs a fresh token sooner, call [BX24.refreshAuth](./bx24-refresh-auth.md) ||
|| The page is opened outside the application frame | The library does not initialize: on load it throws the exception `Unable to initialize Bitrix24 JS library!`, and the `BX24` object becomes `null` | Open the page as a Bitrix24 application. Authorization data cannot be checked in a regular browser tab or on your own website ||
|#

{% note warning "" %}

Tokens are an application secret. Do not log `access_token` and `refresh_token` and do not retain them in browser storage: pass them to your own server and retain them there.

{% endnote %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-init.md)
- [{#T}](./bx24-install.md)
- [{#T}](./bx24-install-finish.md)
- [{#T}](./bx24-refresh-auth.md)
- [{#T}](../../../settings/oauth/index.md)
