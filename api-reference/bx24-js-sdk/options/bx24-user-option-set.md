# Set Settings for User BX24.userOption.set

```js
BX24.userOption.set(string name, string value): void;
```

The method `BX24.userOption.set` sets the `value` of the setting named `name` for the current user. The value is set immediately.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Parameter code ||
|| **value***
[`mixed`](../../data-types.md) | Parameter value ||
|#


## Code Example

```js
BX24.init(() => {
    BX24.userOption.set('param_str', 'str');
    BX24.userOption.set('param_numb', 1);
    BX24.userOption.set('param_obj', {foo: 'bar'});
});
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](./bx24-user-option-get.md)
- [{#T}](./bx24-app-option-set.md)
- [{#T}](./bx24-app-option-get.md)