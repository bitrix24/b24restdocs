# Deleting Related Entities

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

{% note info "landing.landing.removeEntities" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.removeEntities` deletes related entities of the landing page - blocks and images of the blocks.

{% note warning %}

When blocks are deleted, the associated images are removed as well. However, there may be situations where you need to delete images independently of the blocks to clean up clutter. Use this method in that case.

{% endnote %}

## Parameters

#|
|| **Parameters** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Identifier of the landing page. ||
|| **data**
[`unknown`](../../../data-types.md) | An associative array where the key **blocks** contains the blocks to be deleted, and the key **images** contains block-image pairs for which images need to be deleted (the blocks are not deleted in this case). ||
|#

## Example

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

{% include [Footnote on examples](../../../../_includes/examples.md) %}