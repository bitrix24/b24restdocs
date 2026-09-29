# LINK Block

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `LINK` block displays a link with a title, a description, and an optional preview image. Pass the block as an element of the attachment's `BLOCKS` array — general rules and limits are described on the [Attachments in Messages ATTACH](../index.md#limits) page.

![LINK Block](./_images/link.png){width=420}

## When to Use LINK

Use `LINK` when a link should appear as a separate card with a title, a description, and a preview — for example, a link to a task, a document, or a deal in a bot notification.

Choose a different block if:

- the link is part of the text. Use the `[URL=https://example.com]text[/URL]` BB code in the [MESSAGE](./text.md) block
- you need to show an employee with an avatar. Use the [USER](./user.md) block
- the link is a value in a property card. Use the `LINK` field of a [GRID](./grid.md) block element

## Block Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **LINK***
[`string`](../../../../../../data-types.md) | URL of the link. Absolute URLs (`http://`, `https://`) and relative paths from the Bitrix24 root are allowed, for example `/crm/deal/details/142/`. This is the only field that makes the block clickable. A block with an invalid URL is skipped without an error ||
|| **NAME**
[`string`](../../../../../../data-types.md) | Title of the link. If not specified, `LINK` is displayed. ||
|| **DESC**
[`string`](../../../../../../data-types.md) | Description under the link title. ||
|| **HTML**
[`string`](../../../../../../data-types.md) | A legacy alternative to `DESC`. The web client displays the tags as plain text, the mobile app does not show the field, and when `HTML` is set, `PREVIEW`, `WIDTH`, and `HEIGHT` are ignored. Use `DESC` ||
|| **PREVIEW**
[`string`](../../../../../../data-types.md) | URL of the preview image. ||
|| **WIDTH**
[`integer`](../../../../../../data-types.md) | Width of the preview in pixels. Applies only together with `PREVIEW` ||
|| **HEIGHT**
[`integer`](../../../../../../data-types.md) | Height of the preview in pixels. Applies only together with `PREVIEW` ||
|| **USER_ID**
[`integer`](../../../../../../data-types.md) | Bitrix24 user ID. It is retained in the attachment, but the web client does not implement navigation by it ||
|| **CHAT_ID**
[`integer`](../../../../../../data-types.md) | Bitrix24 chat ID. It is retained in the attachment, but the web client does not implement navigation by it ||
|| **NETWORK_ID**
[`string`](../../../../../../data-types.md) | Bitrix24 Network user ID: a letter followed by a number. If several IDs are passed, only one is retained, in order of priority: `NETWORK_ID`, `USER_ID`, `CHAT_ID` ||
|#

## Examples

{% include [Example Notes](../../../../../../../_includes/examples.md) %}

The examples show a single element of the `BLOCKS` array.

### External Page with a Preview {#external-link}

{% list tabs %}

- JS

    ```js
    {
        LINK: {
            PREVIEW: 'https://example.com/bitrix/templates/bitrix-new/images/logo.png',
            WIDTH: 1000,
            HEIGHT: 638,
            NAME: 'Ticket #12345: New API for the "Web Messenger" Module',
            DESC: 'Must be implemented by the release!',
            LINK: 'https://api.bitrix24.com/'
        }
    }
    ```

- Python

    ```python
    block = {
        "LINK": {
            "PREVIEW": "https://api.bitrix24.com/bitrix/templates/1c-bitrix-new/images/logo.png",
            "WIDTH": 1000,
            "HEIGHT": 638,
            "NAME": 'Ticket #12345: new API for the "Web Messenger" module',
            "DESC": "Must be implemented by the release!",
            "LINK": "https://api.bitrix24.com/",
        },
    }
    ```

- PHP

    ```php
    [
        'LINK' => [
            'PREVIEW' => 'https://example.com/bitrix/templates/bitrix-new/images/logo.png',
            'WIDTH' => 1000,
            'HEIGHT' => 638,
            'NAME' => 'Ticket #12345: New API for the "Web Messenger" Module',
            'DESC' => 'Must be implemented by the release!',
            'LINK' => 'https://api.bitrix24.com/'
        ]
    ]
    ```

{% endlist %}

### Bitrix24 Page {#portal-link}

A relative path opens the page on the same Bitrix24 account the message was sent from.

{% list tabs %}

- JS

    ```js
    {
        LINK: {
            NAME: 'Deal #142: equipment delivery',
            DESC: 'Responsible: Klaus Weber',
            LINK: '/crm/deal/details/142/'
        }
    }
    ```

- Python

    ```python
    block = {
        "LINK": {
            "NAME": "Deal #142: equipment delivery",
            "DESC": "Responsible: Klaus Weber",
            "LINK": "/crm/deal/details/142/",
        },
    }
    ```

- PHP

    ```php
    [
        'LINK' => [
            'NAME' => 'Deal #142: equipment delivery',
            'DESC' => 'Responsible: Klaus Weber',
            'LINK' => '/crm/deal/details/142/'
        ]
    ]
    ```

{% endlist %}

## Continue Learning

- [API imbot.v2 Change Log](../../../../change-log.md)
- [{#T}](./index.md)
- [{#T}](./text.md)
- [{#T}](./user.md)
- [{#T}](../constructor.md)
- [{#T}](../../chat-message-send.md)