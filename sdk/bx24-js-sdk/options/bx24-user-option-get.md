# Get User Settings BX24.userOption.get

```js
BX24.userOption.get(string name): mixed;
```

The method `BX24.userOption.get` returns the value of the setting with the name `name` for the current user.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../api-reference/data-types.md) | Parameter code ||
|#

## Code Examples

```js
BX24.init(() => {
    BX24.userOption.set('param_str', 'str');
    BX24.userOption.set('param_numb', 1);
    BX24.userOption.set('param_obj', {foo: 'bar'});

    console.log(BX24.userOption.get('param_str')); //will return str
    console.log(BX24.userOption.get('param_numb')); //will return 1
    console.log(BX24.userOption.get('param_obj')); //will return {foo: 'bar'}
});
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](./bx24-user-option-set.md)
- [{#T}](./bx24-app-option-set.md)
- [{#T}](./bx24-app-option-get.md)