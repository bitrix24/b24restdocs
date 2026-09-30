# FILE Block

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `FILE` block adds files to a message by link — for example, a report, a contract, or a data export that is already hosted on a website or in cloud storage. This way, a bot can send an employee a ready-made report from your accounting system. The value of the `FILE` key is an array of objects, one per file. A single object is accepted as well. Pass the block as an element of the attachment's `BLOCKS` array — general rules and limits are described on the [Attachments in Messages ATTACH](../index.md#limits) page.

Bitrix24 does not copy the file from `LINK` and does not manage access permissions to it: the recipient can open the file only if they have access to the website or storage that hosts it.

Choose a different approach if you need to:

- upload a file to Bitrix24 and send it to a chat — use the [imbot.v2.File.upload](../../../files/file-upload.md) method
- show an image directly in the message — use the [IMAGE](./images.md) block

![FILE Block](./_images/file.png){width=420}

## FILE Element Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **LINK*** 
[`string`](../../../../../../data-types.md) | URL of the file: an absolute `http://`/`https://` URL or a path from the Bitrix24 root. A file with a link in any other format is not added to the message, while the other files in the block remain ||
|| **NAME** 
[`string`](../../../../../../data-types.md) | File name with the extension, for example `report.pdf`. The web client picks an icon based on the extension and labels a file without a name as "Untitled" ||
|| **SIZE** 
[`integer`](../../../../../../data-types.md) | Size of the file in bytes. Bitrix24 does not check it against the actual file. If the field is not specified or equals `0`, the message displays the file without a size ||
|#

## Example

{% include [Example Note](../../../../../../../_includes/examples.md) %}

The example shows a single element of the `BLOCKS` array with two files. In the web client, each file displays its name and size, and clicking a file opens the link from `LINK` in a new tab.

{% list tabs %}

- JS

    ```js
    {
        FILE: [
            {
                NAME: 'September report.pdf',
                LINK: 'https://example.com/files/report-september.pdf',
                SIZE: 1500000
            },
            {
                NAME: 'Price list.xlsx',
                LINK: 'https://example.com/files/price-list.xlsx',
                SIZE: 48213
            }
        ]
    }
    ```

- Python

    ```python
    block = {
        "FILE": [
            {
                "NAME": "September report.pdf",
                "LINK": "https://example.com/files/report-september.pdf",
                "SIZE": 1500000,
            },
            {
                "NAME": "Price list.xlsx",
                "LINK": "https://example.com/files/price-list.xlsx",
                "SIZE": 48213,
            },
        ],
    }
    ```

- PHP

    ```php
    [
        'FILE' => [
            [
                'NAME' => 'September report.pdf',
                'LINK' => 'https://example.com/files/report-september.pdf',
                'SIZE' => 1500000
            ],
            [
                'NAME' => 'Price list.xlsx',
                'LINK' => 'https://example.com/files/price-list.xlsx',
                'SIZE' => 48213
            ]
        ]
    ]
    ```

{% endlist %}

## Continue Learning

- [API imbot.v2 Change Log](../../../../change-log.md)
- [{#T}](./index.md)
- [{#T}](./images.md)
- [{#T}](../../../files/file-upload.md)
- [{#T}](../constructor.md)
- [{#T}](../../chat-message-send.md)