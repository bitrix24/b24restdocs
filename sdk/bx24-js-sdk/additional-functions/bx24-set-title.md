# Set the Page Heading BX24.setTitle

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.setTitle(title: string, callback?: callable): void;
```

The `BX24.setTitle` method changes the title of the Bitrix24 page above the application frame. The browser tab name does not change.

The method works only when the application is open on its own page in Bitrix24. In [embedding locations](../../../api-reference/widgets/index.md), for example, in a tab of a CRM detail form, Bitrix24 does not execute the command. Call the method after the library is initialized, in the [BX24.init](../system-functions/bx24-init.md) handler. The method requires no scope of its own: it controls the interface and does not call the REST API.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **title***
[`string`](../../../api-reference/data-types.md) | The new title. The library converts a number to a string ||
|| **callback**
[`callable`](../../../api-reference/data-types.md) | Callback function. It receives an object with the new title [(detailed description)](#callback) ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.setTitle('Requests for the week', function (result) {
        console.log('New title:', result.title);
    });
});
```

## Response Handling {#callback}

The method does not return data (`void`). When Bitrix24 processes the command, it calls `callback` and passes an object with the new title:

```json
{
    "title": "Requests for the week"
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../../api-reference/data-types.md) | The new page title ||
|#

## Error Handling

The method does not return error codes.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| The method is called in an embedding location | The title does not change, and `callback` is not called | Display the title in the application's own interface ||
|| The method is called in the window opened by [BX24.openApplication](./bx24-open-application.md) | The title of the page under the window changes. The window itself has no title. `callback` is called as usual | Display the title in the application's own interface ||
|| `null` or `undefined` is passed in `title` | The application code fails with a JavaScript `TypeError`, and the command is not sent to Bitrix24 | Pass a string ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-scroll-parent-window.md)
- [{#T}](./bx24-reload-window.md)