# Site Fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Site fields describe the title, address, type, publication status, and system pages of a site. The method [landing.site.getList](./landing-site-get-list.md) returns these fields, and the methods [landing.site.add](./landing-site-add.md) and [landing.site.update](./landing-site-update.md) accept some of them in the `fields` parameter.

Additional site fields, such as the theme, background, analytics, and custom code, are not included in this table. They are passed in the `ADDITIONAL_FIELDS` array and read with the method [landing.site.getadditionalfields](./landing-site-get-additional-fields.md). They are described in the article [{#T}](./additional-fields.md).

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can retrieve the fields: a user with the "view" access permission for sites. Changing the fields requires the "edit settings" access permission for the site

## How to Retrieve Site Fields

Pass the names of the fields you need in `params.select` of the method [landing.site.getList](./landing-site-get-list.md). For example, to find a site by its external identifier and retrieve its address and publication status:

```json
{
    "params": {
        "select": ["ID", "TITLE", "ACTIVE", "PUBLIC_URL"],
        "filter": {
            "=XML_ID": "my-integration-site-1"
        }
    }
}
```

If `select` is not passed, the method returns all fields from the tables of [main](#main) and [service](#service) fields. [Calculated fields](#calculated) are returned only if you specify them in `select`. The method always adds the `ID` and `TYPE` fields to the selection.

By default, `landing.site.getList` returns only sites with the value `DELETED = "N"`. To retrieve sites from the trash, pass `DELETED` or `=DELETED` in `filter`.

## What You Need to Know

- When created via `landing.site.add`, the site is created unpublished with the value `ACTIVE = "N"`.
- The methods `landing.site.add` and `landing.site.update` do not save the `ACTIVE`, `SPECIAL`, and `TPL_CODE` fields: values from `fields` are ignored without an error. Publication is managed by the methods [landing.site.publication](./landing-site-publication.md) and [landing.site.unpublic](./landing-site-unpublic.md).
- If the site is moved to the trash, it is automatically unpublished.
- To delete and restore a site, use [landing.site.markDelete](./landing-site-mark-delete.md) and [landing.site.markUnDelete](./landing-site-mark-undelete.md).
- Bitrix24 fills in the `CREATED_BY_ID`, `MODIFIED_BY_ID`, `DATE_CREATE`, and `DATE_MODIFY` fields automatically when a site is created or updated.

## Main Fields {#main}

#|
|| **Field**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Site identifier. The field is always present in the response of `landing.site.getList` ||
|| **TITLE**
[`string`](../../data-types.md) | Site title. This field is required when creating a site ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the site. In the response of `landing.site.getList`, it is returned in the format `/code/`. When creating and updating, you can pass the value without the outer `/`. If an empty string is passed, the code will be generated from `TITLE` or from the string `site`. A code consisting of only digits receives the prefix `site` ||
|| **TYPE**
[`string`](../../data-types.md) | Type of the site.

Possible values:
`PAGE` — site
`STORE` — store
`SMN` — project
`KNOWLEDGE` — knowledge base
`GROUP` — group site
`MAINPAGE` — main page or vibe

Default is `PAGE`.

For more details on types and internal scopes, read the article [Working with Site Types and Scopes](../types.md) ||
|| **ACTIVE**
[`string`](../../data-types.md) | Site publication status.

Possible values:
`Y` — site is published
`N` — site is not published

When created via `landing.site.add`, the site receives the value `N`. The field cannot be changed through `landing.site.add` and `landing.site.update`. To publish and unpublish, use [landing.site.publication](./landing-site-publication.md) and [landing.site.unpublic](./landing-site-unpublic.md) ||
|| **DELETED**
[`string`](../../data-types.md) | Trash flag.

Possible values:
`Y` — site is in the trash
`N` — site is not in the trash

By default, `landing.site.getList` returns only sites with the value `DELETED = "N"` ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Brief description of the site. Displayed in the list of sites ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier of the site. It is convenient for finding the site from an integration without storing the internal `ID` ||
|| **DOMAIN_ID**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Domain identifier. The domain name is returned by the calculated field `DOMAIN_NAME` ||
|| **LANG**
[`string`](../../data-types.md) | Two-letter code of the site's language zone, for example `de`, `en`, `fr`, `it`, `es`, `jp`. It may be an empty string if the site language is not set ||
|| **TPL_ID**
[`integer`](../../data-types.md) | Identifier of the [view template](../template/index.md) of the site ||
|| **LANDING_ID_INDEX**
[`integer`](../../data-types.md) | Identifier of the main page of the site. This field is also used to obtain `PREVIEW_PICTURE` ||
|| **LANDING_ID_404**
[`integer`](../../data-types.md) | Identifier of the `404` error page ||
|| **LANDING_ID_503**
[`integer`](../../data-types.md) | Identifier of the `503` error page ||
|| **CREATED_BY_ID**
[`integer`](../../data-types.md) | Identifier of the user who created the site ||
|| **MODIFIED_BY_ID**
[`integer`](../../data-types.md) | Identifier of the user who last modified the site ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Date and time of site creation. In the REST response, it is returned as a string. The format depends on the portal's regional settings, for example `04/20/2020 12:48:10 pm`. The value in ISO 8601 format is returned by the `DATE_CREATE_ISO` field ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Date and time of the last modification of the site. In the REST response, it is returned as a string. The format depends on the portal's regional settings, for example `04/20/2020 12:48:10 pm`. The value in ISO 8601 format is returned by the `DATE_MODIFY_ISO` field ||
|#

## Service Fields {#service}

Bitrix24 fills in these fields itself. The method `landing.site.getList` returns them when requested without `select` or when they are explicitly specified in `select`. You do not need to pass them to `landing.site.add` and `landing.site.update`.

#|
|| **Field**
`type` | **Description** ||
|| **TPL_CODE**
[`string`](../../data-types.md) \| `null` | Code of the template of the preset solution the site was created from, for example `clothes`. If the site was not created from a preset solution, `null` is returned. The methods `landing.site.add` and `landing.site.update` ignore the value from `fields` ||
|| **SPECIAL**
[`string`](../../data-types.md) | Technical site indicator.

Possible values:
`Y` — technical site, for example a site for CRM forms
`N` — regular site

Default is `N`. The methods `landing.site.add` and `landing.site.update` ignore the value from `fields` ||
|| **SMN_SITE_ID**
[`string`](../../data-types.md) \| `null` | Identifier of the main module site the site is linked to. If there is no link, `null` is returned ||
|| **VERSION**
[`integer`](../../data-types.md) | Internal version of the site ||
|#

## Calculated Fields {#calculated}

These fields are not stored in the site table. The method `landing.site.getList` generates them in the response.

#|
|| **Field**
`type` | **Description** ||
|| **DOMAIN_NAME**
[`string`](../../data-types.md) | Domain name of the site. Returned if the field is specified in `select` ||
|| **PUBLIC_URL**
[`string`](../../data-types.md) | Full public URL of the site. It may be an empty string if the URL could not be determined. Returned if the field is specified in `select` ||
|| **PREVIEW_PICTURE**
[`string`](../../data-types.md) | URL of the preview of the main page of the site. It may be an empty string if the preview is unavailable. Returned if the field is specified in `select` ||
|| **PHONE**
[`string`](../../data-types.md) \| `null` | Phone number of the CRM contact associated with the site. Returned if the field is specified in `select` ||
|| **DATE_CREATE_ISO**
[`string`](../../data-types.md) | Date and time of site creation in ISO 8601 format, for example `2020-04-20T12:48:10+02:00`. Returned together with `DATE_CREATE` ||
|| **DATE_MODIFY_ISO**
[`string`](../../data-types.md) | Date and time of the last modification of the site in ISO 8601 format. Returned together with `DATE_MODIFY` ||
|#

## Continue Your Learning

- create a site and change its fields — [{#T}](./landing-site-add.md), [{#T}](./landing-site-update.md)
- retrieve a list of sites with selected fields — [{#T}](./landing-site-get-list.md)
- publish or unpublish a site — [{#T}](./landing-site-publication.md), [{#T}](./landing-site-unpublic.md)
- retrieve the public address and preview of a site — [{#T}](./landing-site-get-public-url.md), [{#T}](./landing-site-get-preview.md)
- work with additional site fields — [{#T}](./additional-fields.md), [{#T}](./landing-site-get-additional-fields.md)
- choose a site type and internal scope — [{#T}](../types.md)