# Page Fields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Page fields are used in the method [landing.landing.getList](./methods/landing-landing-get-list.md).

Some fields can be passed when creating and updating a page through the methods [landing.landing.add](./methods/landing-landing-add.md) and [landing.landing.update](./methods/landing-landing-update.md).

Additional page fields are not stored with the main ones. They are passed separately in the `ADDITIONAL_FIELDS` array and read through [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md).

## What You Need to Know

- The REST methods for the page object belong to the `landing` scope.
- A new page created via `landing.landing.add` and `landing.landing.addByTemplate` is created unpublished with the value `ACTIVE = N`.
- If a page is moved to the trash, it is automatically unpublished.
- To publish and unpublish, use [landing.landing.publication](./methods/landing-landing-publication.md) and [landing.landing.unpublic](./methods/landing-landing-unpublic.md).

## Main Fields

#| 
|| **Field**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the page. In `landing.landing.getList`, this field is always present in the response ||
|| **TITLE**
[`string`](../../data-types.md) | Title of the page. This field is required when creating via `landing.landing.add` ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the page. It forms the page address within the site. When creating, if the field is not passed or a string of spaces is passed, the code is generated from `TITLE`.

If the generated address matches the address of an already existing page in the same section of the site, the system will automatically add a suffix of 4 random characters to `CODE`, for example, `my-page_a1b2`. The final value of `CODE` may differ from the one passed ||
|| **SITE_ID**
[`integer`](../../data-types.md) | Identifier of the site to which the page belongs ||
|| **FOLDER_ID**
[`integer`](../../data-types.md) | Identifier of the folder in which the page is located. If the value is empty or equals `0`, the page is in the root of the site ||
|| **TPL_ID**
[`integer`](../../data-types.md) | Identifier of the [view template](../template/index.md) for the page. If the value is empty, the page uses the site template ||
|| **ACTIVE**
[`string`](../../data-types.md) | Publication status of the page.

Possible values:
`Y`  the page is published
`N`  the page is not published

A new page is created with the value `N`. To change the status, use [landing.landing.publication](./methods/landing-landing-publication.md) and [landing.landing.unpublic](./methods/landing-landing-unpublic.md) ||
|| **DELETED**
[`string`](../../data-types.md) | Trash flag.

Possible values:
`Y`  the page is in the trash
`N`  the page is not in the trash

When moved to the trash, the page automatically receives `ACTIVE = "N"`. For deletion and restoration, use [landing.landing.markDelete](./methods/landing-landing-mark-delete.md) and [landing.landing.markUnDelete](./methods/landing-landing-mark-undelete.md) ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Arbitrary description of the page. Displayed in the list of pages ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier of the page ||
|| **SITEMAP**
[`string`](../../data-types.md) | Flag for including the page in the sitemap.

Possible values:
`Y`  the page is included in `sitemap.xml`
`N`  the page is not included in `sitemap.xml` ||
|| **FOLDER**
[`string`](../../data-types.md) | Indicator that the object is used as a folder in the site structure, rather than as a regular page.

Possible values:
`Y`  the object is a folder
`N`  the object is a page

A folder is needed to group pages within the site. It can be created using the method [landing.landing.add](./methods/landing-landing-add.md) by passing `FOLDER = Y` ||
|| **RULE**
[`string`](../../data-types.md) | Regular expression for displaying the page by mask. For example, the rule `section/([\d]+)` for a page in the root of the site will match addresses like `/section/<n>/`, where `<n>` is any number ||
|| **CREATED_BY_ID**
[`integer`](../../data-types.md) | Identifier of the user who created the page ||
|| **MODIFIED_BY_ID**
[`integer`](../../data-types.md) | Identifier of the user who last modified the page ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Date and time of page creation. The format depends on Bitrix24 settings ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Date and time of the last modification of the page. The format depends on Bitrix24 settings ||
|| **DATE_PUBLIC**
[`datetime`](../../data-types.md) \| `null` | Date and time of page publication. Changing this field does not publish the page by itself. In the response, the value is returned as a string in the portal format or `null` for a page that has never been published ||
|#

## Calculated and Related Fields

These fields are not stored in the page table. The method `landing.landing.getList` returns them automatically or adds them based on separate selection flags.

#| 
|| **Field**
`type` | **Description** ||
|| **DOMAIN_ID**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the site domain to which the page is linked. The method `landing.landing.getList` always adds this field to the result ||
|| **PUBLIC_URL**
[`string`](../../data-types.md) \| `null` | Full public URL of the page. Returned in `landing.landing.getList` if the `get_urls` flag is enabled, and as a separate value in the method [landing.landing.getpublicurl](./methods/landing-landing-get-public-url.md) ||
|| **PREVIEW**
[`string`](../../data-types.md) \| `null` | URL or relative path to the page preview. Returned in `landing.landing.getList` if the `get_preview` flag is enabled, and as a separate value in the method [landing.landing.getpreview](./methods/landing-landing-get-preview.md) ||
|| **IS_AREA**
[`boolean`](../../data-types.md) | Indicator that the page is used as an included area. Returned in `landing.landing.getList` if the `check_area` flag is enabled ||
|#

## Continue Your Exploration

- [{#T}](./methods/landing-landing-add.md)
- [{#T}](./methods/landing-landing-add-by-template.md)
- [{#T}](./methods/landing-landing-update.md)
- [{#T}](./methods/landing-landing-get-list.md)
- [{#T}](./methods/landing-landing-get-additional-fields.md)
- [{#T}](./methods/landing-landing-get-preview.md)
- [{#T}](./methods/landing-landing-get-public-url.md)
- [{#T}](./methods/landing-landing-mark-delete.md)
- [{#T}](./methods/landing-landing-mark-undelete.md)
- [{#T}](./additional-fields.md)