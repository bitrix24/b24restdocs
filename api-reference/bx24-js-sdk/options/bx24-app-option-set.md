# Set Settings for the Application BX24.appOption.set

```js
BX24.appOption.set(string name, mixed value[, Function callback]): void;
```

The method `BX24.appOption.set` sets global settings for the current application.

Setting application values is only available to users with application management access permission (see [BX24.isAdmin](../additional-functions/bx24-is-admin.md)). A completion handler may be required for application settings (see the `callback` parameter below).

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Parameter code ||
|| **value***
[`mixed`](../../data-types.md) | Parameter value ||
|| **callback**
[`function`](../../data-types.md) | Callback after saving. The current application settings will be passed as an argument ||
|#

## Code Example

```js
BX24.init(() => {
    BX24.appOption.set('param_str', 'str1', (params) => console.log(params));
    BX24.appOption.set('param_numb', 1);
});
```

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](./bx24-user-option-set.md)
- [{#T}](./bx24-user-option-get.md)
- [{#T}](./bx24-app-option-get.md)