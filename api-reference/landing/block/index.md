# Blocks Object: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A block is a ready-made fragment of a site page: a cover, a text column, a form, a gallery, or a menu. A page is assembled from blocks, and the `landing.block.*` methods read and modify a block that is already placed — its content, nodes, attributes, styles, and cards. The repository methods are the exception: they work with block templates before placement, and they do not need a page.

For example, on a promotion page you can replace the heading and the image, add product cards, and publish the page without opening the visual editor.

The Blocks object is part of the [Sites and Stores](../index.md) section.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Create and configure your Bitrix24 site](https://helpdesk.bitrix24.com/open/25743741/)

## How to Choose a Section

#|
|| **If You Need To** | **Open the Section** ||
|| Read or modify a block on a page: content, attributes, styles, cards | [Working with Blocks](./methods/index.md) ||
|| Add a block to a page, move it, hide it, or delete it | [Blocks on Page](../page/block-methods/index.md) ||
|| Examine the structure of a block description | [Block Manifest](./manifest.md) ||
|| Understand how the editable elements of a block are described | [Node Types](./node-types.md) ||
|| Add settings to a block that are retained in DOM attributes | [Attributes](./attributes.md) ||
|| Collect cards of different kinds in a single list | [Extended Description of Cards](./extended-description.md) ||
|| Translate the labels of a block into other languages | [Block Localization](./localization.md) ||
|| Configure a menu, a map, search, or a CRM form inside a block | [Special Blocks](./special/index.md) ||
|| Connect a slider, a gallery, or a countdown timer | [Interactive Blocks](./interactive/index.md) ||
|| Add your own block from an app to Bitrix24 | [Custom Blocks](../user-blocks/index.md) ||
|| Work with a block of a knowledge base or of the main page | [The scope Parameter](#scope) ||
|| Investigate a method error | [Section Error Codes](#error-codes), [REST Error Codes](../../../error-codes.md) ||
|#

## Getting Started with a Block

1. Retrieve the list of blocks on the page using the [landing.block.getlist](./methods/landing-block-get-list.md) method with the `params.edit_mode = true` parameter and find the required block by its code or name.
2. Retrieve the block manifest using the [landing.block.getmanifest](./methods/landing-block-get-manifest.md) method with the same `params.edit_mode = true` parameter and check which selectors are described in `nodes`, `cards`, `attrs`, and `style`.
3. Modify the block: node content with the [landing.block.updatenodes](./methods/landing-block-update-nodes.md) method, attributes with [landing.block.updateattrs](./methods/landing-block-update-attrs.md), styles with [landing.block.updateStyles](./methods/landing-block-update-styles.md), and cards with [landing.block.updateCards](./methods/landing-block-update-cards.md).
4. Publish the page using the [landing.landing.publication](../page/methods/landing-landing-publication.md) method so that the changes appear in the published version.

## Block Identifier

The methods that work with a block on a page address it by a pair of values: the page identifier `lid` and the block identifier `block`. The [landing.block.getbyid](./methods/landing-block-get-by-id.md) and [landing.block.uploadfile](./methods/landing-block-upload-file.md) methods and the repository methods do not need a page. The page identifier is returned by [landing.landing.getList](../page/methods/landing-landing-get-list.md), and the block identifier by [landing.block.getlist](./methods/landing-block-get-list.md).

In edit mode, Bitrix24 retains a separate copy of the page blocks, and a draft block has its own identifier. That is why you should request the list of blocks with the `params.edit_mode = true` parameter before modifying anything. A block identifier from the published version does not suit the modification methods: they return the `BLOCK_NOT_FOUND` error.

The read methods enable edit mode differently. For [landing.block.getlist](./methods/landing-block-get-list.md) and [landing.block.getmanifest](./methods/landing-block-get-manifest.md) it is `params.edit_mode = true`, while [landing.block.getcontent](./methods/landing-block-get-content.md) has a separate `editMode = true` parameter. If you are editing a block and want to see the result before publication, pass edit mode in every read call.

## The scope Parameter {#scope}

The `scope` parameter defines the type of site a call works with. It is unrelated to the REST scope `landing`, which grants an app access to the methods of the section.

Blocks of regular sites and online stores are available without additional parameters. If a block is placed on a page of a knowledge base, of a group knowledge base, or on the Bitrix24 main page, every call needs the `scope` parameter with the value matching the site type. The types are listed in the [Site Types](../types.md) article.

The parameter is set separately in every call: the value does not carry over from the previous request and is not inherited by neighbouring commands inside a `batch`. Without `scope`, the method works with regular sites and will not find the block.

## Relationship with Other Objects

**Page.** A block lives on a page: without `lid` it can be neither read nor modified. Block placement is managed by the methods of the [Blocks on Page](../page/block-methods/index.md) section: [landing.landing.addblock](../page/block-methods/landing-landing-add-block.md) adds a block, [landing.landing.upblock](../page/block-methods/landing-landing-up-block.md) and [landing.landing.downblock](../page/block-methods/landing-landing-down-block.md) move it, [landing.landing.hideblock](../page/block-methods/landing-landing-hide-block.md) hides it, and [landing.landing.deleteblock](../page/block-methods/landing-landing-delete-block.md) deletes it.

**Block Repository.** Before being placed on a page, a block exists as a template with a symbolic code, `01.big_with_text` for example. The list of templates is returned by [landing.block.getrepository](./methods/landing-block-get-repository.md), the original manifest by code by [landing.block.getmanifestfile](./methods/landing-block-get-manifest-file.md), and the original HTML by [landing.block.getContentFromRepository](./methods/landing-block-get-content-from-repository.md). The same code is passed to `landing.landing.addblock` when a block is added to a page.

**Manifest.** The manifest describes which elements of a block are editable and which settings are available in the editor. The manifest of a placed block is returned by [landing.block.getmanifest](./methods/landing-block-get-manifest.md), and the original manifest of a template by [landing.block.getmanifestfile](./methods/landing-block-get-manifest-file.md). The structure of the file is described in the [Block Manifest](./manifest.md) article.

**Custom Blocks.** An app can add its own block to Bitrix24 together with a manifest, using the [landing.repo.register](../user-blocks/landing-repo-register.md) method from the [Custom Blocks](../user-blocks/index.md) section. The description of a standard block cannot be changed via REST, see [Block Manifest](./manifest.md) for details.

## How a Block is Structured

A block is not displayed on the page in its original form. During rendering, the system adds a service container `<div id="{anchor}" class="block-wrapper block-{code}">...</div>`.

In the service container:

- **`{anchor}`** — the block's anchor. If the user has not manually changed it, it appears as `block123`, where `123` is the block ID
- **`block-wrapper`** — a common class for all blocks
- **`block-{code}`** — a class that depends on the block's code, where `code` is a safely transformed version of the block's code

## Response Format

What arrives in `result` depends on the method:

- the read methods return data: the list of page blocks, the block object, its content, the manifest, the list of repository templates, and the original content of a template
- an object with the `id` and `src` fields is returned by [landing.block.uploadfile](./methods/landing-block-upload-file.md)
- the other modification methods return `true`. The exception is [landing.block.updatecontent](./methods/landing-block-update-content.md): if the block designer is not available in Bitrix24, a call with the `designed = true` parameter changes nothing and returns `null`. The availability of the designer is determined by the plan, not by a specific block

The exact value is given in the "Response Handling" section on the method page.

## Limits

The section has no pagination. [landing.block.getlist](./methods/landing-block-get-list.md) returns all the blocks of a page in a single response, [landing.block.getrepository](./methods/landing-block-get-repository.md) returns the whole repository, and the other methods work with a single block. The response never contains the `next` and `total` fields.

If a page has many blocks, the `getlist` response grows noticeably with `params.get_content = 1`: the HTML, CSS, and JS of every block are added to it.

## Access Permissions

The permission is checked at the level of the site the page belongs to:

- access to the Sites and Stores section is required for any `landing.*` call. Without it, the method returns `ACCESS_DENIED`
- the methods that read a block on a page additionally require the "view" permission for the site
- the modification methods require the "edit" permission for the site

The specific permission is given in the header of every method page, and the permissions themselves are configured by the methods of the [Access Permissions](../rights/index.md) section.

## Error Codes {#error-codes}

The methods of the section return the error code in the `error` field. The most common ones are:

- `MISSING_PARAMS` — a required parameter was not passed
- `LANDING_NOT_EXIST` — the page with the `lid` identifier was not found or is not available
- `BLOCK_NOT_FOUND` — the block was not found in the selected version of the page. The repository methods do not have this error: they do not work with a page
- `ACCESS_DENIED` — the user does not have the required permission
- `TYPE_ERROR` — a parameter was passed in an unsuitable format
- `SYSTEM_ERROR` — an internal error while executing the method

Individual methods have their own codes: `CARD_NOT_FOUND` for [landing.block.addcard](./methods/landing-block-add-card.md), [landing.block.clonecard](./methods/landing-block-clone-card.md), and [landing.block.removecard](./methods/landing-block-remove-card.md); `NODES_NOT_FOUND` for [landing.block.updatenodes](./methods/landing-block-update-nodes.md) and [landing.block.changeNodeName](./methods/landing-block-change-node-name.md); `INCORRECT_AFFECTED` for [landing.block.updatenodes](./methods/landing-block-update-nodes.md) and only when strict result checking is enabled in Bitrix24; `BAD_ANCHOR` for [landing.block.changeAnchor](./methods/landing-block-change-anchor.md); `FILE_ERROR` for [landing.block.uploadfile](./methods/landing-block-upload-file.md).

The full list, including the method-specific codes, is given on the method page. The common REST errors are described in the [Error Codes](../../../error-codes.md) article.

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

### Reading Block Data

#|
|| **Method** | **Description** ||
|| [landing.block.getlist](./methods/landing-block-get-list.md) | Retrieves the list of blocks on the page ||
|| [landing.block.getbyid](./methods/landing-block-get-by-id.md) | Retrieves a block by its identifier ||
|| [landing.block.getcontent](./methods/landing-block-get-content.md) | Retrieves the content of a block ||
|| [landing.block.getmanifest](./methods/landing-block-get-manifest.md) | Retrieves the manifest of a block already placed on the page ||
|#

### Modifying a Block

#|
|| **Method** | **Description** ||
|| [landing.block.updatenodes](./methods/landing-block-update-nodes.md) | Modifies the content of the block's nodes ||
|| [landing.block.updateattrs](./methods/landing-block-update-attrs.md) | Modifies the attributes of the block's nodes ||
|| [landing.block.updateStyles](./methods/landing-block-update-styles.md) | Modifies the styles of the block ||
|| [landing.block.updatecontent](./methods/landing-block-update-content.md) | Completely replaces the content of the block ||
|| [landing.block.changeNodeName](./methods/landing-block-change-node-name.md) | Changes the tag name of a node ||
|| [landing.block.changeAnchor](./methods/landing-block-change-anchor.md) | Changes the symbolic code of the block's anchor ||
|| [landing.block.uploadfile](./methods/landing-block-upload-file.md) | Uploads a file and associates it with the block ||
|| [landing.block.publication](./methods/landing-block-publication.md) | Publishes a single block of the page ||
|#

### Block Cards

#|
|| **Method** | **Description** ||
|| [landing.block.addcard](./methods/landing-block-add-card.md) | Adds a block card with modified content ||
|| [landing.block.clonecard](./methods/landing-block-clone-card.md) | Clones a block card ||
|| [landing.block.updateCards](./methods/landing-block-update-cards.md) | Massively modifies block cards ||
|| [landing.block.removecard](./methods/landing-block-remove-card.md) | Removes a block card ||
|#

### Block Repository

#|
|| **Method** | **Description** ||
|| [landing.block.getrepository](./methods/landing-block-get-repository.md) | Retrieves the list of block templates from the repository ||
|| [landing.block.getmanifestfile](./methods/landing-block-get-manifest-file.md) | Retrieves the manifest of a block template from the repository ||
|| [landing.block.getContentFromRepository](./methods/landing-block-get-content-from-repository.md) | Retrieves the content of a block template before it is added to the page ||
|#
