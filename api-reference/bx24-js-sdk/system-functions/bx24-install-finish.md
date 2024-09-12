# Notify about the completion of the installer BX24.installFinish

```js
BX24.installFinish(): void;
```

The function `BX24.installFinish` indicates the completion of the installer or application setup.

If the function is called during the installer startup phase, the page will reload and the application will launch. If called during the setup phase, it will trigger the handlers for [BX24.init](./bx24-init.md). In other cases, there will be no effect from the call.

No parameters.

## Example

```js
document.addEventListener("DOMContentLoaded", function() {
    BX24.init(function(){
        BX24.installFinish();
    });
});
```

## Continue your study

- [{#T}](./bx24-init.md)
- [{#T}](./bx24-install.md)
- [{#T}](./bx24-get-auth.md)
- [{#T}](./bx24-refresh-auth.md)