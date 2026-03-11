# IMAGE Block

The `IMAGE` block displays one or more images within an attachment.

![IMAGE Block](./_images/img.png)

## Block Parameters

#|
|| **Name**
`type` | **Description** ||
|| **LINK***
[`string`](../../../../data-types.md) | URL of the original image ||
|| **NAME**
[`string`](../../../../data-types.md) | Name of the image ||
|| **PREVIEW**
[`string`](../../../../data-types.md) | URL of the thumbnail version of the image. If not specified, the `LINK` will be used for preview. It is recommended to explicitly specify this for stable display across different clients ||
|| **WIDTH**
[`integer`](../../../../data-types.md) | Width of the image in pixels. It is advisable to provide this along with `HEIGHT` ||
|| **HEIGHT**
[`integer`](../../../../data-types.md) | Height of the image in pixels. It is advisable to provide this along with `WIDTH` ||
|#

## Example

{% include [Example Note](../../../../../_includes/examples.md) %}

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

- [{#T}](./index.md)
- [{#T}](./links.md)
- [{#T}](./files.md)