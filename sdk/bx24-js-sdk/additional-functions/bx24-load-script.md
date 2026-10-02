# Load JavaScript Files BX24.loadScript

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.loadScript(script: string | array, callback?: callable): void;
```

The `BX24.loadScript` method adds one or more JavaScript files to the application page and executes them. Files from an array are loaded in sequence: each file is loaded after the previous one has finished loading. This way, you can add a library and then a script that uses it.

If the page is not ready yet, the method itself waits for page readiness — the same moment when the [BX24.ready](./bx24-ready.md) handlers run. The method does not call Bitrix24, so you do not need to wait for [BX24.init](../system-functions/bx24-init.md). The method requires no scope of its own.

{% note warning "" %}

If a file fails to load, for example, because it is not on the server, the method stops without an error: the remaining files are not loaded, and `callback` is not called. The loading failure is visible in the browser console.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **script*** 
[`string`\|`array`](../../../api-reference/data-types.md) | The address of a JavaScript file or an array of addresses. The browser resolves a relative address the same way as for a `<script>` tag on the application page: against the page address, or against the address in the `<base>` tag if there is one, but not against the Bitrix24 address ||
|| **callback** 
[`callable`](../../../api-reference/data-types.md) | Callback function. It is called without parameters when all files are loaded ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

Load a charting library, then a report script that uses it:

```js
BX24.loadScript(
    [
        'js/chart.min.js',
        'js/report.js'
    ],
    function () {
        console.log('All scripts have been loaded');
    }
);
```

## Response Handling

The method does not return data (`void`). When the last file has loaded, the library calls `callback` without parameters. If the page is already ready and an empty array is passed, `callback` runs immediately.

## Error Handling

The method does not return error codes.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| A file failed to load: it is not on the server, or the network connection was lost | Loading stops silently, as described in the warning above | Check the file addresses. If the application needs to know about the failure, limit the wait for `callback` with a timer ||
|| A file contains a syntax error | The browser does not execute this file, but loading continues, and `callback` is called | Do not treat the `callback` call as proof that every file worked: check that the required objects have appeared on the page ||
|| `script` is not passed or is `null` | A `TypeError` exception is thrown, and `callback` is not called. If the page is not ready yet, the exception is thrown later, when the method has finished waiting for page readiness, and a `try...catch` block around the call does not catch it | Pass a file address or an array of addresses ||
|| Something other than a function is passed as `callback` | The files load without an error, but no call is made | Pass a function ||
|| An empty string or an array of empty strings is passed as `script` | No files are loaded, there is no error, and `callback` is called | If the addresses are built in code, check them before the call ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-ready.md)
- [{#T}](./bx24-get-lang.md)