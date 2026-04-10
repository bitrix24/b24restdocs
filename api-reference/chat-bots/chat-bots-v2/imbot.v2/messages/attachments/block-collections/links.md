# LINK Block

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The `LINK` block displays a link with a caption, description, and an optional preview image.

![LINK Block](./_images/link.png){width=420}

## When to Use LINK

The `LINK` block is suitable for manually creating a link block in an attachment.

If you specifically need the format of an expanded link preview, use `RICH_LINK`.

## Block Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **LINK***
[`string`](../../../../../../data-types.md) | URL of the link. Absolute URLs (`http://`, `https://`) and relative paths from the root of Bitrix are allowed. ||
|| **NAME**
[`string`](../../../../../../data-types.md) | Text of the link. If not specified, `LINK` is displayed. ||
|| **DESC**
[`string`](../../../../../../data-types.md) | Description under the link title. ||
|| **HTML**
[`string`](../../../../../../data-types.md) | HTML description. If specified, it is used instead of `DESC`. ||
|| **PREVIEW**
[`string`](../../../../../../data-types.md) | URL of the preview image. ||
|| **WIDTH**
[`integer`](../../../../../../data-types.md) | Width of the preview in pixels. ||
|| **HEIGHT**
[`integer`](../../../../../../data-types.md) | Height of the preview in pixels. ||
|| **USER_ID**
[`integer`](../../../../../../data-types.md) | Link to the Bitrix user (internal navigation). ||
|| **CHAT_ID**
[`integer`](../../../../../../data-types.md) | Link to the Bitrix chat (internal navigation). ||
|| **NETWORK_ID**
[`string`](../../../../../../data-types.md) | Link to the Bitrix24 Network user. ||
|#

## Example

{% include [Example Notes](../../../../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    {
        LINK: {
            PREVIEW: 'https://example.com/bitrix/templates/1c-bitrix-new/images/logo.png',
            WIDTH: 1000,
            HEIGHT: 638,
            NAME: 'Ticket #12345: New API for the "Web Messenger" Module',
            DESC: 'Must be implemented by the release!',
            LINK: 'https://api.bitrix24.com/'
        }
    }
    ```

- PHP

    ```php
    [
        'LINK' => [
            'PREVIEW' => 'https://dev.example..com/bitrix/templates/1c-bitrix-new/images/logo.png',
            'WIDTH' => 1000,
            'HEIGHT' => 638,
            'NAME' => 'Ticket #12345: New API for the "Web Messenger" Module',
            'DESC' => 'Must be implemented by the release!',
            'LINK' => 'https://api.bitrix24.com/'
        ]
    ]
    ```

{% endlist %}

## Continue Learning

- [API imbot.v2 Change Log](../../../../change-log.md)
- [{#T}](./index.md)
- [{#T}](./text.md)
- [{#T}](./images.md)