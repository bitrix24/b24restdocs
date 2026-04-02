# LINK Block

The `LINK` block displays a link with a title, description, and an optional preview image.

![LINK Block](./_images/link.png){width=420}

## When to Use LINK

`LINK` is suitable for manually creating a link block in an attachment.

If you specifically need the format for an extended link preview, use `RICH_LINK`.

## Block Parameters

#|
|| **Name**
`type` | **Description** ||
|| **LINK***
[`string`](../../../../../../data-types.md) | URL of the link. Absolute URLs (`http://`, `https://`) and relative paths from the root of Bitrix are allowed. ||
|| **NAME**
[`string`](../../../../../../data-types.md) | Link text. If not specified, `LINK` is displayed. ||
|| **DESC**
[`string`](../../../../../../data-types.md) | Description under the link title. ||
|| **HTML**
[`string`](../../../../../../data-types.md) | HTML description. If provided, it is used instead of `DESC`. ||
|| **PREVIEW**
[`string`](../../../../../../data-types.md) | URL of the preview image. ||
|| **WIDTH**
[`integer`](../../../../../../data-types.md) | Width of the preview in pixels. ||
|| **HEIGHT**
[`integer`](../../../../../../data-types.md) | Height of the preview in pixels. ||
|| **USER_ID**
[`integer`](../../../../../../data-types.md) | Link to a Bitrix user (internal navigation). ||
|| **CHAT_ID**
[`integer`](../../../../../../data-types.md) | Link to a Bitrix chat (internal navigation). ||
|| **NETWORK_ID**
[`string`](../../../../../../data-types.md) | Link to a Bitrix24 Network user. ||
|#

## Example

{% include [Example Note](../../../../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    {
        LINK: {
            PREVIEW: 'https://dev.1c-bitrix.com/bitrix/templates/1c-bitrix-new/images/logo.png',
            WIDTH: 1000,
            HEIGHT: 638,
            NAME: 'Ticket #12345: New API for the "Web Messenger" Module',
            DESC: 'Implementation required by the release!',
            LINK: 'https://api.bitrix24.com/'
        }
    }
    ```

- PHP

    ```php
    [
        'LINK' => [
            'PREVIEW' => 'https://dev.1c-bitrix.com/bitrix/templates/1c-bitrix-new/images/logo.png',
            'WIDTH' => 1000,
            'HEIGHT' => 638,
            'NAME' => 'Ticket #12345: New API for the "Web Messenger" Module',
            'DESC' => 'Implementation required by the release!',
            'LINK' => 'https://api.bitrix24.com/'
        ]
    ]
    ```

{% endlist %}

## Continue Learning

- [ATTACH Block Collection](./index.md)
- [MESSAGE Block](./text.md)
- [IMAGE Block](./images.md)
