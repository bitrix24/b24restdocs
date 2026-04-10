# Special Website Pages: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In Bitrix24, a website page can be assigned a special type. For example, it can be designated as the main page, a catalog page, a cart page, or an order page.

This is necessary for Bitrix24 to know which page to use in each scenario. For instance, if a page is assigned the type `cart`, Bitrix24 will use it as the website's cart page.

The pages remain ordinary: they can be created, edited, and configured. Only their role in the website's operation changes.

Methods help assign a special type to a page, retrieve current bindings and addresses of special pages, and delete bindings.

> Quick Navigation: [All Methods](#all-methods)

## How to Assign and Retrieve Special Pages

1. Obtain the site ID using the methods [landing.site.getList](../../site/landing-site-get-list.md) or [landing.site.add](../../site/landing-site-add.md).
2. Get the page ID using the methods [landing.landing.getList](../methods/landing-landing-get-list.md), [landing.landing.add](../methods/landing-landing-add.md), [landing.landing.addByTemplate](../methods/landing-landing-add-by-template.md), or [landing.landing.copy](../methods/landing-landing-copy.md).
3. Assign the page as special using the method [landing.syspage.set](./landing-syspage-set.md). For this, specify the type of special page in the `type` parameter and the ID of the page to be bound.
4. Check current bindings using the method [landing.syspage.get](./landing-syspage-get.md) or retrieve the URL of a special page using the method [landing.syspage.getSpecialPage](./landing-syspage-get-special-page.md).
5. If you need to remove bindings, use [landing.syspage.deleteForLanding](./landing-syspage-delete-for-landing.md) or [landing.syspage.deleteForSite](./landing-syspage-delete-for-site.md). The first method removes all bindings for a specific page, while the second clears all special pages for the site.

## Codes of Special Pages

The `type` parameter is used for the special page type. It defines what role the page will play on the site.

#| 
|| **Type Code** | **Purpose** | **Marker in Page Body** ||
|| `mainpage` | Main Page | `#system_mainpage` ||
|| `catalog` | Main Catalog Page | `#system_catalog` ||
|| `personal` | Personal Section | `#system_personal` ||
|| `cart` | Cart | `#system_cart` ||
|| `order` | Order Processing | `#system_order` ||
|| `payment` | Payment Page | `#system_payment` ||
|| `compare` | Comparison Page | `#system_compare` ||
|| `feedback` | Feedback Page | `#system_feedback` ||
|#

If a page is assigned for the code on the current site, Bitrix24 will substitute its address instead of the marker. If there is no such binding, the marker will not work.

For example, if the cart page is assigned the type `cart`, then at the location of the marker `#system_cart`, Bitrix24 will substitute the address of that page. If the binding is removed, the address will no longer be substituted.

## Relation to Other Objects

Special pages are configured for a specific site. Each binding indicates which page Bitrix24 will use for the required scenario.

**Site.** The methods [landing.syspage.set](./landing-syspage-set.md), [landing.syspage.get](./landing-syspage-get.md), [landing.syspage.getSpecialPage](./landing-syspage-get-special-page.md), and [landing.syspage.deleteForSite](./landing-syspage-delete-for-site.md) operate in the context of the site. Therefore, `siteId` is always required for operation. It can be obtained using the methods [landing.site.getList](../../site/landing-site-get-list.md) or [landing.site.add](../../site/landing-site-add.md).

**Page.** An ordinary page can be assigned as special. The page ID can be obtained using the methods [landing.landing.getList](../methods/landing-landing-get-list.md), [landing.landing.add](../methods/landing-landing-add.md), [landing.landing.addByTemplate](../methods/landing-landing-add-by-template.md), or [landing.landing.copy](../methods/landing-landing-copy.md). If the page should no longer be used as special, all its bindings can be removed using the method [landing.syspage.deleteForLanding](./landing-syspage-delete-for-landing.md).

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Assigning and Retrieving

#| 
|| **Method** | **Description** ||
|| [landing.syspage.set](./landing-syspage-set.md) | Assigns a special page for the site ||
|| [landing.syspage.get](./landing-syspage-get.md) | Retrieves the list of special pages for the site ||
|| [landing.syspage.getSpecialPage](./landing-syspage-get-special-page.md) | Retrieves the URL of a special page for the site ||
|#

### Removing Bindings

#| 
|| **Method** | **Description** ||
|| [landing.syspage.deleteForLanding](./landing-syspage-delete-for-landing.md) | Removes all bindings of the page as special ||
|| [landing.syspage.deleteForSite](./landing-syspage-delete-for-site.md) | Removes all bindings of special pages for the site ||
|#