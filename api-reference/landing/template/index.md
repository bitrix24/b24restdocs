# Object View Template: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A view template is a structure that Bitrix24 uses to assemble a website page. It defines where the main content will be located and which additional design elements are included. The template does not store content; it only specifies the layout of the page sections.

This helps avoid recreating the design for each page. Common elements are placed in separate areas and reused. For example, you can create a page with a header and connect it to all pages of the site. If the header changes, the updates will apply across all pages.

> Quick Navigation: [all methods](#all-methods)

## How the Template Works at the Site and Page Level

The template can be set at either the site or page level.

**Site.** The site template defines the overall appearance of all pages. For instance, if a template with a header and footer is selected at the site level, this structure will be used by default on all pages. The site template identifier is passed in the [`TPL_ID`](../site/base-fields.md) field. The included areas at the site level are managed and modified using the [landing.template.getSiteRef](./landing-template-get-site-ref.md) and [landing.template.setSiteRef](./landing-template-set-site-ref.md) methods.

**Page.** A specific page can have its own template. This is necessary if the page needs to differ from the overall site structure. For example, the site may have a template with a sidebar, while a promotional landing page may not have a sidebar. The page template identifier is passed in the `TPL_ID` field of the [landing.landing.add](../page/methods/landing-landing-add.md) and [landing.landing.update](../page/methods/landing-landing-update.md) methods. The bindings for included areas for the page are managed by the [landing.template.getLandingRef](./landing-template-get-landing-ref.md) and [landing.template.setLandingRef](./landing-template-set-landing-ref.md) methods.

**Priority of Settings.** If the settings for the site and page differ, Bitrix24 applies the page settings.

## System Templates

Bitrix24 includes ready-made view templates. These define the structure of the page: for example, whether it will have a header, footer, or sidebar. These templates are already configured in Bitrix24. It is not possible to create a new system template or modify an existing one.

Each template has two identifiers:

-  `XML_ID` — the external code of the template. For example, `sidebar_right` is a template with a right sidebar, while `header_footer` is a template with a header and footer.

-  `ID` — the internal identifier of the template.

The table below lists the external codes `XML_ID` of the system templates. To obtain the internal `ID` of a template by its `XML_ID`, use [landing.template.getlist](./landing-template-get-list.md).

#|
|| **XML_ID** | **Name** ||
|| `empty` | Empty ||
|| `sidebar_left` | With left sidebar ||
|| `sidebar_right` | With right sidebar ||
|| `header_footer` | With header and footer ||
|| `without_left` | Without left sidebar ||
|| `without_right` | Without right sidebar ||
|#

## How Included Areas Work

Included areas are separate pages that are inserted into the template and used as recurring design elements: header, footer, or sidebar.

For example, you can create a separate page with a menu and a banner in the header, bind it to the area `#AREA_1#`, and then this block will automatically appear on all pages of the site that use such a template.

In the template, each area is designated as `#AREA_N#`, where `N` is the area number. For example:

-  `#AREA_1#` — the first additional area,

-  `#AREA_2#` — the second additional area.

To configure the areas, you need to know:

-  the identifier of the site or page for which the binding is saved, and the identifier of the page that needs to be inserted into the area. The site identifier can be obtained using the [landing.site.getList](../site/landing-site-get-list.md) method, while the page identifier can be obtained using the [landing.landing.getList](../page/methods/landing-landing-get-list.md) method,

-  the area number from the `CONTENT` of the template. The `CONTENT` field is returned by the [landing.template.getlist](./landing-template-get-list.md) method, and its structure is described in the article [Template Fields](./fields.md),

-  the final set of bindings for reading or writing. The necessary methods are listed in the [overview below](#all-methods).

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### View Templates

#|
|| **Method** | **Description** ||
|| [landing.template.getlist](./landing-template-get-list.md) | Retrieves a list of view templates ||
|#

### Included Areas of the Site

#|
|| **Method** | **Description** ||
|| [landing.template.getSiteRef](./landing-template-get-site-ref.md) | Retrieves a list of included areas for the site ||
|| [landing.template.setSiteRef](./landing-template-set-site-ref.md) | Sets included areas for the site ||
|#

### Included Areas of the Page

#|
|| **Method** | **Description** ||
|| [landing.template.getLandingRef](./landing-template-get-landing-ref.md) | Retrieves a list of included areas for the page ||
|| [landing.template.setLandingRef](./landing-template-set-landing-ref.md) | Sets included areas for the page ||
|#