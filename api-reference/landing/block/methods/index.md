# Object Blocks: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Blocks are the fundamental elements of a website page in Bitrix24. For instance, a block can contain text, an image, a feedback form, or a news list.

The methods `landing.block.*` manage the content of already placed blocks and work with templates from the repository. You can change text, images, styles, add cards, and upload files.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Create and configure your Bitrix24 site](https://helpdesk.bitrix24.com/open/25743741/)

## How to Work with Blocks

1. Retrieve the list of blocks on the page using the [landing.block.getlist](./landing-block-get-list.md) method. You will need the page identifier `lid`.
2. Select the desired block and obtain its data using the [landing.block.getbyid](./landing-block-get-by-id.md) or [landing.block.getcontent](./landing-block-get-content.md) methods.
3. Make changes. Use point methods (`updatenodes`, `updateattrs`) for partial updates or `updatecontent` for a complete content replacement.
4. If you need to add or modify cards, such as list items, use the methods from the [Block Cards](#cards) group.
5. Publish the page using the [landing.landing.publication](../../page/methods/landing-landing-publication.md) method to make the changes visible to visitors.

## Interaction with Other Objects

**Page.** The page identifier `lid` is mandatory for all operations. You can obtain it using the [landing.landing.getList](../../page/methods/landing-landing-get-list.md) method.

**Page Methods.** The methods in the [Blocks on Page](../../page/block-methods/index.md) section add, move, hide, and delete blocks on the page. The `landing.block.*` methods modify the content and configure a specific block.

**Manifest.** The structure of the block, available fields, and settings are described in the manifest. You can obtain the manifest of a placed block using the [landing.block.getmanifest](./landing-block-get-manifest.md) method, and the template from the repository using the [landing.block.getmanifestfile](./landing-block-get-manifest-file.md) method.

## Features of Operation

**Drafts.** Most modification methods work with the draft of the page. For published pages, use the block identifiers from the edit mode `edit_mode`.

**Publication.** Changes in the draft are not visible on the site until the page is published using the [landing.landing.publication](../../page/methods/landing-landing-publication.md) method.

**Files.** The [landing.block.uploadfile](./landing-block-upload-file.md) method only uploads a file and associates it with the block. To display the file in the content, additionally call [landing.block.updatenodes](./landing-block-update-nodes.md).

**Point Update.** To modify individual elements, use `updatenodes`, `updateattrs`, or `updateStyles`. A complete replacement via `updatecontent` requires passing the entire content of the block.

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can perform methods: depending on the method

### Block Cards {#cards}

#|
|| **Method** | **Description** ||
|| [landing.block.addcard](./landing-block-add-card.md) | Adds a card to the block ||
|| [landing.block.clonecard](./landing-block-clone-card.md) | Clones a block card ||
|| [landing.block.updateCards](./landing-block-update-cards.md) | Massively updates block cards ||
|| [landing.block.removecard](./landing-block-remove-card.md) | Removes a card from the block ||
|#

### Content and Parameters of the Block

#|
|| **Method** | **Description** ||
|| [landing.block.updatenodes](./landing-block-update-nodes.md) | Modifies the content of the block nodes ||
|| [landing.block.updateattrs](./landing-block-update-attrs.md) | Changes the attributes of the block nodes ||
|| [landing.block.updateStyles](./landing-block-update-styles.md) | Modifies the styles of the block ||
|| [landing.block.updatecontent](./landing-block-update-content.md) | Completely replaces the content of the block ||
|| [landing.block.changeNodeName](./landing-block-change-node-name.md) | Changes the node tag ||
|| [landing.block.changeAnchor](./landing-block-change-anchor.md) | Modifies the anchor link of the block ||
|| [landing.block.uploadfile](./landing-block-upload-file.md) | Uploads and associates a file with the block ||
|#

### Retrieving Block Data

#|
|| **Method** | **Description** ||
|| [landing.block.getlist](./landing-block-get-list.md) | Returns the list of blocks on the page ||
|| [landing.block.getbyid](./landing-block-get-by-id.md) | Returns block data by ID ||
|| [landing.block.getcontent](./landing-block-get-content.md) | Returns the content of the block ||
|| [landing.block.getmanifest](./landing-block-get-manifest.md) | Returns the manifest of the placed block ||
|#

### Block Repository

#|
|| **Method** | **Description** ||
|| [landing.block.getrepository](./landing-block-get-repository.md) | Returns the list of templates from the repository ||
|| [landing.block.getmanifestfile](./landing-block-get-manifest-file.md) | Returns the manifest of a template from the repository ||
|| [landing.block.getContentFromRepository](./landing-block-get-content-from-repository.md) | Returns the content of a template before adding it to the page ||
|#