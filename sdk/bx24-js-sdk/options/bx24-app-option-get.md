# Get App Configurations BX24.appOption.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.appOption.get(string name): any | undefined;
```

The `BX24.appOption.get` method returns a configuration by its code.

The method works after [BX24.init](../system-functions/bx24-init.md) and reads the application configurations loaded during library initialization.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../api-reference/data-types.md) | Parameter code ||
|#

## Code Example

```js
BX24.init(() => {
    BX24.appOption.set('param_str', 'str1', (params) => console.log(params));
    BX24.appOption.set('param_numb', 1);

    console.log(BX24.appOption.get('param_str')); // returns str1
    console.log(BX24.appOption.get('param_numb')); // returns 1
    console.log(BX24.appOption.get('unknown')); // returns undefined
});
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Response Handling

The method synchronously returns the application configuration value.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`any`\|`undefined`](../../../api-reference/data-types.md) | If a configuration named `name` is retained, the method returns its value. The type depends on the value passed to [BX24.appOption.set](./bx24-app-option-set.md). If the configuration is not retained, the method returns `undefined` ||
|#

## Error Handling

The method does not return error codes. A missing configuration is not considered an error: the method returns `undefined`.

## Continue Learning

- [{#T}](./bx24-user-option-set.md)
- [{#T}](./bx24-user-option-get.md)
- [{#T}](./bx24-app-option-set.md)
