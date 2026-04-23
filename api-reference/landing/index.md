# Websites and Stores: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods in the `landing` group create and configure websites, stores, pages, and blocks. They manage publication, templates, access permissions, and embedding locations for applications.

The sequence of calls resembles working in a website editor: first, you create or select a site, then a page, add blocks, configure content, and publish the result.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Create and configure your Bitrix24 site](https://helpdesk.bitrix24.com/open/25743741/)

## Structure of Websites and Stores

A website combines pages, folders, settings, access permissions, and publication. The type of website determines which pages, blocks, and settings are available in the `landing` methods.

A page belongs to a website and consists of blocks. Page methods create, copy, move, and publish the page, while block methods on the page add, hide, move, or delete blocks.

A block is a part of a page with an HTML structure, nodes, cards, attributes, and styles. Nodes store editable content, cards describe repeating elements of the block, and attributes and styles handle markup and design.

The block manifest shows which nodes, cards, attributes, and styles are available for modification via the API.

Changes to pages and blocks are saved in a draft. The publication method transfers changes to the public version of the page or website.

{% note tip "User Documentation" %}

- [Create a multi-page website](https://helpdesk.bitrix24.com/open/25558484/)
- [Site and page settings](https://helpdesk.bitrix24.com/open/21883080/)
- [Site access permissions](https://helpdesk.bitrix24.com/open/22057418/)

{% endnote %}

## Getting Started

1. Create a website using the [landing.site.add](./site/landing-site-add.md) method or retrieve an existing website using the [landing.site.getList](./site/landing-site-get-list.md) method.
2. Create a page using the [landing.landing.add](./page/methods/landing-landing-add.md) method, select an existing one using the [landing.landing.getList](./page/methods/landing-landing-get-list.md) method, or create a page from a template using the [landing.landing.addByTemplate](./page/methods/landing-landing-add-by-template.md) method.
3. Add a block to the page using the [landing.landing.addblock](./page/block-methods/landing-landing-add-block.md) method.
4. Modify the block content using the [landing.block.*](./block/index.md) methods.
5. Publish the page using the [landing.landing.publication](./page/methods/landing-landing-publication.md) method or the entire website using the [landing.site.publication](./site/landing-site-publication.md) method.

## Choosing Methods for Working with Blocks

There are two groups of methods for page blocks. The methods for [working with blocks on the page](./page/block-methods/index.md) manage the placement of blocks on a specific page: adding, copying, moving, hiding, deleting, and saving blocks to "My Blocks." The methods for the [Block object](./block/index.md) modify the content of already placed blocks: nodes, cards, attributes, styles, content, and files.

#| 
|| **Task** | **What to use** ||
|| Add a block from the repository to the page | [landing.landing.addblock](./page/block-methods/landing-landing-add-block.md) ||
|| Change the order, visibility, or state of a block on the page | Methods for [working with blocks on the page](./page/block-methods/index.md) ||
|| Change text, images, links, cards, or styles of a block | Methods [landing.block.*](./block/index.md) ||
|| Get the code of a standard or custom block before adding | [landing.block.getrepository](./block/methods/landing-block-get-repository.md) ||
|| Get the identifier of an already placed block | [landing.block.getList](./block/methods/landing-block-get-list.md) with `params.edit_mode = true` ||
|#

## Additional Scenarios

#| 
|| **Task** | **What to use** ||
|| Configure repeating parts of pages | [View templates](./template/index.md) and methods [landing.template.*](./template/index.md) ||
|| Add your blocks to the editor | [Custom blocks](./user-blocks/index.md) and methods [landing.repo.*](./user-blocks/index.md) ||
|| Add your templates to the website and page creation wizard | [Custom templates](./demos/index.md) and methods [landing.demos.*](./demos/index.md) ||
|| Configure access to websites and stores | [Permissions: extended or role-based model](./rights/index.md) ||
|| Embed an application into the interface of websites and pages | [Embedding locations](./embedding/index.md) ||
|#

## Embedding Locations

Embedding locations allow you to add an application to the interface of websites and pages. The application can be opened from the website or page settings and from block editing actions.

**Website or page settings.** The embedding location [LANDING_SETTINGS](./embedding/settings.md) adds an application item to the settings menu of the website or page. The identifiers of the website and page are passed to the handler in `PLACEMENT_OPTIONS`.

**Block actions.** The embedding location [LANDING_BLOCK_*](./embedding/block.md) adds an application item to the block action menu. The block identifier, block code, and page identifier are passed to the handler.

**Knowledge Base.** The binding of the Knowledge Base to a menu or group is described in the subsection [Embedding the Knowledge Base](./embedding/knowledge-base/index.md). These bindings are managed by the `landing.site.*` methods because the Knowledge Base is presented as a separate website.

In the `landing` module, embedding locations are registered using the internal method `landing.repo.bind`, not [placement.bind](../widgets/placement-bind.md). You can remove the embedding location of the current application using the [landing.repo.unbind](./embedding/landing-repo-unbind.md) method.

## Types of Websites and Scope

The methods in the `landing` group work with different types of websites: regular websites, stores, service websites, Knowledge Bases, group Knowledge Bases, the main page, and vibes. For some types, you need to pass the `scope` parameter at the root of the request.

This parameter is not related to the access scope [`landing`](../scopes/permissions.md) that you grant to the application or webhook. The rules for selecting the `scope` value and examples of requests are described in the article [Working with Website Types and Scopes](./types.md).

## Key Identifiers

#| 
|| **Identifier** | **Where used** | **How to obtain** ||
|| `SITE_ID` or `siteId` | Page, folder, rights, template methods, website publication, and Knowledge Base bindings | From the result of [landing.site.add](./site/landing-site-add.md) or [landing.site.getList](./site/landing-site-get-list.md) ||
|| `LID` or `lid` | Page, block, page publication methods, and embedding location `LANDING_SETTINGS` | From the result of [landing.landing.add](./page/methods/landing-landing-add.md), [landing.landing.addByTemplate](./page/methods/landing-landing-add-by-template.md), [landing.landing.copy](./page/methods/landing-landing-copy.md), or [landing.landing.getList](./page/methods/landing-landing-get-list.md) ||
|| `FOLDER_ID`, `PARENT_ID` or `ID` of the folder | Placing pages in folders and managing website structure | From the result of [landing.site.addFolder](./site/landing-site-add-folder.md) or [landing.site.getFolders](./site/landing-site-get-folders.md) ||
|| `ID` of the block | Modifying, moving, copying, hiding, and deleting a block on the page | From the result of [landing.landing.addblock](./page/block-methods/landing-landing-add-block.md) or [landing.block.getlist](./block/methods/landing-block-get-list.md) with `params.edit_mode = true`, if you need a block from the draft ||
|| `CODE` of the block | Adding a standard or custom block to the page | From [landing.block.getrepository](./block/methods/landing-block-get-repository.md) or after registering the block using the [landing.repo.register](./user-blocks/landing-repo-register.md) method ||
|#

## Overview of Methods {#all-methods}

> Scope: [`landing`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Custom Blocks

#| 
|| **Method** | **Description** ||
|| [landing.repo.register](./user-blocks/landing-repo-register.md) | Registers a custom block in the repository ||
|| [landing.repo.checkContent](./user-blocks/landing-repo-check-content.md) | Checks the block content through a sanitizer ||
|| [landing.repo.getList](./user-blocks/landing-repo-get-list.md) | Retrieves a list of custom blocks from the repository ||
|| [landing.repo.unregister](./user-blocks/landing-repo-unregister.md) | Removes a custom block from the repository by its code ||
|#

### View Templates

#| 
|| **Method** | **Description** ||
|| [landing.template.getlist](./template/landing-template-get-list.md) | Retrieves a list of view templates ||
|| [landing.template.getSiteRef](./template/landing-template-get-site-ref.md) | Retrieves a list of included areas for the website ||
|| [landing.template.setSiteRef](./template/landing-template-set-site-ref.md) | Sets included areas for the website ||
|| [landing.template.getLandingRef](./template/landing-template-get-landing-ref.md) | Retrieves a list of included areas for the page ||
|| [landing.template.setLandingRef](./template/landing-template-set-landing-ref.md) | Sets included areas for the page ||
|#

### Custom Templates

#| 
|| **Method** | **Description** ||
|| [landing.demos.register](./demos/landing-demos-register.md) | Registers a template in the website and page creation wizard ||
|| [landing.demos.getList](./demos/landing-demos-get-list.md) | Retrieves a list of registered templates ||
|| [landing.demos.getSiteList](./demos/landing-demos-get-site-list.md) | Retrieves a list of templates for creating websites ||
|| [landing.demos.getPageList](./demos/landing-demos-get-page-list.md) | Retrieves a list of templates for creating pages ||
|| [landing.demos.unregister](./demos/landing-demos-unregister.md) | Removes a registered custom template ||
|#

### Websites

#| 
|| **Method** | **Description** ||
|| [landing.site.add](./site/landing-site-add.md) | Adds a website ||
|| [landing.site.addFolder](./site/landing-site-add-folder.md) | Adds a folder to the website ||
|| [landing.site.update](./site/landing-site-update.md) | Updates website parameters ||
|| [landing.site.updateFolder](./site/landing-site-update-folder.md) | Updates folder parameters ||
|| [landing.site.getList](./site/landing-site-get-list.md) | Retrieves a list of websites ||
|| [landing.site.getFolders](./site/landing-site-get-folders.md) | Retrieves website folders ||
|| [landing.site.getPreview](./site/landing-site-get-preview.md) | Returns the preview URL of the website ||
|| [landing.site.getPublicUrl](./site/landing-site-get-public-url.md) | Returns the public URL of the website ||
|| [landing.site.getadditionalfields](./site/landing-site-get-additional-fields.md) | Retrieves additional fields of the website ||
|| [landing.site.publication](./site/landing-site-publication.md) | Publishes the website and all its pages ||
|| [landing.site.publicationFolder](./site/landing-site-publication-folder.md) | Publishes the website folder ||
|| [landing.site.unpublic](./site/landing-site-unpublic.md) | Unpublishes the website and all its pages ||
|| [landing.site.unPublicFolder](./site/landing-site-unpublic-folder.md) | Unpublishes the website folder ||
|| [landing.site.markDelete](./site/landing-site-mark-delete.md) | Marks the website as deleted ||
|| [landing.site.markFolderDelete](./site/landing-site-mark-folder-delete.md) | Marks the folder as deleted ||
|| [landing.site.markFolderUnDelete](./site/landing-site-mark-folder-undelete.md) | Restores the folder from the trash ||
|| [landing.site.markUnDelete](./site/landing-site-mark-undelete.md) | Restores the website from the trash ||
|| [landing.site.delete](./site/landing-site-delete.md) | Deletes the website ||
|| [landing.site.fullExport](./site/landing-site-full-export.md) | Exports the website and its pages to an array ||
|#

### Permissions

#| 
|| **Method** | **Description** ||
|| [landing.role.enable](./rights/landing-role-enable.md) | Enables or disables the role-based permission model ||
|| [landing.role.isEnabled](./rights/landing-role-is-enabled.md) | Checks if the role-based permission model is enabled ||
|#

#### Extended Permission Model

#| 
|| **Method** | **Description** ||
|| [landing.site.getRights](./rights/extended-model/landing-site-get-rights.md) | Retrieves the current user's permissions for the website ||
|| [landing.site.setRights](./rights/extended-model/landing-site-set-rights.md) | Sets access permissions for the website ||
|#

#### Role-Based Permission Model

#| 
|| **Method** | **Description** ||
|| [landing.role.getList](./rights/role-model/landing-role-get-list.md) | Retrieves a list of roles for the current type of websites ||
|| [landing.role.getRights](./rights/role-model/landing-role-get-rights.md) | Returns the permissions of the role by websites ||
|| [landing.role.setAccessCodes](./rights/role-model/landing-role-set-access-codes.md) | Sets access codes for the role ||
|| [landing.role.setRights](./rights/role-model/landing-role-set-rights.md) | Sets the permissions of the role by websites ||
|#

### Pages

#| 
|| **Method** | **Description** ||
|| [landing.landing.add](./page/methods/landing-landing-add.md) | Adds a page ||
|| [landing.landing.addByTemplate](./page/methods/landing-landing-add-by-template.md) | Creates a page from a template ||
|| [landing.landing.copy](./page/methods/landing-landing-copy.md) | Copies a page ||
|| [landing.landing.update](./page/methods/landing-landing-update.md) | Modifies page parameters ||
|| [landing.landing.move](./page/methods/landing-landing-move.md) | Moves a page to another website or folder ||
|| [landing.landing.getList](./page/methods/landing-landing-get-list.md) | Retrieves a list of pages ||
|| [landing.landing.getadditionalfields](./page/methods/landing-landing-get-additional-fields.md) | Retrieves additional fields of the page ||
|| [landing.landing.getpreview](./page/methods/landing-landing-get-preview.md) | Returns the path to the page preview ||
|| [landing.landing.getpublicurl](./page/methods/landing-landing-get-public-url.md) | Returns the public URL of the page ||
|| [landing.landing.resolveIdByPublicUrl](./page/methods/landing-landing-resolve-id-by-public-url.md) | Returns the identifier of the page by public URL ||
|| [landing.landing.publication](./page/methods/landing-landing-publication.md) | Publishes the page ||
|| [landing.landing.unpublic](./page/methods/landing-landing-unpublic.md) | Unpublishes the page ||
|| [landing.landing.markDelete](./page/methods/landing-landing-mark-delete.md) | Marks the page as deleted ||
|| [landing.landing.markUnDelete](./page/methods/landing-landing-mark-undelete.md) | Restores the page from deleted ||
|| [landing.landing.removeEntities](./page/methods/landing-landing-remove-entities.md) | Removes blocks and images from the page ||
|| [landing.landing.delete](./page/methods/landing-landing-delete.md) | Deletes the page ||
|#

#### Working with Blocks on the Page

#| 
|| **Method** | **Description** ||
|| [landing.landing.addblock](./page/block-methods/landing-landing-add-block.md) | Adds a new block to the page ||
|| [landing.landing.copyblock](./page/block-methods/landing-landing-copy-block.md) | Copies a block from one page to another ||
|| [landing.landing.deleteblock](./page/block-methods/landing-landing-delete-block.md) | Deletes a block from the page ||
|| [landing.landing.downblock](./page/block-methods/landing-landing-down-block.md) | Moves the block down one position ||
|| [landing.landing.favoriteBlock](./page/block-methods/landing-landing-favorite-block.md) | Saves the block to "My Blocks" ||
|| [landing.landing.hideblock](./page/block-methods/landing-landing-hide-block.md) | Hides the block on the page ||
|| [landing.landing.markdeletedblock](./page/block-methods/landing-landing-mark-deleted-block.md) | Marks the block as deleted without physically removing it ||
|| [landing.landing.markundeletedblock](./page/block-methods/landing-landing-mark-undeleted-block.md) | Restores the block from deleted ||
|| [landing.landing.moveblock](./page/block-methods/landing-landing-move-block.md) | Moves the block from one page to another ||
|| [landing.landing.showblock](./page/block-methods/landing-landing-show-block.md) | Shows the block on the page ||
|| [landing.landing.unFavoriteBlock](./page/block-methods/landing-landing-unfavorite-block.md) | Removes the block from "My Blocks" ||
|| [landing.landing.upblock](./page/block-methods/landing-landing-up-block.md) | Moves the block up one position ||
|#

#### Special Pages

#| 
|| **Method** | **Description** ||
|| [landing.syspage.deleteForLanding](./page/special-pages/landing-syspage-delete-for-landing.md) | Deletes the page binding as special ||
|| [landing.syspage.deleteForSite](./page/special-pages/landing-syspage-delete-for-site.md) | Deletes all special pages of the website ||
|| [landing.syspage.getSpecialPage](./page/special-pages/landing-syspage-get-special-page.md) | Retrieves the address of the special page of the website ||
|| [landing.syspage.get](./page/special-pages/landing-syspage-get.md) | Retrieves a list of special pages ||
|| [landing.syspage.set](./page/special-pages/landing-syspage-set.md) | Assigns a special page for the website ||
|#

### Blocks

#| 
|| **Method** | **Description** ||
|| [landing.block.getlist](./block/methods/landing-block-get-list.md) | Retrieves a list of blocks on the page ||
|| [landing.block.getbyid](./block/methods/landing-block-get-by-id.md) | Retrieves a block by its identifier ||
|| [landing.block.getcontent](./block/methods/landing-block-get-content.md) | Retrieves the content of the block ||
|| [landing.block.getmanifest](./block/methods/landing-block-get-manifest.md) | Retrieves the manifest of the block already placed on the page ||
|| [landing.block.updatenodes](./block/methods/landing-block-update-nodes.md) | Modifies the content of the block's nodes ||
|| [landing.block.updateattrs](./block/methods/landing-block-update-attrs.md) | Modifies the attributes of the block's nodes ||
|| [landing.block.updateStyles](./block/methods/landing-block-update-styles.md) | Modifies the styles of the block ||
|| [landing.block.updatecontent](./block/methods/landing-block-update-content.md) | Updates the content of the block placed on the page with arbitrary content ||
|| [landing.block.changeNodeName](./block/methods/landing-block-change-node-name.md) | Changes the tag name of the node ||
|| [landing.block.changeAnchor](./block/methods/landing-block-change-anchor.md) | Changes the symbolic code of the block's anchor ||
|| [landing.block.uploadfile](./block/methods/landing-block-upload-file.md) | Uploads a file and binds it to the block ||
|| [landing.block.clonecard](./block/methods/landing-block-clone-card.md) | Clones a card of the block ||
|| [landing.block.addcard](./block/methods/landing-block-add-card.md) | Adds a card to the block with modified content ||
|| [landing.block.removecard](./block/methods/landing-block-remove-card.md) | Removes a card from the block ||
|| [landing.block.updateCards](./block/methods/landing-block-update-cards.md) | Massively modifies the cards of the block ||
|| [landing.block.getrepository](./block/methods/landing-block-get-repository.md) | Retrieves a list of blocks from the repository ||
|| [landing.block.getmanifestfile](./block/methods/landing-block-get-manifest-file.md) | Retrieves the manifest of the block from the repository ||
|| [landing.block.getContentFromRepository](./block/methods/landing-block-get-content-from-repository.md) | Retrieves the content of the block from the repository before adding it to the page ||
|#

### Embedding Locations

#| 
|| **Method or Embedding Location** | **Description** ||
|| [LANDING_SETTINGS](./embedding/settings.md) | Adds an application item to the settings menu of the website or page ||
|| [LANDING_BLOCK_*](./embedding/block.md) | Adds an application item to the block editing actions ||
|| [landing.repo.unbind](./embedding/landing-repo-unbind.md) | Removes the embedding location of the current application ||
|#

#### Embedding the Knowledge Base

#| 
|| **Method** | **Description** ||
|| [landing.site.bindingToGroup](./embedding/knowledge-base/landing-site-binding-to-group.md) | Binds the Knowledge Base to a Social Network group ||
|| [landing.site.bindingToMenu](./embedding/knowledge-base/landing-site-binding-to-menu.md) | Binds the Knowledge Base to a menu ||
|| [landing.site.getGroupBindings](./embedding/knowledge-base/landing-site-get-group-bindings.md) | Retrieves the bindings of Knowledge Bases to groups ||
|| [landing.site.getMenuBindings](./embedding/knowledge-base/landing-site-get-menu-bindings.md) | Retrieves the bindings of Knowledge Bases to menus ||
|| [landing.site.unbindingFromGroup](./embedding/knowledge-base/landing-site-unbinding-from-group.md) | Unbinds the Knowledge Base from a Social Network group ||
|| [landing.site.unbindingFromMenu](./embedding/knowledge-base/landing-site-unbinding-from-menu.md) | Unbinds the Knowledge Base from a menu ||
|#