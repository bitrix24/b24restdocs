# FILE Block

The `FILE` block displays a file as an attachment element with its name and size.

![FILE Block](./_images/file.png){width=420}

## Block Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **LINK*** 
[`string`](../../../../../../data-types.md) | URL of the file ||
|| **NAME** 
[`string`](../../../../../../data-types.md) | Display name of the file ||
|| **SIZE** 
[`integer`](../../../../../../data-types.md) | Size of the file in bytes. If this field is not specified, the file is displayed without a correct size ||
|#

## Example

{% include [Example Note](../../../../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    {
        FILE: [
            {
                NAME: 'mantis.jpg',
                LINK: 'https://files.shelenkov.com/bitrix/images/mantis.jpg',
                SIZE: 1500000
            }
        ]
    }
    ```

- PHP

    ```php
    [
        'FILE' => [
            [
                'NAME' => 'mantis.jpg',
                'LINK' => 'https://files.shelenkov.com/bitrix/images/mantis.jpg',
                'SIZE' => 1500000
            ]
        ]
    ]
    ```

{% endlist %}

## Continue Learning

- [API imbot.v2 Change Log](../../../../change-log.md)
- [{#T}](./index.md)
- [{#T}](./images.md)
- [{#T}](../index.md)