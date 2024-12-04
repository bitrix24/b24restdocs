# Remove Related Entities landing.landing.removeEntities

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- corrections needed for writing standards
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

The method `landing.landing.removeEntities` removes related entities of the landing page - blocks and images of the blocks.

{% note warning %}

When blocks are deleted, the associated images are removed in any case. However, there may be situations where it is necessary to delete images independently of the block to clean up junk. Use this method in such cases.

{% endnote %}

## Parameters

#|
|| **Parameters** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Identifier of the landing page. ||
|| **data**
[`unknown`](../../../data-types.md) | Associative array where the key **blocks** contains the blocks to be deleted, and the key **images** contains block-image pairs for which images need to be deleted (the blocks are not deleted in this case). ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.landing.removeEntities',
        {
            lid: 648,
            data: {
                blocks: [12167, 123],
                images: [
                    {
                        block: 12269,
                        image: 6866
                    },
                    {
                        block: 12268,
                        image: 6861
                    }
                ]
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
    // In this example, we are deleting blocks with IDs 12167, 123, as well as image 6866 (from block 12269) and image 6861 (from block 12268).
    // All entities are located in landing page 648.
    ```

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}