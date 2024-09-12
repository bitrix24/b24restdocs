# Block with MESSAGE Text

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet writing standards.

{% endnote %}

{% endif %}

![Block with text](./_images/text.png)

`MESSAGE` - outputs simple text without formatting.

## Example

{% list tabs %}

- JS

    ```js
    {
        MESSAGE: "The API will be available in update im 24.0.0"
    },
    ```

- PHP

    ```php
    Array(
        "MESSAGE" => "The API will be available in update im 24.0.0"
    ),
    ```

{% endlist %}

{% include [Footnote about examples](../../../../../_includes/examples.md) %}

The following bb-codes are available in the text: `USER`, `CHAT`, `SEND`, `PUT`, `CALL`, `BR`, `B`, `U`, `I`, `S`, `URL`.