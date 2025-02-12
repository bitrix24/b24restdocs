# Object Fields Page

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- links to pages that have not yet been created are not provided (view template)

{% endnote %}

{% endif %}

> Quick navigation: [all methods](#all-methods) 

#|

|| **Fields** | **Description** | **Read** | **Write** ||
|| **ID**
[`unknown`](../../data-types.md) | Page identifier. Automatically created and unique within the database. | Yes | No ||
|| **CODE^*^**
[`unknown`](../../data-types.md) | Unique symbolic code of the page. Added to the website address if it is not the main page. | Yes | Yes ||
|| **RULE**
[`unknown`](../../data-types.md) | Regular expression for displaying the page by mask. For example, the rule `section/([\d]+)` for a page at the root of the site will match all pages of the form `/section/<n>/`, where <n> is any number. | Yes | No ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | Page activity: Y / N. | Yes | No ||
|| **DELETED**
[`unknown`](../../data-types.md) | Flag [deleted page](*deleted_page): Y / N.  | Yes | Yes ||
|| **TITLE^*^**
[`unknown`](../../data-types.md) | Page title. | Yes | Yes ||
|| **XML_ID**
[`unknown`](../../data-types.md) | External key for developer needs. Not used by the service. | Yes | Yes ||
|| **DESCRIPTION**
[`unknown`](../../data-types.md) | Arbitrary description of the page. Displayed in the list of pages. | Yes | Yes ||
|| **SITE_ID^*^**
[`unknown`](../../data-types.md) | Identifier of the site to which the page is linked. | Yes | Yes ||
|| **CREATED_BY_ID**
[`unknown`](../../data-types.md) | Identifier of the user who created the page. | Yes | No ||
|| **MODIFIED_BY_ID**
[`unknown`](../../data-types.md) | Identifier of the user who modified the page. | Yes | No ||
|| **DATE_CREATE**
[`unknown`](../../data-types.md) | Creation date. | Yes | No ||
|| **DATE_MODIFY**
[`unknown`](../../data-types.md) | Modification date. | Yes | No ||
|| **SITEMAP**
[`unknown`](../../data-types.md) | The page is present in the sitemap (`/sitemap.xml`), Y / N. | Yes | Yes ||
|| **FOLDER_ID**
[`unknown`](../../data-types.md) | Identifier of the folder where the page is located. | Yes | Yes ||
|| **TPL_ID**
[`unknown`](../../data-types.md) | Identifier of the [view template](../template/index.md). | Yes | Yes ||
|| **TPL_CODE**
[`unknown`](../../data-types.md) | Identifier of the partner solution template on which the site was created. For example, bitrix.eshop. | Yes | No ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Overview of Methods {#all-methods}

### Working with Blocks

#|
|| **Method** | **Description** | **Version** ||
|| [landing.landing.addblock](./block-methods/landing-landing-add-block.md) | Method for adding a new block to the page. | ||
|| [landing.landing.copyblock](./block-methods/landing-landing-copy-block.md) | Method for copying a block from one page to another. | ||
|| [landing.landing.deleteblock](./block-methods/landing-landing-delete-block.md) | Method for deleting a block from the page. | ||
|| [landing.landing.downblock](./block-methods/landing-landing-down-block.md) | Method for moving a block down one position on the page. | ||
|| [landing.landing.favoriteBlock](./block-methods/landing-landing-favorite-block.md) | Method saves an existing block on the page to "My Blocks." | 21.800.0 ||
|| [landing.landing.hideblock](./block-methods/landing-landing-hide-block.md) | Method hides a block from the page. | ||
|| [landing.landing.markdeletedblock](./block-methods/landing-landing-mark-deleted-block.md) | Method marks a block as deleted but does not physically remove it. | ||
|| [landing.landing.markundeletedblock](./block-methods/landing-landing-mark-undeleted-block.md) | Method restores a block from marked as deleted. | ||
|| [landing.landing.moveblock](./block-methods/landing-landing-move-block.md) | Method for moving a block from one page to another. | ||
|| [landing.landing.showblock](./block-methods/landing-landing-show-block.md) | Method for displaying a block on the page. | ||
|| [landing.landing.unFavoriteBlock](./block-methods/landing-landing-unfavorite-block.md) | Method removes a block that was saved in "My Blocks." | 21.800.0 ||
|| [landing.landing.upblock](./block-methods/landing-landing-up-block.md) | Method for moving a block up one position on the page. | ||
|#

### Working with Pages

#|
|| **Method** | **Description** | **Version** ||
|| [landing.landing.add](./methods/landing-landing-add.md) | Method for adding a page. | ||
|| [landing.landing.addByTemplate](./methods/landing-landing-add-by-template.md) | Method for adding a page by template. | ||
|| [landing.landing.copy](./methods/landing-landing-copy.md) | Method copies the specified page. | ||
|| [landing.landing.delete](./methods/landing-landing-delete.md) | Method for deleting a page. | ||
|| [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md) | Method for obtaining additional fields of the page. | ||
|| [landing.landing.getlist](./methods/landing-landing-get-list.md) | Method for obtaining a list of pages. | ||
|| [landing.landing.getpreview](./methods/landing-landing-get-preview.md) | Method returns the path to the page preview. | ||
|| [landing.landing.getpublicurl](./methods/landing-landing-get-public-url.md) | Method returns the web address of the page. | ||
|| [landing.landing.markDelete](./methods/landing-landing-mark-delete.md) | Method marks the page as deleted. | ||
|| [landing.landing.markUnDelete](./methods/landing-landing-mark-undelete.md) | Method marks the page as not deleted. | ||
|| [landing.landing.move](./methods/landing-landing-move.md) | Method moves the page to another site and/or folder. | 21.800.0 ||
|| [landing.landing.publication](./methods/landing-landing-publication.md) | Method for publishing the page. | ||
|| [landing.landing.removeEntities](./methods/landing-landing-remove-entities.md) | Method removes related landing entities. | ||
|| [landing.landing.resolveIdByPublicUrl](./methods/landing-landing-resolve-id-by-public-url.md) | Method returns the page identifier by the provided relative URL. | 21.800.0 ||
|| [landing.landing.unpublic](./methods/landing-landing-unpublic.md) | Method for unpublishing the page. | ||
|| [landing.landing.update](./methods/landing-landing-update.md) | Method for modifying the page. | ||
|#

### Special Pages

#|
|| **Method** | **Description** ||
|| [landing.syspage.deleteForLanding](./special-pages/landing-syspage-delete-for-landing.md) | Deletes all mentions of the page as special. ||
|| [landing.syspage.deleteForSite](./special-pages/landing-syspage-delete-for-site.md) | Deletes all special pages. ||
|| [landing.syspage.getSpecialPage](./special-pages/landing-syspage-get-special-page.md) | Gets the address of the special page of the site. ||
|| [landing.syspage.get](./special-pages/landing-syspage-get.md) | Gets a list of special pages. ||
|| [landing.syspage.set](./special-pages/landing-syspage-set.md) | Sets a special page for the site. ||
|#

[*deleted_page]: Marked as deleted entities do not appear in any requests. The system does not see them. Through REST, you can only access such entities by explicitly specifying in the filter DELETED=Y.