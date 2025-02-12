# Entity Blocks

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet writing standards

{% endnote %}

{% endif %}

> Quick navigation: [all methods](#all-methods) 

A block is HTML code accompanied by a [manifest file](./manifest.md). Here is an example of a simple block:

```html
<section class="landing-block">
    <div class="text-center g-color-gray-dark-v3 g-pa-10">
        <div class="g-width-600 mx-auto">
            <div class="landing-block-node-text g-font-size-12 ">
                <p>&copy; 2017 All rights reserved. Developed by
                <a href="#" class="landing-block-node-link g-color-primary">Bitrix24</a></p>
            </div>
        </div>
    </div>
</section>
```

It is important to remember that when blocks are rendered (both in edit mode and view mode), they are wrapped in a special wrapper `<div id="anchor" class="block-wrapper block-code">...</div>`, where:

- **anchor** – the anchor of the block, if not changed by the user, will be in the format block123, where 123 is the block ID;
- **block-wrapper** – a common class for all blocks;
- **block-code** – a class that depends on the block code, where **code** is the actual code of the block (converted to a safe format).

{% note warning %}

In edit mode, copies of all blocks are created before they are published. Accessing blocks by ID should refer to the draft versions of the blocks.

{% endnote %}

You can order the layout of new blocks from a specialist or use the additional blocks offered by the [vendor](https://htmlstream.com/preview/unify-v2.6/unify-main/shortcodes/index.html).

## Overview of Methods {#all-methods}

#| 
|| **Method** | **Description** | **Available since** ||
|| [landing.block.clonecard](./methods/landing-block-clone-card.md) | Method for cloning a block card. | ||
|| [landing.block.removecard](./methods/landing-block-remove-card.md) | Method for removing a block. | ||
|| [landing.block.updatenodes](./methods/landing-block-update-nodes.md) | Method for changing the content of a block. | ||
|| [landing.block.changeNodeName](./methods/landing-block-change-node-name.md) | Method to change the tag name. | ||
|| [landing.block.updateattrs](./methods/landing-block-update-attrs.md) | Method for changing the attributes of a block node. | ||
|| [landing.block.updateStyles](./methods/landing-block-update-styles.md) | Method for changing the styles of a block. | ||
|| [landing.block.getcontent](./methods/landing-block-get-content.md) | Method for retrieving the content of a block. | ||
|| [landing.block.getlist](./methods/landing-block-get-list.md) | Method for getting a list of blocks on the page. | ||
|| [landing.block.getbyid](./methods/landing-block-get-by-id.md) | Method for retrieving a block by its identifier. | ||
|| [landing.block.getmanifest](./methods/landing-block-get-manifest.md) | Method for retrieving the manifest of a specific block already placed on the page. | ||
|| [landing.block.getmanifestfile](./methods/landing-block-get-manifest-file.md) | Method for retrieving the manifest of a block from the repository. | ||
|| [landing.block.getrepository](./methods/landing-block-get-repository.md) | Method returns a list of blocks from the repository. | ||
|| [landing.block.uploadfile](./methods/landing-block-upload-file.md) | Method uploads an image and associates it with the specified block. | ||
|| [landing.block.updatecontent](./methods/landing-block-update-content.md) | Method updates the content of a block already placed on the page to any arbitrary content. | ||
|| [landing.block.addcard](./methods/landing-block-add-card.md) | Method fully replicates the work of [landing.block.clonecard](./methods/landing-block-clone-card.md) but allows inserting a card with modified content immediately. | ||
|| [landing.block.updateCards](./methods/landing-block-update-cards.md) | Method for bulk updating block cards. | ||
|| [landing.block.changeAnchor](./methods/landing-block-change-anchor.md) | Method changes the symbolic code of the anchor. | ||
|| [landing.block.getContentFromRepository](./methods/landing-block-get-content-from-repository.md) | Method retrieves the content of a block from the repository "as is" before adding the block to any page. | 18.7.500 ||
|#