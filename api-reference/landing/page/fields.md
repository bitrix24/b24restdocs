# Page Fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Page fields are used in the method [landing.landing.getList](./methods/landing-landing-get-list.md).

Some fields can be passed when creating and updating a page through the methods [landing.landing.add](./methods/landing-landing-add.md) and [landing.landing.update](./methods/landing-landing-update.md).

Additional page fields are not stored with the main ones. They are passed separately in the `fields.ADDITIONAL_FIELDS` object of the [landing.landing.update](./methods/landing-landing-update.md) method and read through [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md).

## How to Retrieve Page Fields

List the fields you need in `params.select` of the [landing.landing.getList](./methods/landing-landing-get-list.md) method. The method adds the public URL and the preview of the page based on the `get_urls` and `get_preview` flags if `select` is not passed or contains `ID`. Example of a request for site pages with the URL and preview:

```json
{
    "params": {
        "select": ["ID", "TITLE", "ACTIVE"],
        "filter": {
            "SITE_ID": 273
        },
        "get_urls": true,
        "get_preview": true
    }
}
```

Each element of the `result` array in the response contains the fields from `select`, `DOMAIN_ID`, and the fields added by the flags:

```json
{
    "ID": "2513",
    "TITLE": "Spring Promotion",
    "ACTIVE": "Y",
    "DOMAIN_ID": "225",
    "PUBLIC_URL": "https://mysite.bitrix24site.com/spring-sale/",
    "PREVIEW": "https://mysite.bitrix24site.com/spring-sale/preview.jpg"
}
```

If `select` is not passed, the method returns all [main fields](#main) and system fields, such as `VERSION` and `SEARCH_CONTENT`. The method adds [calculated fields](#calculated), except `DOMAIN_ID`, only based on the flags.

## What You Need to Know

- The REST methods for the page object belong to the `landing` scope.
- A new page created via [landing.landing.add](./methods/landing-landing-add.md) and [landing.landing.addByTemplate](./methods/landing-landing-add-by-template.md) is created unpublished with the value `ACTIVE = N`.
- If a page is moved to the trash, it is automatically unpublished.
- To publish and unpublish, use [landing.landing.publication](./methods/landing-landing-publication.md) and [landing.landing.unpublic](./methods/landing-landing-unpublic.md).

## Main Fields {#main}

#| 
|| **Field**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the page. If `select` is passed to [landing.landing.getList](./methods/landing-landing-get-list.md), include `ID` in it: without it, the method returns only one page, and the `get_urls`, `get_preview`, and `check_area` flags do not work ||
|| **TITLE**
[`string`](../../data-types.md) | Title of the page. This field is required when creating via [landing.landing.add](./methods/landing-landing-add.md) ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the page. It forms the page address within the site. When creating, if the field is not passed or a string of spaces is passed, the code is generated from `TITLE`.

If the generated address matches the address of an already existing page in the same section of the site, the system will automatically add a suffix of 4 random characters to `CODE`, for example, `my-page_a1b2`. The final value of `CODE` may differ from the one passed ||
|| **SITE_ID**
[`integer`](../../data-types.md) | Identifier of the site to which the page belongs. This field is required when creating via [landing.landing.add](./methods/landing-landing-add.md) ||
|| **FOLDER_ID**
[`integer`](../../data-types.md) | Identifier of the folder in which the page is located. If the value is empty or equals `0`, the page is in the root of the site ||
|| **TPL_ID**
[`integer`](../../data-types.md) | Identifier of the [view template](../template/index.md) for the page. If the value is empty, the page uses the site template ||
|| **ACTIVE**
[`string`](../../data-types.md) | Publication status of the page.

Possible values:
`Y` — the page is published
`N` — the page is not published

A new page is created with the value `N`. To change the status, use [landing.landing.publication](./methods/landing-landing-publication.md) and [landing.landing.unpublic](./methods/landing-landing-unpublic.md) ||
|| **DELETED**
[`string`](../../data-types.md) | Trash flag.

Possible values:
`Y` — the page is in the trash
`N` — the page is not in the trash

You can move a page to the trash and restore it with the [landing.landing.markDelete](./methods/landing-landing-mark-delete.md) and [landing.landing.markUnDelete](./methods/landing-landing-mark-undelete.md) methods. When moved to the trash, the page receives `ACTIVE = "N"` ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Arbitrary description of the page. Displayed in the list of pages ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier of the page ||
|| **SITEMAP**
[`string`](../../data-types.md) | Flag for including the page in the sitemap.

Possible values:
`Y` — the page is included in `sitemap.xml`
`N` — the page is not included in `sitemap.xml` ||
|| **FOLDER**
[`string`](../../data-types.md) | Folder page flag: this is how folders used to work.

Possible values:
`Y` — a folder page
`N` — a regular page

Folders are now a separate object returned by the [landing.site.getFolders](../site/landing-site-get-folders.md) method. If you pass `FOLDER = Y` without `FOLDER_ID` to the [landing.landing.add](./methods/landing-landing-add.md) method, Bitrix24 creates a folder with the title and code of the page in the same call. The new page becomes the index page of the folder: it has `FOLDER = N`, and `FOLDER_ID` contains the identifier of the new folder ||
|| **RULE**
[`string`](../../data-types.md) | Regular expression for displaying the page by mask. For example, the rule `section/([\d]+)` for a page in the root of the site will match addresses like `/section/<n>/`, where `<n>` is any number. The [landing.landing.add](./methods/landing-landing-add.md) and [landing.landing.update](./methods/landing-landing-update.md) methods do not write this field ||
|| **CREATED_BY_ID**
[`integer`](../../data-types.md) | Identifier of the user who created the page ||
|| **MODIFIED_BY_ID**
[`integer`](../../data-types.md) | Identifier of the user who last modified the page ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Date and time of page creation. The format depends on Bitrix24 settings ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Date and time of the last modification of the page. The format depends on Bitrix24 settings ||
|| **DATE_PUBLIC**
[`datetime`](../../data-types.md) \| `null` | Date and time of page publication. Changing this field does not publish the page by itself. In the response, the value is returned as a string in the format set in the Bitrix24 settings or `null` for a page that has never been published ||
|#

## Calculated and Related Fields {#calculated}

These fields are not stored in the page table. The method [landing.landing.getList](./methods/landing-landing-get-list.md) returns them automatically or adds them based on separate selection flags.

#| 
|| **Field**
`type` | **Description** ||
|| **DOMAIN_ID**
[`string`](../../data-types.md) | Identifier of the site domain to which the page is linked. The method [landing.landing.getList](./methods/landing-landing-get-list.md) always adds this field to the result ||
|| **PUBLIC_URL**
[`string`](../../data-types.md) \| `null` | Full public URL of the page. Returned in [landing.landing.getList](./methods/landing-landing-get-list.md) if the `get_urls` flag is enabled, and as a separate value in the method [landing.landing.getpublicurl](./methods/landing-landing-get-public-url.md) ||
|| **PREVIEW**
[`string`](../../data-types.md) \| `null` | Link to the preview image of the page. Returned in [landing.landing.getList](./methods/landing-landing-get-list.md) if the `get_preview` flag is enabled, and as a separate value in the method [landing.landing.getpreview](./methods/landing-landing-get-preview.md) ||
|| **IS_AREA**
[`boolean`](../../data-types.md) | Indicator that the page is used as an included area. Returned in [landing.landing.getList](./methods/landing-landing-get-list.md) if the `check_area` flag is enabled ||
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