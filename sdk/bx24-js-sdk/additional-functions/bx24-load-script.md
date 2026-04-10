# Load the javascript file BX24.loadScript

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The method `BX24.loadScript` loads and executes one or more javascript files. If an array is provided, the files are loaded sequentially. If the DOM is not yet ready, meaning the page has not been parsed by the browser and elements are not yet available for the script, execution is deferred until [BX24.ready](./bx24-ready.md).

```js
void BX24.loadScript(Array|String script[, Function callback])
```

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **script*** 
`array|string` | Path to the javascript file or an array of paths. When an array is provided, the files are loaded and executed in order ||
|| **callback** 
`function` | A callback function that is executed after all files have been loaded ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.loadScript(
    [
        '/local/scripts/first.js',
        '/local/scripts/second.js'
    ],
    function () {
        console.log('All scripts have been loaded');
    }
);
```

## Response Handling

The method does not return data (`void`).

## Continue Learning

- [{#T}](./bx24-ready.md)
- [{#T}](./bx24-get-lang.md)