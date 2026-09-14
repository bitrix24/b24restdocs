# DELIMITER Block

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `DELIMITER` block adds a visual separator between parts of an attachment.

![Delimiter Block](./_images/delimiter.png){width=420}

## Block Parameters

#| 
|| **Name** 
`type` | **Description** ||
|| **SIZE** 
[`integer`](../../../../../../data-types.md) | Width of the separator in pixels. If the value is not specified or is incorrect, `200` is used ||
|| **COLOR** 
[`string`](../../../../../../data-types.md) | HEX color of the separator (`#RGB` or `#RRGGBB`) ||
|#

## Example

{% include [Example Notes](../../../../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    {
        DELIMITER: {
            SIZE: 200,
            COLOR: '#c6c6c6'
        }
    }
    ```

- Python

    ```python
    attach = {
        "DELIMITER": {
            "SIZE": 200,
            "COLOR": "#c6c6c6",
        },
    }
    ```

- PHP

    ```php
    [
        'DELIMITER' => [
            'SIZE' => 200,
            'COLOR' => '#c6c6c6'
        ]
    ]
    ```

{% endlist %}

## Continue Learning

- [API imbot.v2 Change Log](../../../../change-log.md)
- [{#T}](./index.md)
- [{#T}](./text.md)
- [{#T}](./grid.md)