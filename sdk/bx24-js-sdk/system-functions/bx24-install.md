# Handle the first launch event of the application by the user BX24.install

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.install(someCallback: function|string): void;
```

The `BX24.install` function allows you to set a handler for the event "the application is launched for the first time by the current user." This event occurs immediately after the "library is ready for use" event, but before the handlers set in [BX24.init](./bx24-init.md) are executed. The handler must signal the completion of the setup process by calling the [BX24.installFinish](./bx24-install-finish.md) function.

If a string is passed as the handler, it is treated as a link to a js file that needs to be loaded and executed when the event is triggered. In any case, if the current user is launching the application for the first time and at least one handler for this event is set, [BX24.init](./bx24-init.md) will only be triggered after the call to [BX24.installFinish](./bx24-install-finish.md).

## Function Parameters

#|
|| **Name**
`type` | **Description** ||
|| **someCallback**
[`function`](../../../api-reference/data-types.md) | Accepts a function that will be executed upon success ||
|| **someCallback**
[`string`](../../../api-reference/data-types.md)  | If a string is provided, it is the path to the js file. The file will be loaded first, then the [BX24.installFinish](./bx24-install-finish.md) function will be called ||
|#

## Example

```js
BX24.install(function(){
    BX24.callMethod('user.current', {}, function(res){
        alert('The Hello World application greets you, ' + res.data().NAME + '!');
        BX24.installFinish();
    });
});
```

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](./bx24-init.md)
- [{#T}](./bx24-install-finish.md)
- [{#T}](./bx24-get-auth.md)
- [{#T}](./bx24-refresh-auth.md)