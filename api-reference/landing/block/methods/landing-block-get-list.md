# Get the list of page blocks landing.block.getlist

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- corrections needed for standard writing
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

The method `landing.block.getlist` retrieves a list of page blocks. It returns an array of blocks or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier, or an array of identifiers. | ||
|| **params**
[`unknown`](../../../data-types.md) | Parameters:
- **edit_mode** - Editing mode (1) or not (0 - default), will return a different set of blocks. Note that if you have not [published the page](../../page/methods/landing-landing-publication.md) yet, nothing will be returned in mode 0.
- **deleted** - deleted (1) or not (0) blocks, by default all non-deleted blocks are displayed. In `edit_mode=0`, there can be no deleted blocks. | ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.block.getlist',
        {
            lid: 313,
            params: {
                edit_mode: 0
            }
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