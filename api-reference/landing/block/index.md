# Object Blocks: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A block is a fragment of a page that includes an HTML structure, content, cards, nodes, attributes, and style settings.

The methods in this group allow you to:

- read the structure and content of a block
- modify the composition and design of a block
- work with cards and block templates

The structure of a block is described in articles about [attributes](./attributes.md), [node types](./node-types.md), [extended card descriptions](./extended-description.md), and the [manifest file](./manifest.md).

> Quick navigation: [all methods](#all-methods)

## How a Block Relates to a Page and Repository

A block can exist in two states.

**In the repository.** This is a block template that has not yet been added to a page. You can retrieve a list of available blocks from the repository using the [landing.block.getrepository](./methods/landing-block-get-repository.md) method, read the manifest through [landing.block.getmanifestfile](./methods/landing-block-get-manifest-file.md), and obtain the original content using the [landing.block.getContentFromRepository](./methods/landing-block-get-content-from-repository.md) method.

**On the page.** After a block is added to a page, it receives its own ID. You can get a list of the page's blocks using the [landing.block.getlist](./methods/landing-block-get-list.md) method, and then work with a specific block through [landing.block.getbyid](./methods/landing-block-get-by-id.md), [landing.block.getcontent](./methods/landing-block-get-content.md), [landing.block.getmanifest](./methods/landing-block-get-manifest.md), and update methods.

You can add blocks to a page using [landing.landing.addblock](../page/block-methods/landing-landing-add-block.md), move them with [landing.landing.upblock](../page/block-methods/landing-landing-up-block.md) and [landing.landing.downblock](../page/block-methods/landing-landing-down-block.md), hide them with [landing.landing.hideblock](../page/block-methods/landing-landing-hide-block.md), and delete them with [landing.landing.deleteblock](../page/block-methods/landing-landing-delete-block.md).

## How a Block is Structured

A block is not displayed on the page in its original form. During rendering, the system wraps it in a service container `<div id="anchor" class="block-wrapper block-code">...</div>`.

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

In this wrapper:

- **anchor** — the block's anchor. If the user has not manually changed it, it appears as `block123`, where `123` is the block ID.
- **block-wrapper** — a common class for all blocks.
- **block-code** — a class that depends on the block's code, where `code` is a safely transformed version of the block's code.

{% note warning "" %}

In edit mode, the system creates copies of all blocks before they are published. If you access a block by ID, use the block identifier from the page draft. To ensure changes appear on the published page, publish it using the [landing.landing.publication](../page/methods/landing-landing-publication.md) method after editing.

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Reading Block Data on the Page

#| 
|| **Method** | **Description** ||
|| [landing.block.getlist](./methods/landing-block-get-list.md) | Retrieves the list of blocks on the page ||
|| [landing.block.getbyid](./methods/landing-block-get-by-id.md) | Retrieves a block by its identifier ||
|| [landing.block.getcontent](./methods/landing-block-get-content.md) | Retrieves the content of a block ||
|| [landing.block.getmanifest](./methods/landing-block-get-manifest.md) | Retrieves the manifest of a block already placed on the page ||
|#

### Modifying Block Content and Parameters

#| 
|| **Method** | **Description** ||
|| [landing.block.updatenodes](./methods/landing-block-update-nodes.md) | Modifies the content of the block's nodes ||
|| [landing.block.updateattrs](./methods/landing-block-update-attrs.md) | Modifies the attributes of the block's nodes ||
|| [landing.block.updateStyles](./methods/landing-block-update-styles.md) | Modifies the styles of the block ||
|| [landing.block.updatecontent](./methods/landing-block-update-content.md) | Updates the content of the block placed on the page with arbitrary content ||
|| [landing.block.changeNodeName](./methods/landing-block-change-node-name.md) | Changes the tag name of a node ||
|| [landing.block.changeAnchor](./methods/landing-block-change-anchor.md) | Changes the symbolic code of the block's anchor ||
|| [landing.block.uploadfile](./methods/landing-block-upload-file.md) | Uploads a file and associates it with the block ||
|#

### Working with Block Cards

#| 
|| **Method** | **Description** ||
|| [landing.block.clonecard](./methods/landing-block-clone-card.md) | Clones a block card ||
|| [landing.block.addcard](./methods/landing-block-add-card.md) | Adds a block card with modified content ||
|| [landing.block.removecard](./methods/landing-block-remove-card.md) | Removes a block card ||
|| [landing.block.updateCards](./methods/landing-block-update-cards.md) | Massively modifies block cards ||
|#

### Working with the Block Repository

#| 
|| **Method** | **Description** ||
|| [landing.block.getrepository](./methods/landing-block-get-repository.md) | Retrieves the list of blocks from the repository ||
|| [landing.block.getmanifestfile](./methods/landing-block-get-manifest-file.md) | Retrieves the manifest of a block from the repository ||
|| [landing.block.getContentFromRepository](./methods/landing-block-get-content-from-repository.md) | Retrieves the content of a block from the repository before it is added to the page ||
|#