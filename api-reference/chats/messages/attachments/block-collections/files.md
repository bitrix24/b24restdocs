# File Block FILE

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet the writing standard.

{% endnote %}

{% endif %}

![File Block](./_images/file.png)

`FILE` - displays a formatted link for file upload.

The file size must be specified in bytes.

The fields **NAME** (file name) and **SIZE** (file size) are not mandatory.

## Example

{% list tabs %}

- JS

    ```js
    {
        FILE: {
            NAME: "mantis.jpg",
            LINK: "https://files.shelenkov.com/bitrix/images/mantis.jpg",
            SIZE: 1500000,
        }
    },
    ```

- PHP

    ```php
    Array(
        "FILE" => Array(
            Array(
                "NAME" => "mantis.jpg",
                "LINK" => "https://files.shelenkov.com/bitrix/images/mantis.jpg",
                "SIZE" => "1500000"
            )
        )
    ),
    ```

{% endlist %}

{% include [Footnote about examples](../../../../../_includes/examples.md) %}