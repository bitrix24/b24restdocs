# MESSAGE Block

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `MESSAGE` block displays the text part of an attachment: a title, an explanation, or a comment. Pass the block as an element of the attachment's `BLOCKS` array — general rules and limits are described on the [Attachments in Messages ATTACH](../index.md#limits) page.

For name–value pairs, use the [GRID](./grid.md) block. For a standalone link card with a description and a preview, use the [LINK](./links.md) block.

![MESSAGE Block](./_images/text.png){width=420}

## Block Parameters

#| 
|| **Name** 
`type` | **Description** ||
|| **MESSAGE*** 
[`string`](../../../../../../data-types.md) | Text of the block. Supports BB codes ||
|#

## Supported BB Codes {#bb-codes}

The codes in the table work in the web client. In attachment blocks, the mobile app parses only mentions, actions, and line breaks and displays the other codes as plain text. Tags are case-insensitive: `[B]` and `[b]` are equivalent.

#| 
|| **Code** | **Purpose** | **Example** | **Mobile App** ||
|| `USER` | Mention a user with a link to their profile in the chat | `[USER=1]Klaus Weber[/USER]` | Yes ||
|| `CHAT` | Link to the chat | `[CHAT=456]Sales Department[/CHAT]` | Yes ||
|| `SEND` | Clickable action "send text to chat" | `[SEND=/start]Start[/SEND]` | Yes ||
|| `PUT` | Clickable action "insert text into input field" | `[PUT=/help]Help[/PUT]` | Yes ||
|| `CALL` | Clickable action for making a call | `[CALL=+4930123456789]Call[/CALL]` | Yes ||
|| `BR` | Line break | `Line 1[BR]Line 2` | Yes ||
|| `B` | Bold text | `[B]bold[/B]` | No ||
|| `U` | Underlined text | `[U]underlined[/U]` | No ||
|| `I` | Italic text | `[I]italic[/I]` | No ||
|| `S` | Strikethrough text | `[S]strikethrough[/S]` | No ||
|| `URL` | Link | `[URL=https://example.com]link text[/URL]` | No ||
|#

In an attachment, the web client also handles the other codes from the [Text Formatting (BB Codes)](../../message-formatting.md) article, such as `[SIZE]`, `[COLOR]`, and `[CODE]`. The full syntax is described there as well.

## Example

{% include [Example Note](../../../../../../../_includes/examples.md) %}

The example shows a single element of the `BLOCKS` array. Clicking "Subscribe to news" sends the text `/subscribe` to the chat.

{% list tabs %}

- JS

    ```js
    {
        MESSAGE: 'The API will be available in the update [B]im 24.0.0[/B][BR][SEND=/subscribe]Subscribe to news[/SEND]'
    }
    ```

- Python

    ```python
    block = {
        "MESSAGE": "The API will be available in the [B]im 24.0.0[/B] update[BR][SEND=/subscribe]Subscribe to news[/SEND]",
    }
    ```

- PHP

    ```php
    [
        'MESSAGE' => 'The API will be available in the update [B]im 24.0.0[/B][BR][SEND=/subscribe]Subscribe to news[/SEND]'
    ]
    ```

{% endlist %}

## Continue Learning

- [API imbot.v2 Change Log](../../../../change-log.md)
- [{#T}](./index.md)
- [{#T}](./delimiter.md)
- [{#T}](./grid.md)
- [{#T}](../../message-formatting.md)
- [{#T}](../../chat-message-send.md)