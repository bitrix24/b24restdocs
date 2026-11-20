# Websites and Stores: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The methods in the `landing` group create and configure websites, stores, pages, and blocks. They manage publication, templates, access permissions, and embedding locations for applications.

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
|| Get the identifier of an already placed block | [landing.block.getlist](./block/methods/landing-block-get-list.md) with `params.edit_mode = true` ||
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

Embedding locations display an application item in the website interface. [LANDING_SETTINGS](./embedding/settings.md) adds it to the website or page settings menu, and [LANDING_BLOCK_*](./embedding/block.md) adds it to the block action menu. Only an application can register embedding locations.

`LANDING_SETTINGS` is registered using the [landing.repo.bind](./embedding/landing-repo-bind.md) or [placement.bind](../widgets/placement-bind.md) method, and `LANDING_BLOCK_*` only using the [landing.repo.bind](./embedding/landing-repo-bind.md) method. For the `LANDING_BLOCK_*` code, the [placement.bind](../widgets/placement-bind.md) method returns the `ERROR_PLACEMENT_NOT_FOUND` error.

An embedding location is removed the same way it was registered: using the [landing.repo.unbind](./embedding/landing-repo-unbind.md) or [placement.unbind](../widgets/placement-unbind.md) method.

## Types of Websites and Scope

The methods in the `landing` group work with different types of websites: regular websites, stores, service websites, Knowledge Bases, group Knowledge Bases, the main page, and vibes. For some types, you need to pass the `scope` parameter at the root of the request.

This parameter is not related to the access scope [`landing`](../scopes/permissions.md) that you grant to the application or webhook. The rules for selecting the `scope` value and examples of requests are described in the article [Working with Website Types and Scopes](./types.md).

## Key Identifiers

#| 
|| **Identifier** | **What it means** | **Where to get it** ||
|| `SITE_ID`, `siteId`, or `id` | Website identifier, for example, `253` | `result` of the [landing.site.add](./site/landing-site-add.md) method or `result[].ID` of the [landing.site.getList](./site/landing-site-get-list.md) method ||
|| `LID` or `lid` | Page identifier, for example, `2227` | `result` of the [landing.landing.add](./page/methods/landing-landing-add.md), [landing.landing.addByTemplate](./page/methods/landing-landing-add-by-template.md), and [landing.landing.copy](./page/methods/landing-landing-copy.md) methods or `result[].ID` of the [landing.landing.getList](./page/methods/landing-landing-get-list.md) method ||
|| `FOLDER_ID`, `PARENT_ID`, `folderId`, or `id` | Folder identifier, for example, `89` | `result` of the [landing.site.addFolder](./site/landing-site-add-folder.md) method or `result[].ID` of the [landing.site.getFolders](./site/landing-site-get-folders.md) method ||
|| `ID` of the block | Identifier of a block on the page, for example, `28597` | `result` of the [landing.landing.addblock](./page/block-methods/landing-landing-add-block.md) method or `result[].id` of the [landing.block.getlist](./block/methods/landing-block-get-list.md) method with `params.edit_mode = true` if you need a block from the draft ||
|| `CODE` of the block | Code of a block in the repository, for example, `02.three_cols_big_1` or `repo_385` | Block key in the `items` list of the required section in the response of the [landing.block.getrepository](./block/methods/landing-block-get-repository.md) method. A custom block receives the code `repo_<ID>`, where `<ID>` is the `result` of the [landing.repo.register](./user-blocks/landing-repo-register.md) method ||
|#

The website identifier has different names in different methods:

- `id` — in the methods of the [Sites](./site/index.md) object, for example, [landing.site.update](./site/landing-site-update.md).
- `siteId` — in folder methods, for example, [landing.site.addFolder](./site/landing-site-add-folder.md), and in [landing.landing.addByTemplate](./page/methods/landing-landing-add-by-template.md).
- `fields.SITE_ID` — in [landing.landing.add](./page/methods/landing-landing-add.md).
- `params.filter.SITE_ID` — in the filter of [landing.landing.getList](./page/methods/landing-landing-get-list.md).

The exact parameter name is specified on the page of each method.

## Limitations and Features {#restrictions}

- The [landing.landing.addblock](./page/block-methods/landing-landing-add-block.md) method returns the identifier of the created block as a number by default, for example, `28597`. If you pass `fields.RETURN_CONTENT = "Y"`, the method returns the block object, and the identifier is in the `result.id` field.
- The [landing.repo.unbind](./embedding/landing-repo-unbind.md) method also removes an embedding location registered via [placement.bind](../widgets/placement-bind.md), but returns the `PLACEMENT_NO_EXIST` error: it treats as a success only the removal of a location registered via [landing.repo.bind](./embedding/landing-repo-bind.md).
- All methods in the section except [landing.repo.bind](./embedding/landing-repo-bind.md) and [landing.repo.unbind](./embedding/landing-repo-unbind.md) can also be called via a webhook. Via a webhook, only a portal or sites administrator can register and delete custom blocks. The [Authorization in REST](../../settings/how-to-call-rest-api/authorization.md) article helps you choose an authorization method.
- General REST API limits, including request execution time and request intensity, are described in the [REST API Limits](../../limits.md) article.

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
