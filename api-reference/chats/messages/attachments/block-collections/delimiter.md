# Block with DELIMITER

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards.

{% endnote %}

{% endif %}

![Block with delimiter](./_images/delimiter.png)

`DELIMITER` - outputs a delimiter.

You can set the size using the SIZE parameter. This parameter is optional.

## Example

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    {
        DELIMITER: {
            SIZE: 200,
        }
    },
    ```

- PHP

    ```php
    Array(
        "DELIMITER" => Array(
            'SIZE' => 200,
        )
    ),
    ```

{% endlist %}