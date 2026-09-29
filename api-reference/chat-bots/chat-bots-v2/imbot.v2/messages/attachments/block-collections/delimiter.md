# DELIMITER Block

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `DELIMITER` block adds a horizontal line between parts of an attachment. Use it to separate a card title from its properties or the main text from links.

Pass the block as an element of the attachment's `BLOCKS` array, between the blocks you want to separate. General attachment rules and limits are described on the [Attachments in Messages ATTACH](../index.md#limits) page.

![Delimiter Block](./_images/delimiter.png){width=420}

## Block Parameters

#| 
|| **Name** 
`type` | **Description** ||
|| **SIZE** 
[`integer`](../../../../../../data-types.md) | Width of the separator in pixels. If the value is not specified or is incorrect, `200` is used ||
|| **COLOR** 
[`string`](../../../../../../data-types.md) | HEX color of the separator (`#RGB` or `#RRGGBB`), for example `#c6c6c6`. If it is not specified or is incorrect, the client draws the line in its default color ||
|#

## Example

{% include [Example Notes](../../../../../../../_includes/examples.md) %}

A separator between a card title and its properties. The example shows the entire `BLOCKS` array, with the `DELIMITER` block as the second element.

{% list tabs %}

- JS

    ```js
    BLOCKS: [
        { MESSAGE: '[B]Request #142[/B]' },
        {
            DELIMITER: {
                SIZE: 200,
                COLOR: '#c6c6c6'
            }
        },
        { GRID: [{ DISPLAY: 'ROW', NAME: 'Status', VALUE: 'In progress' }] }
    ]
    ```

- Python

    ```python
    blocks = [
        {"MESSAGE": "[B]Request #142[/B]"},
        {
            "DELIMITER": {
                "SIZE": 200,
                "COLOR": "#c6c6c6",
            },
        },
        {"GRID": [{"DISPLAY": "ROW", "NAME": "Status", "VALUE": "In progress"}]},
    ]
    ```

- PHP

    ```php
    'BLOCKS' => [
        ['MESSAGE' => '[B]Request #142[/B]'],
        [
            'DELIMITER' => [
                'SIZE' => 200,
                'COLOR' => '#c6c6c6'
            ]
        ],
        ['GRID' => [['DISPLAY' => 'ROW', 'NAME' => 'Status', 'VALUE' => 'In progress']]]
    ]
    ```

{% endlist %}

## Continue Learning

- [API imbot.v2 Change Log](../../../../change-log.md)
- [{#T}](./index.md)
- [{#T}](./text.md)
- [{#T}](./grid.md)
- [{#T}](../constructor.md)
- [{#T}](../../chat-message-send.md)