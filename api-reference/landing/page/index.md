# Pages: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The methods of the Page object create site pages and folders, change page parameters, manage its blocks, publish the page, and assign it a special role on the site.

For example, you can create a promotional page, fill it with blocks, and publish it. Once the promotion ends, you can unpublish the page, move it to another folder, or delete it.

The Page object is part of the [Websites and Stores](../index.md) section.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Create and configure your Bitrix24 site](https://helpdesk.bitrix24.com/open/25743741/)

## How to Choose a Section

#|
|| **If You Need To** | **Open the Section** ||
|| Create a page, change its parameters, publish, move, or delete it | [Working with Pages](./methods/index.md) ||
|| Place a block on a page, hide, move, or delete it | [Working with Page Blocks](./block-methods/index.md) ||
|| Change the text, images, and settings of a block that is already placed | [Blocks](../block/index.md) ||
|| Make a page the main page or a service page of the site | [Special Website Pages](./special-pages/index.md) ||
|| See the set of page fields | [Page Fields](./fields.md) ||
|| Find a value for `ADDITIONAL_FIELDS` or `THEME_CODE` | [Additional Page Fields](./additional-fields.md), [Page Color Themes](./color-themes.md) ||
|| Understand which `scope` to pass and what site types exist | [Working with Site Types and Scopes](../types.md) ||
|| Look into a method error | [Error Codes](../../../error-codes.md) ||
|#

## How to Work with the Page

1. Retrieve the site identifier using the [landing.site.getList](../site/landing-site-get-list.md) method, or create a new site using the [landing.site.add](../site/landing-site-add.md) method. All the methods of the Site object are collected in the [Sites](../site/index.md) section
2. If the page has to be placed in a folder, retrieve the folder identifier using the [landing.site.getFolders](../site/landing-site-get-folders.md) method, or create a folder using the [landing.site.addFolder](../site/landing-site-add-folder.md) method
3. Create the page using one of the three methods. [landing.landing.add](./methods/landing-landing-add.md) adds an empty page. [landing.landing.addByTemplate](./methods/landing-landing-add-by-template.md) creates a page from a ready-made template of the wizard, and the template code is provided by the [landing.demos.getPageList](../demos/landing-demos-get-page-list.md) method. [landing.landing.copy](./methods/landing-landing-copy.md) copies an existing page. The response contains the page identifier `lid`
4. Fill the page with blocks using the methods of the [Working with Page Blocks](./block-methods/index.md) section
5. Configure the page parameters using the [landing.landing.update](./methods/landing-landing-update.md) method, or move the page to another folder or another site using the [landing.landing.move](./methods/landing-landing-move.md) method
6. Publish the page using the [landing.landing.publication](./methods/landing-landing-publication.md) method. To unpublish the page, use [landing.landing.unpublic](./methods/landing-landing-unpublic.md)
7. Send a page you no longer need to the recycle bin using the [landing.landing.markDelete](./methods/landing-landing-mark-delete.md) method, and restore it if needed using the [landing.landing.markUnDelete](./methods/landing-landing-mark-undelete.md) method. The [landing.landing.delete](./methods/landing-landing-delete.md) method deletes the page permanently and only works with a page that is not in the recycle bin

Page and block changes are retained in the draft. They appear in the public version only after `landing.landing.publication` is called, so publish the page after every edit.

## Page Identifier

The `lid` identifier is required by the methods that work with a specific page and its blocks. To find the `lid` of an existing page, use the [landing.landing.getList](./methods/landing-landing-get-list.md) and [landing.landing.resolveIdByPublicUrl](./methods/landing-landing-resolve-id-by-public-url.md) methods.

## Response Format

What comes in `result` depends on the method:

- the object identifier is returned by the methods that create and copy — `add`, `addByTemplate`, `copy`, `addblock`, `copyblock`, `moveblock`, `favoriteBlock` — as well as by `markDelete` and `markUnDelete`. The exception is the block methods with the `RETURN_CONTENT` flag: `landing.landing.addblock` returns the block data, while `landing.landing.copyblock` and `landing.landing.moveblock` return an object with a success flag and the block data. Where to pass the flag is specified on the method page
- data is returned by the read methods: a list of pages, the public address of a page, the address of the preview image, the page identifier by public URL, or a set of fields
- the other methods that update, publish, and delete return `true`

The exact value is specified in the Returned Data section on the method page. The page object comes only from [landing.landing.getList](./methods/landing-landing-get-list.md), and the composition of its fields is described in the article [Page Fields](./fields.md).

## Limits

The number of published pages is limited by the Bitrix24 plan. The limit is checked at publication: if it is exhausted, the [landing.landing.publication](./methods/landing-landing-publication.md) method returns the `PUBLIC_PAGE_REACHED` error. The site limit errors are listed on the page of this method.

## When to Pass Scope

The `scope` parameter indicates the type of site or page in which the method should operate. This is an internal parameter of sites and pages. It is not related to the REST scope `landing` in the method name.

The `scope` parameter is required if the page belongs to a non-public site type: a knowledge base, a group knowledge base, or the Bitrix24 main page, which is also called vibe. The `scope` value matches the site type code, and the full list of codes is provided in the article [Working with Site Types and Scopes](../types.md). Without `scope`, such a page is not found even though it exists. A typical sign of a missing `scope` is an empty result from the search methods and the `LANDING_NOT_EXIST` error from the methods that work with a specific page.

Pass the parameter at the top level, next to the method parameters: it applies to the entire call. In a batch request, `scope` is specified in every command — it is not inherited from a neighboring command. `scope` is required in any method that addresses such a page: in search, when updating, moving, or deleting it, and in the methods for working with blocks.

## Relationships with Other Objects

A page is linked to a site and a folder, a view template, blocks, special pages, and access rights.

**Site and Folder.** A page always belongs to a site and can be placed in its folder. The `SITE_ID` and `FOLDER_ID` identifiers are retrieved at the first step of working with the page, and the [landing.landing.move](./methods/landing-landing-move.md) method moves the page to another folder or another site.

**View Template.** The page design is set by `TPL_ID` — the identifier of a view template from the [Object View Template](../template/index.md) section. Do not confuse it with the page template code, which is passed to `landing.landing.addByTemplate` in the `code` parameter.

**Block.** The content of the page consists of blocks. A block lives on a page and is addressed by the pair "page identifier and block identifier". The block identifier is returned by the methods of the [Working with Page Blocks](./block-methods/index.md) section and by the [landing.block.getlist](../block/methods/landing-block-get-list.md) method — for draft blocks, it is called with `params.edit_mode = 1`.

**Special Page.** A regular page can be assigned as a service page of the site, for example, as the shopping cart or the checkout page. The binding is created by the [landing.syspage.set](./special-pages/landing-syspage-set.md) method: it takes the site identifier, the special page type, and the identifier of the regular page.

**Access Rights.** The methods of the section check rights twice. First comes the user's general access to the Websites and Stores section: without it, the method returns `ACCESS_DENIED` regardless of the rights to a specific site. This check is skipped only when working in the main page scope. Then the right to the site itself is checked: view, edit, publish, change settings, or delete — depending on the method. The specific right is stated in the header of each method page, and rights are configured using the methods of the [Access Rights](../rights/index.md) section.

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

### Working with Pages

#| 
|| **Method** | **Description** ||
|| [landing.landing.add](./methods/landing-landing-add.md) | Adds a page or a folder ||
|| [landing.landing.addByTemplate](./methods/landing-landing-add-by-template.md) | Creates a page from a template ||
|| [landing.landing.copy](./methods/landing-landing-copy.md) | Copies a page ||
|| [landing.landing.update](./methods/landing-landing-update.md) | Updates page parameters ||
|| [landing.landing.move](./methods/landing-landing-move.md) | Moves a page to another site or folder ||
|| [landing.landing.getList](./methods/landing-landing-get-list.md) | Retrieves a list of pages ||
|| [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md) | Retrieves additional fields of the page ||
|| [landing.landing.getpreview](./methods/landing-landing-get-preview.md) | Returns the URL of the page preview ||
|| [landing.landing.getpublicurl](./methods/landing-landing-get-public-url.md) | Returns the public URL of the page ||
|| [landing.landing.resolveIdByPublicUrl](./methods/landing-landing-resolve-id-by-public-url.md) | Returns the page ID by public URL ||
|| [landing.landing.publication](./methods/landing-landing-publication.md) | Publishes the page ||
|| [landing.landing.unpublic](./methods/landing-landing-unpublic.md) | Unpublishes the page ||
|| [landing.landing.markDelete](./methods/landing-landing-mark-delete.md) | Marks the page as deleted ||
|| [landing.landing.markUnDelete](./methods/landing-landing-mark-undelete.md) | Restores the page from the recycle bin ||
|| [landing.landing.removeEntities](./methods/landing-landing-remove-entities.md) | Deletes the blocks of the page and unlinks image files from it ||
|| [landing.landing.delete](./methods/landing-landing-delete.md) | Deletes the page ||
|#

### Working with Blocks

#| 
|| **Method** | **Description** ||
|| [landing.landing.addblock](./block-methods/landing-landing-add-block.md) | Adds a new block to the page ||
|| [landing.landing.copyblock](./block-methods/landing-landing-copy-block.md) | Copies a block to the page ||
|| [landing.landing.moveblock](./block-methods/landing-landing-move-block.md) | Moves a block from one page to another ||
|| [landing.landing.upblock](./block-methods/landing-landing-up-block.md) | Moves a block up one position ||
|| [landing.landing.downblock](./block-methods/landing-landing-down-block.md) | Moves a block down one position ||
|| [landing.landing.showblock](./block-methods/landing-landing-show-block.md) | Displays a block on the page ||
|| [landing.landing.hideblock](./block-methods/landing-landing-hide-block.md) | Hides a block on the page ||
|| [landing.landing.favoriteBlock](./block-methods/landing-landing-favorite-block.md) | Saves a block to "My Blocks" ||
|| [landing.landing.unFavoriteBlock](./block-methods/landing-landing-unfavorite-block.md) | Removes a block from "My Blocks" ||
|| [landing.landing.markdeletedblock](./block-methods/landing-landing-mark-deleted-block.md) | Marks a block as deleted without physically removing it ||
|| [landing.landing.markundeletedblock](./block-methods/landing-landing-mark-undeleted-block.md) | Restores a block from deleted ||
|| [landing.landing.deleteblock](./block-methods/landing-landing-delete-block.md) | Deletes a block from the page ||
|#

### Special Pages

#| 
|| **Method** | **Description** ||
|| [landing.syspage.set](./special-pages/landing-syspage-set.md) | Assigns a special page for the site ||
|| [landing.syspage.get](./special-pages/landing-syspage-get.md) | Retrieves a list of special pages of the site ||
|| [landing.syspage.getSpecialPage](./special-pages/landing-syspage-get-special-page.md) | Retrieves the URL of the special page of the site ||
|| [landing.syspage.deleteForLanding](./special-pages/landing-syspage-delete-for-landing.md) | Removes all bindings of the page as a special one ||
|| [landing.syspage.deleteForSite](./special-pages/landing-syspage-delete-for-site.md) | Removes all bindings of the site's special pages ||
|#
