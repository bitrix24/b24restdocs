# Working with Pages: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A page is always associated with a website. It can be created at the root of the site or within a folder. The methods `landing.landing.*` are used to:

- create, copy, and move pages,
- retrieve a list of pages and metadata,
- modify page parameters,
- publish and unpublish a page,
- restore and delete pages.

For example, you can create a promotional page within a store's website, publish it, and when the promotion ends, move it to another folder or unpublish it.

> Quick Navigation: [All Methods](#all-methods)

## How to Create a Website Page

1. Obtain the site ID using the methods [landing.site.getList](../../site/landing-site-get-list.md) or [landing.site.add](../../site/landing-site-add.md).
2. If the page should reside in a folder, get the folder ID using the method [landing.site.getFolders](../../site/landing-site-get-folders.md).
3. Create the page using the method [landing.landing.add](./landing-landing-add.md), [landing.landing.addByTemplate](./landing-landing-add-by-template.md), or [landing.landing.copy](./landing-landing-copy.md).
4. To find the page and check its parameters, use [landing.landing.getList](./landing-landing-get-list.md), [landing.landing.getadditionalfields](./landing-landing-get-additional-fields.md), [landing.landing.getpreview](./landing-landing-get-preview.md), and [landing.landing.getpublicurl](./landing-landing-get-public-url.md).
5. After configuration, publish the page using the method [landing.landing.publication](./landing-landing-publication.md). If you need to hide the page, use [landing.landing.unpublic](./landing-landing-unpublic.md).

## Relationships with Other Objects

A page in Bitrix24 is linked to other objects. The site provides the overall context, the folder indicates the page's location in the structure, the view template defines the layout, blocks form the content, and special pages determine its role on the site.

**Site.** Each page belongs to a specific site. Therefore, when creating a page, you need to provide the `SITE_ID`, and to search for pages, specify a filter by site. The site ID can be obtained using the methods [landing.site.getList](../../site/landing-site-get-list.md) or [landing.site.add](../../site/landing-site-add.md).

**Folder.** A page can be created within a folder. For this, the `FOLDER_ID` is provided. Later, the page can be moved to another folder or site using the method [landing.landing.move](./landing-landing-move.md). The list of site folders is returned by the method [landing.site.getFolders](../../site/landing-site-get-folders.md).

**View Template.** When creating or updating a page, you can provide `TPL_ID` to immediately associate it with a view template. This helps set the desired structure and design. The list of templates is returned by [landing.template.getlist](../../template/landing-template-get-list.md), and the templates themselves are described in the section [View Template](../../template/index.md).

**Blocks.** The methods in this section manage the page but do not edit the structure and content of the blocks. To work with blocks, use [Page Blocks](../block-methods/index.md).

**Special Pages.** A regular page can be designated as a special page of the site, such as the main page, cart, or checkout page. For this, use the methods [Special Pages](../special-pages/index.md).

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Creating and Modifying a Page

#| 
|| **Method** | **Description** ||
|| [landing.landing.add](./landing-landing-add.md) | Adds a page or folder ||
|| [landing.landing.addByTemplate](./landing-landing-add-by-template.md) | Creates a page from a template ||
|| [landing.landing.copy](./landing-landing-copy.md) | Copies a page ||
|| [landing.landing.update](./landing-landing-update.md) | Updates page parameters ||
|| [landing.landing.move](./landing-landing-move.md) | Moves a page to another site or folder ||
|#

### Retrieving Data and Navigation

#| 
|| **Method** | **Description** ||
|| [landing.landing.getList](./landing-landing-get-list.md) | Retrieves a list of pages ||
|| [landing.landing.getadditionalfields](./landing-landing-get-additional-fields.md) | Retrieves additional fields of the page ||
|| [landing.landing.getpreview](./landing-landing-get-preview.md) | Retrieves the preview URL of the page ||
|| [landing.landing.getpublicurl](./landing-landing-get-public-url.md) | Retrieves the public URL of the page ||
|| [landing.landing.resolveIdByPublicUrl](./landing-landing-resolve-id-by-public-url.md) | Determines the page ID by public URL ||
|#

### Publication

#| 
|| **Method** | **Description** ||
|| [landing.landing.publication](./landing-landing-publication.md) | Publishes a page ||
|| [landing.landing.unpublic](./landing-landing-unpublic.md) | Unpublishes a page ||
|#

### Deletion and Restoration

#| 
|| **Method** | **Description** ||
|| [landing.landing.markDelete](./landing-landing-mark-delete.md) | Marks the page as deleted ||
|| [landing.landing.markUnDelete](./landing-landing-mark-undelete.md) | Restores the page from the trash ||
|| [landing.landing.removeEntities](./landing-landing-remove-entities.md) | Deletes blocks and clears image file bindings of the page ||
|| [landing.landing.delete](./landing-landing-delete.md) | Deletes a page ||
|#