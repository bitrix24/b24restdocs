# IMAGE Block

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `IMAGE` block displays one or more images within an attachment.

![IMAGE Block](./_images/img.png){width=420}

## Block Parameters

#| 
|| **Name** 
`type` | **Description** ||
|| **LINK*** 
[`string`](../../../../../../data-types.md) | URL of the original image ||
|| **NAME** 
[`string`](../../../../../../data-types.md) | Name of the image ||
|| **PREVIEW** 
[`string`](../../../../../../data-types.md) | URL of the thumbnail version of the image. If not specified, the `LINK` will be used for preview. It is recommended to specify this explicitly for stable display across different clients ||
|| **WIDTH** 
[`integer`](../../../../../../data-types.md) | Width of the image in pixels. It is recommended to provide this along with `HEIGHT` ||
|| **HEIGHT** 
[`integer`](../../../../../../data-types.md) | Height of the image in pixels. It is recommended to provide this along with `WIDTH` ||
|#

## Example

{% include [Example Note](../../../../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    {
        IMAGE: [
            {
                NAME: 'This is Mantis',
                LINK: 'https://files.shelenkov.com/bitrix/images/mantis.jpg',
                PREVIEW: 'https://files.shelenkov.com/bitrix/images/mantis.jpg',
                WIDTH: 1000,
                HEIGHT: 638
            }
        ]
    }
    ```

- Python

    ```python
    attach = {
        "IMAGE": [
            {
                "NAME": "This is Mantis",
                "LINK": "https://files.shelenkov.com/bitrix/images/mantis.jpg",
                "PREVIEW": "https://files.shelenkov.com/bitrix/images/mantis.jpg",
                "WIDTH": 1000,
                "HEIGHT": 638,
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
                'LINK' => 'https://files.shelenkov.com/bitrix/images/mantis.jpg',
                'PREVIEW' => 'https://files.shelenkov.com/bitrix/images/mantis.jpg',
                'WIDTH' => 1000,
                'HEIGHT' => 638
            ]
        ]
    ]
    ```

{% endlist %}

## Continue Learning

- [API imbot.v2 Change Log](../../../../change-log.md)
- [{#T}](./index.md)
- [{#T}](./links.md)
- [{#T}](./files.md)