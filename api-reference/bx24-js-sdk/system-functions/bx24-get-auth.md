# Get Data for OAuth 2.0 BX24.getAuth

```js
BX24.getAuth(): boolean|object;
```

The function `BX24.getAuth` retrieves the current authorization data via the OAuth 2.0 protocol. It returns an object in the following format:

```json
{
    access_token: "cd4b8566006efd82005fdecc000000007dccbb3dcc7411d1e5878338535115c7e",
    domain: "b24.yurta.bx",
    expires_in: 1720011727002,
    member_id: "42bc01fbd89dd1d45d13506933f6f4fc",
    refresh_token: "bdc"
}
```

The expiration date is provided as a `date` object.

It only works after [BX24.init](./bx24-init.md). If called before the application is initialized or after the token has expired, it will return `false`. When the token expires, a new one is automatically generated on the next call to **BX24.callMethod** or **BX24.refreshAuth**.

No parameters required.

## Example

```js
document.addEventListener("DOMContentLoaded", function() {
    BX24.init(() => {
        const authInfo = BX24.getAuth();
        console.log('B24: authInfo: ', authInfo);	
    });
});
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](./bx24-init.md)
- [{#T}](./bx24-install.md)
- [{#T}](./bx24-install-finish.md)
- [{#T}](./bx24-refresh-auth.md)