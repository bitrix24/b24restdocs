# Object Fields Template View

#|
|| **Field** | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`](../../data-types.md) | Template identifier | Yes | No ||
|| **ACTIVE**
[`string`](../../data-types.md) | Template activity

Possible values:
`Y` — template is active
`N` — template is inactive | Yes | No ||
|| **AREA_COUNT**
[`integer`](../../data-types.md) | Number of additional areas in the template, besides the main area `#CONTENT#` | Yes | No ||
|| **SORT**
[`integer`](../../data-types.md) | Template sorting order | Yes | No ||
|| **TITLE**
[`string`](../../data-types.md) | Template title. For system templates, the localized title is usually returned | Yes | No ||
|| **XML_ID**
[`string`](../../data-types.md) | External code of the template | Yes | No ||
|| **CONTENT**
[`string`](../../data-types.md) | HTML markup of the template with area macros, such as `#CONTENT#` and `#AREA_1#` | Yes | No ||
|| **CREATED_BY_ID**
[`integer`](../../data-types.md) | Identifier of the user who created the template. Only the identifier is returned via REST | Yes | No ||
|| **MODIFIED_BY_ID**
[`integer`](../../data-types.md) | Identifier of the user who last modified the template. Only the identifier is returned via REST | Yes | No ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Date and time of template creation | Yes | No ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Date and time of the last modification of the template | Yes | No ||
|#

In the method [landing.template.getlist](./landing-template-get-list.md), only simple fields of the template can be used.

For the author and last editor, only identifiers are available in the fields `CREATED_BY_ID` and `MODIFIED_BY_ID`.