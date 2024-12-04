# Change the content of the landing.block.updatecontent block

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits are needed for standard writing
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.block.updatecontent` updates the content of an already placed block on the page to any arbitrary content. To change the content part, it is recommended to use the method [landing.block.updatenodes](./landing-block-update-nodes.md). It will return _true_ on success, or an error.

{% note warning %}

- If the new block markup does not match its current manifest, the block may become non-editable.
- Content is passed through a sanitizer, which may remove some suspicious attributes and tags.

{% endnote %}

## Parameters

#|
|| **Method** | **Description** | **Since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier | ||
|| **content**
[`unknown`](../../../data-types.md) | New block content | ||
|| **designed**
[`unknown`](../../../data-types.md) | Optional, defaults to _false_. If _true_ is passed, the block will be considered locked from modification by the system's standard updater. | ||
|#

{% note info %}

The **style** attribute may be stripped by the built-in sanitizer. To bypass this, use the **bxstyle** attribute instead. When added, the system converts it to the standard style.

{% endnote %}

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.block.updatecontent',
        {
            lid: 625,
            block: 11883,
            content: '<h3>My super content</h3>'
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}