# Site Fields

Site fields are used in the method [landing.site.getList](./landing-site-get-list.md).

Some fields can be passed when creating and updating a site through the methods [landing.site.add](./landing-site-add.md) and [landing.site.update](./landing-site-update.md).

Additional site fields are passed separately in the `ADDITIONAL_FIELDS` array and can be read through [landing.site.getadditionalfields](./landing-site-get-additional-fields.md).

## What You Need to Know

- When created via `landing.site.add`, the site is created unpublished with the value `ACTIVE = N`.
- If the site is moved to the trash, it is automatically unpublished.
- To delete and restore a site, use [landing.site.markDelete](./landing-site-mark-delete.md) and [landing.site.markUnDelete](./landing-site-mark-undelete.md).
- System fields may appear in the response of `landing.site.getList`, but they are not set manually.

## Main Fields

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
`PAGE`  site
`STORE`  store
`SMN`  project
`KNOWLEDGE`  knowledge base
`GROUP`  group site
`MAINPAGE`  main page or vibe

Default is `PAGE`.

For more details on types and internal scopes, read the article [Working with Site Types and Scopes](../types.md) ||
|| **ACTIVE**
[`string`](../../data-types.md) | Site publication status.

Possible values:
`Y`  site is published
`N`  site is not published

When created via `landing.site.add`, the site receives the value `N`. To publish and unpublish, use [landing.site.publication](./landing-site-publication.md) and [landing.site.unpublic](./landing-site-unpublic.md) ||
|| **DELETED**
[`string`](../../data-types.md) | Trash flag.

Possible values:
`Y`  site is in the trash
`N`  site is not in the trash

By default, `landing.site.getList` returns only sites with the value `DELETED = "N"` ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Brief description of the site. Displayed in the list of sites ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier of the site ||
|| **DOMAIN_ID**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Domain identifier ||
|| **LANG**
[`string`](../../data-types.md) | Two-letter code of the site's language zone, for example `de`, `en`, `fr`, `it`, `es`, `jp` ||
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
[`datetime`](../../data-types.md) | Date and time of site creation. In the REST response, it is returned as a string. The format depends on the portal's regional settings, for example `04/20/2020 12:48:10 pm` ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Date and time of the last modification of the site. In the REST response, it is returned as a string. The format depends on the portal's regional settings, for example `04/20/2020 12:48:10 pm` ||
|#

## Calculated Fields

These fields are not stored in the site table. The method `landing.site.getList` returns them when requested in `select`.

#|
|| **Field**
`type` | **Description** ||
|| **DOMAIN_NAME**
[`string`](../../data-types.md) | Domain name of the site ||
|| **PUBLIC_URL**
[`string`](../../data-types.md) | Full public URL of the site. It may be an empty string if the URL could not be determined ||
|| **PREVIEW_PICTURE**
[`string`](../../data-types.md) | URL of the preview of the main page of the site. It may be an empty string if the preview is unavailable ||
|| **PHONE**
[`string`](../../data-types.md) \| `null` | Phone number of the CRM contact associated with the site ||
|#

## Continue Your Learning

- [{#T}](./landing-site-add.md)
- [{#T}](./landing-site-update.md)
- [{#T}](./landing-site-get-list.md)
- [{#T}](./landing-site-get-public-url.md)
- [{#T}](./landing-site-get-preview.md)
- [{#T}](./landing-site-get-additional-fields.md)
- [{#T}](./additional-fields.md)