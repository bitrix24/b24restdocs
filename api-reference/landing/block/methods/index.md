# Working with Blocks: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `landing.block.*` methods read and modify a block that is already placed on a site page: its content, nodes, attributes, styles, and cards. A separate group of methods works with block templates from the repository — before a block is added to a page.

For example, you can replace the text and the image in a cover block, add a card to a list of services, and upload a new image.

The block itself is described in the [Blocks Object](../index.md) section, which also collects the articles about the manifest, nodes, cards, and attributes.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Create and configure your Bitrix24 site](https://helpdesk.bitrix24.com/open/25743741/)

## How to Modify a Block

1. Retrieve the page identifier `lid` using the [landing.landing.getList](../../page/methods/landing-landing-get-list.md) method.
2. Retrieve the list of blocks on the page using the [landing.block.getlist](./landing-block-get-list.md) method with the `params.edit_mode = true` parameter. This returns the identifiers of the draft blocks — exactly the ones the modification methods need.
3. Check what can be changed in the block: the manifest is returned by [landing.block.getmanifest](./landing-block-get-manifest.md) with the `params.edit_mode = true` parameter, and the current HTML by [landing.block.getcontent](./landing-block-get-content.md) with the `editMode = true` parameter. Edit mode is set differently for these methods: `getmanifest` uses `params.edit_mode`, while `getcontent` uses a separate `editMode` parameter. Without it, the method returns the published version of the block.
4. Modify the block with a suitable method from the table below.
5. Publish the page using the [landing.landing.publication](../../page/methods/landing-landing-publication.md) method to make the changes visible to visitors.

## How to Choose a Method {#how-to-choose}

#|
|| **If You Need To** | **Use the Method** ||
|| Find the blocks on a page and learn their identifiers | [landing.block.getlist](./landing-block-get-list.md) ||
|| Retrieve the data of a single block by its identifier | [landing.block.getbyid](./landing-block-get-by-id.md) ||
|| View the current HTML of a block | [landing.block.getcontent](./landing-block-get-content.md) ||
|| Find out which selectors and settings a block has | [landing.block.getmanifest](./landing-block-get-manifest.md) ||
|| Replace the text, the image, or a link in a block | [landing.block.updatenodes](./landing-block-update-nodes.md) ||
|| Change the value of a setting retained in a DOM attribute | [landing.block.updateattrs](./landing-block-update-attrs.md) ||
|| Change the design of a block or of an individual element | [landing.block.updateStyles](./landing-block-update-styles.md) ||
|| Replace the entire HTML of a block | [landing.block.updatecontent](./landing-block-update-content.md) ||
|| Add, copy, or remove an element of a repeatable list | Methods of the [Block Cards](#cards) group ||
|| Upload a new image into a block | [landing.block.uploadfile](./landing-block-upload-file.md), then [landing.block.updatenodes](./landing-block-update-nodes.md) ||
|| Change the tag of a node, `h2` to `h3` for example | [landing.block.changeNodeName](./landing-block-change-node-name.md) ||
|| Change the anchor of a block for links on the page | [landing.block.changeAnchor](./landing-block-change-anchor.md) ||
|| Publish a single block without publishing the other page edits | [landing.block.publication](./landing-block-publication.md) ||
|| Find a suitable block template before adding it to a page | [landing.block.getrepository](./landing-block-get-repository.md) ||
|| View the original manifest or HTML of a block template | [landing.block.getmanifestfile](./landing-block-get-manifest-file.md), [landing.block.getContentFromRepository](./landing-block-get-content-from-repository.md) ||
|#

The selectors for all modification methods are taken from the block manifest: nodes are described in the [Node Types](../node-types.md) article, attributes in the [Attributes](../attributes.md) article, cards and presets in the [Extended Description of Cards](../extended-description.md) article, and label translations in the [Block Localization](../localization.md) article.

Point methods change only the selectors passed to them, so they do not need the full HTML of the block. The [landing.block.updatecontent](./landing-block-update-content.md) method replaces the content in full: everything not passed in the request is lost from the block. If the block designer is not available in Bitrix24 — which is a plan limitation rather than a property of a specific block — a call with the `designed = true` parameter changes nothing and returns `null`.

The [landing.block.uploadfile](./landing-block-upload-file.md) method only uploads a file and associates it with the block. To make the image appear in the block, substitute the resulting `src` into the required node using the [landing.block.updatenodes](./landing-block-update-nodes.md) method.

## Relationship with Other Objects

**Page.** The page identifier `lid` is required by all methods that work with a block on a page. You can obtain it using the [landing.landing.getList](../../page/methods/landing-landing-get-list.md) method. The changes are retained in the page draft and appear on the site only after the [landing.landing.publication](../../page/methods/landing-landing-publication.md) method is called.

**Block Placement.** The methods of the [Blocks on Page](../../page/block-methods/index.md) section add a block to a page, move it, hide it, and delete it. The `landing.block.*` methods do not control placement — they modify a block that has already been added.

**Manifest.** Which selectors can be passed to the modification methods is determined by the block manifest. The manifest of a placed block is returned by the [landing.block.getmanifest](./landing-block-get-manifest.md) method, and the structure of the file is covered in the [Block Manifest](../manifest.md) article.

**Block Repository.** Before being added to a page, a block exists as a template with a symbolic code, `01.big_with_text` for example. How the repository is organized and which methods read it is described on the [Blocks Object](../index.md) page.

## Common Rules of the Section

The rules are the same for all `landing.block.*` methods and are covered in detail on the [Blocks Object](../index.md) page:

- **Permissions.** Access to the Sites and Stores section is required for any call, while at the site level reading requires the "view" permission and modification requires the "edit" permission. The exact wording is in the header of every method
- **The `scope` parameter.** For blocks of knowledge bases, group knowledge bases, and the Bitrix24 main page, it is passed in every call, otherwise the method will not find the block
- **Limits.** There is no pagination: `getlist` returns all the blocks of a page, and `getrepository` returns the whole repository
- **Response format.** The read methods return data, [landing.block.uploadfile](./landing-block-upload-file.md) returns an object with the `id` and `src` fields, and the other modification methods return `true`. The single exception of `updatecontent` is described in the [How to Choose a Method](#how-to-choose) section
- **Error codes.** The most common ones are `ACCESS_DENIED`, `MISSING_PARAMS`, `LANDING_NOT_EXIST`, and `BLOCK_NOT_FOUND`. The exact list for each method is on its own page, and the common REST errors are in the [Error Codes](../../../../error-codes.md) article

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can perform methods: depending on the method

### Retrieving Block Data

#|
|| **Method** | **Description** ||
|| [landing.block.getlist](./landing-block-get-list.md) | Returns the list of blocks on the page ||
|| [landing.block.getbyid](./landing-block-get-by-id.md) | Returns block data by ID ||
|| [landing.block.getcontent](./landing-block-get-content.md) | Returns the content of the block ||
|| [landing.block.getmanifest](./landing-block-get-manifest.md) | Returns the manifest of the placed block ||
|#

### Modifying a Block

#|
|| **Method** | **Description** ||
|| [landing.block.updatenodes](./landing-block-update-nodes.md) | Modifies the content of the block nodes ||
|| [landing.block.updateattrs](./landing-block-update-attrs.md) | Changes the attributes of the block nodes ||
|| [landing.block.updateStyles](./landing-block-update-styles.md) | Modifies the styles of the block ||
|| [landing.block.updatecontent](./landing-block-update-content.md) | Completely replaces the content of the block ||
|| [landing.block.changeNodeName](./landing-block-change-node-name.md) | Changes the node tag ||
|| [landing.block.changeAnchor](./landing-block-change-anchor.md) | Modifies the anchor link of the block ||
|| [landing.block.uploadfile](./landing-block-upload-file.md) | Uploads and associates a file with the block ||
|| [landing.block.publication](./landing-block-publication.md) | Publishes a single block of the page ||
|#

### Block Cards {#cards}

#|
|| **Method** | **Description** ||
|| [landing.block.addcard](./landing-block-add-card.md) | Adds a card to the block ||
|| [landing.block.clonecard](./landing-block-clone-card.md) | Clones a block card ||
|| [landing.block.updateCards](./landing-block-update-cards.md) | Massively updates block cards ||
|| [landing.block.removecard](./landing-block-remove-card.md) | Removes a card from the block ||
|#

### Block Repository

#|
|| **Method** | **Description** ||
|| [landing.block.getrepository](./landing-block-get-repository.md) | Returns the list of templates from the repository ||
|| [landing.block.getmanifestfile](./landing-block-get-manifest-file.md) | Returns the manifest of a template from the repository ||
|| [landing.block.getContentFromRepository](./landing-block-get-content-from-repository.md) | Returns the content of a template before adding it to the page ||
|#
