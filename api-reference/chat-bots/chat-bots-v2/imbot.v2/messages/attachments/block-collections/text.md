# MESSAGE Block

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `MESSAGE` block displays the text portion of the attachment.

![MESSAGE Block](./_images/text.png){width=420}

## Block Parameters

#| 
|| **Name** 
`type` | **Description** ||
|| **MESSAGE*** 
[`string`](../../../../../../data-types.md) | Text of the block. Supports BB codes ||
|#

## Supported BB Codes

#| 
|| **Code** | **Purpose** ||
|| `USER` | Mention a user with a link to their profile in the chat ||
|| `CHAT` | Link to the chat ||
|| `SEND` | Clickable action "send text to chat" ||
|| `PUT` | Clickable action "insert text into input field" ||
|| `CALL` | Clickable action for making a call ||
|| `BR` | Line break ||
|| `B` | Bold text ||
|| `U` | Underlined text ||
|| `I` | Italic text ||
|| `S` | Strikethrough text ||
|| `URL` | Link ||
|#

## Example

{% include [Example Note](../../../../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    {
        MESSAGE: 'The API will be available in the update [B]im 24.0.0[/B]'
    }
    ```

- Python

    ```python
    attach = {
        "MESSAGE": "The API will be available in the [B]im 24.0.0[/B] update",
    }
    ```

- PHP

    ```php
    [
        'MESSAGE' => 'The API will be available in the update [B]im 24.0.0[/B]'
    ]
    ```

{% endlist %}

## Continue Learning

- [API imbot.v2 Change Log](../../../../change-log.md)
- [{#T}](./index.md)
- [{#T}](./delimiter.md)
- [{#T}](./grid.md)