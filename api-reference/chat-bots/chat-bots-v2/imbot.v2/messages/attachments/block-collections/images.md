# IMAGE Block

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `IMAGE` block displays one or more images within an attachment: a screenshot, a product photo, or a diagram. The value of the `IMAGE` key is an array of objects, one per image. A single object is accepted as well. Pass the block as an element of the attachment's `BLOCKS` array — general rules and limits are described on the [Attachments in Messages ATTACH](../index.md#limits) page.

To attach a file for download, use the [FILE](./files.md) block.

![IMAGE Block](./_images/img.png){width=420}

## IMAGE Element Parameters

#| 
|| **Name** 
`type` | **Description** ||
|| **LINK*** 
[`string`](../../../../../../data-types.md) | URL of the original image: an absolute `http://`/`https://` URL or a path from the Bitrix24 root. An element without a valid URL is skipped without an error ||
|| **NAME** 
[`string`](../../../../../../data-types.md) | Name of the image ||
|| **PREVIEW** 
[`string`](../../../../../../data-types.md) | URL of the thumbnail version of the image, in the same format as `LINK`. If not specified, the `LINK` will be used for preview. It is recommended to specify this explicitly for stable display across different clients ||
|| **WIDTH** 
[`integer`](../../../../../../data-types.md) | Width of the image in pixels ||
|| **HEIGHT** 
[`integer`](../../../../../../data-types.md) | Height of the image in pixels ||
|#

## Example

{% include [Example Note](../../../../../../../_includes/examples.md) %}

The example shows a single element of the `BLOCKS` array with two images. For each image, the message displays a thumbnail from `PREVIEW`, and clicking it opens the original from `LINK`.

{% list tabs %}

- JS

    ```js
    {
        IMAGE: [
            {
                NAME: 'This is Mantis',
                LINK: 'https://example.com/images/mantis.jpg',
                PREVIEW: 'https://example.com/images/mantis-preview.jpg',
                WIDTH: 1000,
                HEIGHT: 638
            },
            {
                NAME: 'Process diagram',
                LINK: 'https://example.com/images/scheme.png',
                PREVIEW: 'https://example.com/images/scheme-preview.png',
                WIDTH: 800,
                HEIGHT: 600
            }
        ]
    }
    ```

- Python

    ```python
    block = {
        "IMAGE": [
            {
                "NAME": "This is Mantis",
                "LINK": "https://example.com/images/mantis.jpg",
                "PREVIEW": "https://example.com/images/mantis-preview.jpg",
                "WIDTH": 1000,
                "HEIGHT": 638,
            },
            {
                "NAME": "Process diagram",
                "LINK": "https://example.com/images/scheme.png",
                "PREVIEW": "https://example.com/images/scheme-preview.png",
                "WIDTH": 800,
                "HEIGHT": 600,
            },
        ],
    }
    ```

- PHP

    ```php
    [
        'IMAGE' => [
            [
                'NAME' => 'This is Mantis',
                'LINK' => 'https://example.com/images/mantis.jpg',
                'PREVIEW' => 'https://example.com/images/mantis-preview.jpg',
                'WIDTH' => 1000,
                'HEIGHT' => 638
            ],
            [
                'NAME' => 'Process diagram',
                'LINK' => 'https://example.com/images/scheme.png',
                'PREVIEW' => 'https://example.com/images/scheme-preview.png',
                'WIDTH' => 800,
                'HEIGHT' => 600
            ]
        ]
    ]
    ```

{% endlist %}

## Continue Learning

- [API imbot.v2 Change Log](../../../../change-log.md)
- [{#T}](./index.md)
- [{#T}](./files.md)
- [{#T}](../constructor.md)
- [{#T}](../../chat-message-send.md)