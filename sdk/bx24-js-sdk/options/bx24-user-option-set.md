# Set Configurations for a User BX24.userOption.set

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.userOption.set(string name, any value): void;
```

The `BX24.userOption.set` method sets the value of the `value` configuration with the name `name` for the current user. The value is applied immediately.

Arrays and objects are retained as part of the user configuration set. The set is serialized before writing and restored when read, so the structure can be retrieved using [BX24.userOption.get](./bx24-user-option-get.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../api-reference/data-types.md) | Parameter code ||
|| **value***
[`any`](../../../api-reference/data-types.md) | Configuration value. You can pass a primitive value, array, or object ||
|#

{% note info "" %}

The method is intended for retaining small configuration values. Do not use it to retain large amounts of data.

{% endnote %}

## Code Example

```js
BX24.init(() => {
    BX24.userOption.set('param_str', 'str');
    BX24.userOption.set('param_numb', 1);
    BX24.userOption.set('param_obj', {foo: 'bar'});

    console.log(BX24.userOption.get('param_obj')); // returns {foo: 'bar'}
});
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Response Handling

The method returns nothing. After the call, the retained value is available through [BX24.userOption.get](./bx24-user-option-get.md).

## Error Handling

The method does not return error codes or accept a completion handler.

## Continue Learning

- [{#T}](./bx24-user-option-get.md)
- [{#T}](./bx24-app-option-set.md)
- [{#T}](./bx24-app-option-get.md)
