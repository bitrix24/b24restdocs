# Object Page: Overview of Methods

Methods help manage the page and its associated actions. With these methods, you can:

- modify the page,
- manage its blocks,
- assign a special role to the page on the site.

For example, you can create a promotional page, fill it with blocks, and publish it. Once the promotion ends, you can unpublish the page, move it to another folder, or delete it.

The fields of the page are described in a separate article [Page Fields](./fields.md).

> Quick navigation: [all methods](#all-methods)

## How to Work with the Page

Working with the page starts with the site. First, obtain the site ID using the method [landing.site.getList](../site/landing-site-get-list.md). If the page needs to be placed in a folder, additionally get the folder ID using the method [landing.site.getFolders](../site/landing-site-get-folders.md).

After that, choose a method to create the page:

- [landing.landing.add](./methods/landing-landing-add.md) if you need a new page,
- [landing.landing.addByTemplate](./methods/landing-landing-add-by-template.md) if you need a page with a predefined structure,
- [landing.landing.copy](./methods/landing-landing-copy.md) if you want to base it on an existing page.

Once the page is created, you can configure its parameters using the methods [landing.landing.update](./methods/landing-landing-update.md) and [landing.landing.move](./methods/landing-landing-move.md).

The completed page can be published or hidden. To publish, use [landing.landing.publication](./methods/landing-landing-publication.md), and to unpublish the page, use [landing.landing.unpublic](./methods/landing-landing-unpublic.md).

## When to Pass Scope

The `scope` parameter indicates the type of site or page in which the method should operate. This is an internal parameter for landing pages and is not related to the REST scope `landing` in the method name.

If `scope` is not specified, the method will only work in the standard context. For non-public types, such as `knowledge`, `group`, and `vibe`, this is insufficient. In such cases, the site or page may not be found, even though they exist.

The `scope` parameter is necessary in methods that search for a page, change its visibility, or work with content. For example, in [landing.landing.getList](./methods/landing-landing-get-list.md), [landing.landing.publication](./methods/landing-landing-publication.md), [landing.landing.unpublic](./methods/landing-landing-unpublic.md), and in the methods of the section [Working with Page Blocks](./block-methods/index.md).

For instance, if the page belongs to the knowledge base, the call should include `scope=knowledge`. This way, the method will search for and modify the page within the knowledge base. Without this parameter, the page may not be found. The rules for selecting values and their relation to site types are described in the article [Working with Site Types and Scopes](../types.md).

## Connection with Other Objects

A page in Bitrix24 is always connected with other objects. The site sets the overall context, blocks provide content, and special pages define the page's purpose on the site.

**Site.** Each page belongs to a specific site. Therefore, when creating a page, you need to pass `SITE_ID`. The site ID can be obtained using the methods [landing.site.getList](../site/landing-site-get-list.md) or [landing.site.add](../site/landing-site-add.md).

**Folder.** A page can be placed in a folder to organize the site structure. For this, `FOLDER_ID` is used. The list of site folders is returned by [landing.site.getFolders](../site/landing-site-get-folders.md), and moving a page to another folder or site is done using [landing.landing.move](./methods/landing-landing-move.md).

**View Template.** When creating or updating a page, you can pass `TPL_ID` to immediately set its design and structure. The list of templates is returned by [landing.template.getlist](../template/landing-template-get-list.md). More details about them can be found in the section [View Template](../template/index.md).

**Blocks.** The content of the page consists of blocks. The methods in this section allow you to manage blocks on the page: add, delete, and change their order. To modify the content of a block, use the methods in the section [Working with Page Blocks](./block-methods/index.md).

**Special Pages.** A page can be designated as a special page of the site. For example, it can be made the main page or a service page for a specific scenario. For this, use the methods from the section [Special Pages](./special-pages/index.md).

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Working with Pages

#| 
|| **Method** | **Description** ||
|| [landing.landing.add](./methods/landing-landing-add.md) | Adds a page ||
|| [landing.landing.addByTemplate](./methods/landing-landing-add-by-template.md) | Creates a page from a template ||
|| [landing.landing.copy](./methods/landing-landing-copy.md) | Copies a page ||
|| [landing.landing.update](./methods/landing-landing-update.md) | Modifies page parameters ||
|| [landing.landing.move](./methods/landing-landing-move.md) | Moves a page to another site or folder ||
|| [landing.landing.getList](./methods/landing-landing-get-list.md) | Retrieves a list of pages ||
|| [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md) | Retrieves additional fields of the page ||
|| [landing.landing.getpreview](./methods/landing-landing-get-preview.md) | Returns the path to the page preview ||
|| [landing.landing.getpublicurl](./methods/landing-landing-get-public-url.md) | Returns the public URL of the page ||
|| [landing.landing.resolveIdByPublicUrl](./methods/landing-landing-resolve-id-by-public-url.md) | Returns the page ID by public URL ||
|| [landing.landing.publication](./methods/landing-landing-publication.md) | Makes the page available in the current context ||
|| [landing.landing.unpublic](./methods/landing-landing-unpublic.md) | Hides the page in the current context ||
|| [landing.landing.markDelete](./methods/landing-landing-mark-delete.md) | Marks the page as deleted ||
|| [landing.landing.markUnDelete](./methods/landing-landing-mark-undelete.md) | Restores the page from deleted ||
|| [landing.landing.removeEntities](./methods/landing-landing-remove-entities.md) | Deletes blocks and images from the page ||
|| [landing.landing.delete](./methods/landing-landing-delete.md) | Deletes the page ||
|#

### Working with Blocks

#| 
|| **Method** | **Description** ||
|| [landing.landing.addblock](./block-methods/landing-landing-add-block.md) | Adds a new block to the page ||
|| [landing.landing.copyblock](./block-methods/landing-landing-copy-block.md) | Copies a block from one page to another ||
|| [landing.landing.deleteblock](./block-methods/landing-landing-delete-block.md) | Deletes a block from the page ||
|| [landing.landing.downblock](./block-methods/landing-landing-down-block.md) | Moves a block down one position ||
|| [landing.landing.favoriteBlock](./block-methods/landing-landing-favorite-block.md) | Saves a block to "My Blocks" ||
|| [landing.landing.hideblock](./block-methods/landing-landing-hide-block.md) | Hides a block on the page ||
|| [landing.landing.markdeletedblock](./block-methods/landing-landing-mark-deleted-block.md) | Marks a block as deleted without physically removing it ||
|| [landing.landing.markundeletedblock](./block-methods/landing-landing-mark-undeleted-block.md) | Restores a block from deleted ||
|| [landing.landing.moveblock](./block-methods/landing-landing-move-block.md) | Moves a block from one page to another ||
|| [landing.landing.showblock](./block-methods/landing-landing-show-block.md) | Displays a block on the page ||
|| [landing.landing.unFavoriteBlock](./block-methods/landing-landing-unfavorite-block.md) | Removes a block from "My Blocks" ||
|| [landing.landing.upblock](./block-methods/landing-landing-up-block.md) | Moves a block up one position ||
|#

### Special Pages

#| 
|| **Method** | **Description** ||
|| [landing.syspage.deleteForLanding](./special-pages/landing-syspage-delete-for-landing.md) | Removes the page's binding as a special one ||
|| [landing.syspage.deleteForSite](./special-pages/landing-syspage-delete-for-site.md) | Deletes all special pages of the site ||
|| [landing.syspage.getSpecialPage](./special-pages/landing-syspage-get-special-page.md) | Retrieves the address of the special page of the site ||
|| [landing.syspage.get](./special-pages/landing-syspage-get.md) | Retrieves a list of special pages ||
|| [landing.syspage.set](./special-pages/landing-syspage-set.md) | Assigns a special page for the site ||
|#