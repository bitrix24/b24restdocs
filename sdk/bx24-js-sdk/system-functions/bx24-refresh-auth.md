# Force Update Authorization Key BX24.refreshAuth

```js
BX24.refreshAuth(someCallback: function): object
```

The `BX24.refreshAuth` function forcefully updates the authorization key. The handler function `someCallback` will receive an object similar to [BX24.getAuth()](./bx24-get-auth.md). It only works after [BX24.init](./bx24-init.md).

## Function Parameters

#|
|| **Name**
`type` | **Description** ||
|| **someCallback**
[`function`](../../../api-reference/data-types.md) | Accepts a function that will be executed upon success ||
|#

## Example

```js
BX24.init(() => {
    const authInfo = BX24.getAuth();
    console.log('BX24: current authInfo: ', authInfo);

    const button = document.createElement('button');
    button.textContent = 'Refresh auth';
    button.addEventListener('click', () => {
        BX24.refreshAuth((refreshedAuthInfo) => {
            console.log('BX24: refreshed authInfo: ', refreshedAuthInfo);
        })
    });
    document.body.appendChild(button);
});
```

{% include [Note on Examples](../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](./bx24-init.md)
- [{#T}](./bx24-install.md)
- [{#T}](./bx24-install-finish.md)
- [{#T}](./bx24-get-auth.md)