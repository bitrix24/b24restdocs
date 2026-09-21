# Set Configurations for the BX24.appOption.set App

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.appOption.set(string name, any value[, Function callback]): void;
```

The `BX24.appOption.set` method sets general configurations for the current application.

Setting application configuration values is only available to users with application management permissions (see [BX24.isAdmin](../additional-functions/bx24-is-admin.md)). An on-completion handler may be required for application configurations (see parameter `callback` below).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../api-reference/data-types.md) | Parameter code ||
|| **value***
[`any`](../../../api-reference/data-types.md) | Parameter value ||
|| **callback**
[`function`](../../../api-reference/data-types.md) | Handler called after the configuration is retained. Receives an object containing the current application configurations [(detailed description)](#callback) ||
|#

### The callback Argument {#callback}

The `callback` receives an object containing the current application configurations. Property names match the configuration codes, and property values match the values retained for those codes.

#|
|| **Name**
`type` | **Description** ||
|| **<configuration code>**
[`any`](../../../api-reference/data-types.md) | Retained configuration value. The property name matches the value of the `name` parameter ||
|#

## Code Example

```js
BX24.init(() => {
    BX24.appOption.set('param_str', 'str1', () => {
        BX24.appOption.set('param_numb', 1, (options) => {
            console.log(options);
        });
    });
});
```

In an application with no other retained configurations, `console.log` outputs the following object:

```js
{
    param_str: 'str1',
    param_numb: 1
}
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Response Handling

The method returns nothing. You can process the retention result using the `callback` parameter. The current application configurations are passed to its argument.

## Error Handling

The method does not return error codes. If the user does not have application management permissions, the configuration is not retained and `callback` is not called. You can check the permission using the [BX24.isAdmin](../additional-functions/bx24-is-admin.md) method.

## Continue Learning

- [{#T}](./bx24-user-option-set.md)
- [{#T}](./bx24-user-option-get.md)
- [{#T}](./bx24-app-option-get.md)
