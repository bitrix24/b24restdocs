# Check the Readiness of the DOM Structure with BX24.isReady

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.isReady(): boolean;
```

The `BX24.isReady` method immediately reports whether the document DOM structure is ready: the browser has parsed the application page, and its elements are accessible to the script. If your code needs to wait until the page is ready, use [BX24.ready](./bx24-ready.md).

This is the library's flag, not the browser's. If the library is loaded programmatically rather than through a `<script>` tag in the markup, the method may return `false` on a page that has already been parsed. Page readiness does not mean that the library has received data from Bitrix24: for that, use [BX24.init](../system-functions/bx24-init.md).

The method works on an application page where the [BX24.js library](../index.md) is connected and does not call Bitrix24. The method requires no scope of its own.

## Method Parameters

No parameters.

## Code Example

{% include [Example Notes](../../../_includes/examples.md) %}

The script is placed in the page markup and runs while the browser is parsing the page:

```html
<script>
    console.log(BX24.isReady()); // false: the browser is still parsing the page

    BX24.ready(function () {
        console.log(BX24.isReady()); // true
    });
</script>
```

## Response Handling

The method synchronously returns a result of type `boolean`. Example of the result when the page is ready:

```json
true
```

### Returned Data

#|  
|| **Name**  
`type` | **Description** ||  
|| **result**  
[`boolean`](../../../api-reference/data-types.md) | `true` if the library has recorded that the document DOM structure is ready, otherwise `false` ||
|#

## Error Handling

The method does not return error codes. While the browser is parsing the page, the method returns `false`, and this is not an error.

## Continue Your Learning

- [{#T}](./index.md)
- [{#T}](./bx24-ready.md)  
- [{#T}](../system-functions/bx24-init.md)  