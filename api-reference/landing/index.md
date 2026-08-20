# Websites and Stores: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods in the `landing` group create and configure websites, stores, pages, and blocks. They manage publication, templates, access permissions, and embedding locations for applications.

The sequence of calls resembles working in a website editor: first, you create or select a site, then a page, add blocks, configure content, and publish the result.

> Quick navigation: [all sections and methods](#all-methods)
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

## Overview of Sections and Methods {#all-methods}

> Scope: [`landing`](../scopes/permissions.md)
>
> Who can execute methods: depends on the method

### Main Objects

#| 
|| **Section** | **Description** ||
|| [Sites](./site/index.md) | Methods for creating, configuring, publishing, and deleting sites, stores, and folders ||
|| [Pages](./page/index.md) | Methods for working with pages, page blocks, and special pages ||
|| [Blocks](./block/index.md) | Methods for changing block content: nodes, cards, attributes, styles, and files ||
|| [View Templates](./template/index.md) | Methods for retrieving templates and configuring included areas of a site or page ||
|#

### Site Extension

#| 
|| **Section** | **Description** ||
|| [Custom Blocks](./user-blocks/index.md) | Methods for registering custom blocks in the repository ||
|| [Custom Templates](./demos/index.md) | Methods for adding templates to the site and page creation wizard ||
|| [Embedding Locations](./embedding/index.md) | Application embedding locations in site settings, page settings, and block actions ||
|| [Knowledge Base Embedding](./embedding/knowledge-base/index.md) | Methods for binding Knowledge Bases to groups and menus ||
|#

### Settings and Access

#| 
|| **Section** | **Description** ||
|| [Working with Site Types and Scopes](./types.md) | Rules for selecting the internal `scope` parameter for sites, stores, Knowledge Bases, the home page, and vibe ||
|| [Permissions](./rights/index.md) | Methods of the extended and role-based permission models for access to sites and stores ||
|#
