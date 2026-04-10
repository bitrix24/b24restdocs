# Template View Fields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Template view fields are used in the method [landing.template.getlist](./landing-template-get-list.md). This method only supports simple template fields. Fields with a dot in `select` and `filter`, such as `CREATED_BY.NAME`, are not supported.

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

Through [landing.template.getlist](./landing-template-get-list.md), you can select, filter, sort, and group only the fields from the table above.

## Continue Learning

- [{#T}](./landing-template-get-landing-ref.md) 
- [{#T}](./landing-template-get-site-ref.md) 
- [{#T}](./landing-template-get-list.md) 
- [{#T}](./landing-template-set-landing-ref.md) 
- [{#T}](./landing-template-set-site-ref.md) 