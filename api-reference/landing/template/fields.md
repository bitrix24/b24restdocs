# Template View Fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Template view fields define the structure Bitrix24 uses to assemble a site page: the placement of the main content and additional areas. The [landing.template.getlist](./landing-template-get-list.md) method returns these fields for templates in the current Bitrix24 account.

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can retrieve the fields: a user with access to the "Sites and Stores" section

## How to Retrieve Template Fields

Pass the names of the fields you need in `params.select` of the [landing.template.getlist](./landing-template-get-list.md) method. For example, to retrieve the template's external code, the number of additional areas, and the markup with their macros:

```json
{
    "params": {
        "select": ["ID", "XML_ID", "AREA_COUNT", "CONTENT"]
    }
}
```

If `select` is omitted, the method returns all available fields. The fields in the table below can be selected, filtered, sorted, and grouped. Fields with a dot in `select` and `filter`, such as `CREATED_BY.NAME`, are not supported.

## Template View Fields

#| 
|| **Field** 
`type` | **Description** || 
|| **ID** 
[`integer`](../../data-types.md) | Identifier of the template view || 
|| **ACTIVE** 
[`string`](../../data-types.md) | Activity status of the template view 

Possible values: 
`Y` — template is active 
`N` — template is inactive 

Default — `Y` || 
|| **AREA_COUNT** 
[`integer`](../../data-types.md) | Number of additional areas in the template, besides the main area `#CONTENT#`. Additional areas are denoted in `CONTENT` by the macros `#AREA_1#`, `#AREA_2#`, and so on || 
|| **SORT** 
[`integer`](../../data-types.md) | Sorting order of the template || 
|| **TITLE** 
[`string`](../../data-types.md) | Title of the template. For system templates, the value may be localized for the current language of the account || 
|| **XML_ID** 
[`string`](../../data-types.md) | External code of the template. It is convenient to use for matching templates in integration without relying on the internal `ID` || 
|| **CONTENT** 
[`string`](../../data-types.md) | HTML markup of the template with area macros, such as `#CONTENT#`, `#AREA_1#`, `#AREA_2#` || 
|| **CREATED_BY_ID** 
[`integer`](../../data-types.md) | Identifier of the user who created the template || 
|| **MODIFIED_BY_ID** 
[`integer`](../../data-types.md) | Identifier of the user who last modified the template || 
|| **DATE_CREATE** 
[`datetime`](../../data-types.md) | Date and time of template creation. In the REST response, it is returned as a string. The format depends on the regional settings of the account, for example, `04/20/2020 12:48:10 pm` || 
|| **DATE_MODIFY** 
[`datetime`](../../data-types.md) | Date and time of the last modification of the template. In the REST response, it is returned as a string. The format depends on the regional settings of the account, for example, `04/20/2020 12:48:10 pm` || 
|#

## How CONTENT and AREA_COUNT Relate

In `CONTENT`, the `#CONTENT#` macro marks the main area of the page. The `#AREA_1#`, `#AREA_2#`, and subsequent macros mark additional areas to which separate pages can be linked. Their number is specified in `AREA_COUNT`.

For example, a fragment of template markup with one additional area might look like this:

```html
<main>#CONTENT#</main>
<aside>#AREA_1#</aside>
```

In this example, `AREA_COUNT = 1`: `#CONTENT#` is not counted as an additional area. To find the area number for a particular template, retrieve its `CONTENT` using the [landing.template.getlist](./landing-template-get-list.md) method. The [landing.template.setSiteRef](./landing-template-set-site-ref.md) and [landing.template.setLandingRef](./landing-template-set-landing-ref.md) methods set the links between pages and areas for a site and an individual page.

## Continue Learning

- Retrieve a list of templates — [{#T}](./landing-template-get-list.md)
- Retrieve area links for a page or site — [{#T}](./landing-template-get-landing-ref.md), [{#T}](./landing-template-get-site-ref.md)
- Set area links for a page or site — [{#T}](./landing-template-set-landing-ref.md), [{#T}](./landing-template-set-site-ref.md)
