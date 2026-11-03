# Estimate Fields in REST 3.0

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These fields belong to REST 3.0 methods. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

Estimate fields are returned by the [crm.quote.get](./crm-entity-get-rest-v3.md) and [crm.quote.list](./crm-entity-list-rest-v3.md) methods and accepted by the [crm.quote.add](./crm-entity-add-rest-v3.md) and [crm.quote.update](./crm-entity-update-rest-v3.md) methods. The list of fields with their attributes is returned by the [crm.quote.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

Some fields exist in two forms: as an ID and as a related object, for example, `assignedById` and `assignedBy`. The ID can be passed when writing data, while the related object can only be requested in `select`.

## Which Fields the Methods Accept and Return {#methods}

#|
|| **Method** | **Accepts** | **Returns** ||
|| [crm.quote.add](./crm-entity-add-rest-v3.md) | Fields of the [Estimate Object](#item) table that can be edited, and custom fields. There are no required fields: an estimate is created even with an empty `fields` object | ID of the created estimate `id` ||
|| [crm.quote.update](./crm-entity-update-rest-v3.md) | Estimate `id` and the same fields as `add`. Fields not included in `fields` keep their current values | Success flag `true` ||
|| [crm.quote.get](./crm-entity-get-rest-v3.md) | Estimate `id` and `select` | Without `select`, only `id`. With `select`, only the listed fields; `id` is not added automatically. A whole related object, for example, `assignedBy`, returns all of its fields, while a dot-notation path, for example, `assignedBy.firstName`, returns only the selected ones. For details, see the [Related Objects](#relations) section ||
|| [crm.quote.list](./crm-entity-list-rest-v3.md) | `select`, `filter`, `order`, `pagination` | Without `select`, only the `id` of each estimate. Fields and related objects are requested in `select` the same way as in `get` ||
|#

The `add` and `update` methods do not accept fields marked "read-only" and return the `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION` error with the message 'Field "personTypeId" is read-only.' Related objects, for example, `deal` and `stage`, are also read-only: to write data, pass the ID — `dealId`, `stageId`.

The method validates fields with values from reference lists when writing data. If the value is not in the reference list, the method returns the error 'Field value is not one of the possible values'. For the stage and currency, the error message lists the allowed values.

Boolean fields accept and return `true` or `false`. The values `Y`, `N`, `1`, and `0` are not accepted: the method returns the error 'This request requires data type "bool" in field "opened"'.

Fields of the `date` type accept and return a date in the `YYYY-MM-DD` format, for example, `2026-12-01`. Such a field does not accept a date with time or a date in the `01.12.2026` format. Fields of the `datetime` type are returned in ISO 8601 format, for example, `2026-11-03T14:48:50+03:00`. In a filter by a `datetime` field, also pass a date with time: the method does not accept the value `2026-11-03`.

You can filter and sort only by the fields whose description says so. For other fields, the `list` method returns an error stating that the field 'requires attribute "Filterable"' or '"Sortable"'. Dot notation lets you filter by fields of linked CRM items and of the `lastCommunication` and `utm` objects, for example, `["deal.title", "=", "Equipment supply"]`. Filtering by fields of employees and stage, for example, `assignedBy.id` or `stage.semanticId`, is not supported: the method returns the `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION` error. The filter format is described on the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) page and in the [REST API 3.0 Overview](../../../rest-v3.md#filter) article.

{% note warning "" %}

The `movedTime`, `movedById`, `movedBy`, `stageSemanticId`, and `stageSemantic` fields are present in the `crm.quote.field.list` response, but the `get` and `list` methods do not accept them. For such a field in `select`, `filter`, or `order`, the method returns the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error. You can retrieve the stage group from the `semanticId` field of the stage object: `"select": ["stage.semanticId"]`.

{% endnote %}

## Estimate Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../../data-types.md) | Estimate subject, up to 255 characters. If the subject is not passed on creation, Bitrix24 sets it to `Estimate #<id>`.

Can be edited, filtered, and sorted ||
|| **actualTime**
[```date | null```](../../../data-types.md) | Date until which the estimate is valid.

Can be edited, filtered, and sorted ||
|| **content**
[```string | null```](../../../data-types.md) | Estimate content.

Can be edited, filtered, and sorted ||
|| **terms**
[```string | null```](../../../data-types.md) | Estimate terms.

Can be edited, filtered, and sorted ||
|| **quoteNumber**
[```string | null```](../../../data-types.md) | Estimate number, up to 100 characters, for example, `F1-001`.

Can be edited, filtered, and sorted ||
|| **dealId**
[```integer | null```](../../../data-types.md) | ID of the deal the estimate belongs to. Deals are returned by the [crm.deal.list](./crm-entity-list-rest-v3.md) method. The method checks that the deal exists.

Can be edited, filtered, and sorted ||
|| **deal**
[```object | null```](#crm-item) | Deal. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **leadId**
[```integer | null```](../../../data-types.md) | ID of the lead the estimate belongs to. Leads are returned by the [crm.lead.list](./crm-entity-list-rest-v3.md) method. The method checks that the lead exists.

Can be edited, filtered, and sorted ||
|| **lead**
[```object | null```](#crm-item) | Lead. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **locationId**
[```integer | null```](../../../data-types.md) | Client location ID. A system field. The method checks that the location exists.

Can be edited, filtered, and sorted ||
|| **personTypeId**
[`string`](../../../data-types.md) | Client type. Bitrix24 determines it by the estimate client. Possible values:
- `CONTACT` — individual
- `COMPANY` — legal entity, if a company is specified in the estimate

Read-only. Can be filtered and sorted ||
|| **personType**
[`object`](#person-type) | Client type with its name. An object of type [client type](#person-type).

Read-only. Requested in `select` ||
|| **id**
[`integer`](../../../data-types.md) | Estimate ID. Assigned on creation.

Read-only. Can be filtered and sorted ||
|| **createdTime**
[`datetime`](../../../data-types.md) | Date and time the estimate was created.

Read-only. Can be filtered and sorted ||
|| **createdById**
[`integer`](../../../data-types.md) | ID of the user who created the estimate.

Read-only. Can be filtered and sorted ||
|| **createdBy**
[`object`](#employee) | User who created the estimate. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **updatedTime**
[`datetime`](../../../data-types.md) | Date and time the estimate was last updated.

Read-only. Can be filtered and sorted ||
|| **updatedById**
[`integer`](../../../data-types.md) | ID of the user who last updated the estimate.

Read-only. Can be filtered and sorted ||
|| **updatedBy**
[`object`](#employee) | User who last updated the estimate. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **lastActivityTime**
[`datetime`](../../../data-types.md) | Date and time of the last activity in the estimate timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityById**
[`integer`](../../../data-types.md) | ID of the author of the last activity in the timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityBy**
[`object`](#employee) | Author of the last activity in the timeline. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **assignedById**
[`integer`](../../../data-types.md) | ID of the person responsible for the estimate. User IDs are returned by the [user.get](../../../user/user-get.md) method. If the user does not exist, the method returns the error 'Field value is not one of the possible values'.

Can be edited, filtered, and sorted ||
|| **assignedBy**
[`object`](#employee) | Responsible person. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **opened**
[`boolean`](../../../data-types.md) | Whether the estimate is available to all users: `true` — yes, `false` — no.

Can be edited, filtered, and sorted ||
|| **comments**
[```string | null```](../../../data-types.md) | Comment on the estimate. Supports BB codes, for example, `[b]text[/b]`.

Can be edited. Cannot be filtered or sorted ||
|| **webformId**
[```integer | null```](../../../data-types.md) | ID of the CRM form through which the estimate was created. If the estimate was not created by a form, `null`. The list of forms is returned by the [crm.form.list](../../form/crm-form-list.md) method.

Read-only. Can be filtered and sorted ||
|| **webform**
[```object | null```](#webform) | CRM form through which the estimate was created. An object of type [CRM form](#webform).

Read-only. Requested in `select` ||
|| **lastCommunication**
[`object`](#last-communication) | Dates of the last communications with the client on the estimate. An object of type [last communication](#last-communication).

Read-only. Requested in `select`. Can be filtered by object fields using dot notation, for example, `["lastCommunication.emailTime", ">=", "2026-11-01T00:00:00+03:00"]` ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ID. For an estimate, `7`.

Read-only ||
|| **canRead**
[`boolean`](../../../data-types.md) | Whether the current user can view the estimate: `true` — yes, `false` — no.

Read-only ||
|| **url**
[`string`](../../../data-types.md) | Relative link to the estimate card, for example, `/crm/type/7/details/147/`.

Read-only ||
|| **stageId**
[`string`](../../../data-types.md) | Estimate stage ID, for example, `DRAFT`, `SENT`, `APPROVED`. Estimates have a single pipeline. Estimate stages are returned by the [crm.status.list](../../status/crm-status-list.md) method with the `ENTITY_ID = QUOTE_STATUS` filter.

Can be edited, filtered, and sorted ||
|| **stage**
[`object`](#stage) | Estimate stage. An object of type [stage](#stage).

Read-only. Requested in `select` ||
|| **movedTime**
[`datetime`](../../../data-types.md) | Date and time of the last stage change.

Read-only. The field is present in the `field.list` response, but the `get` and `list` methods do not accept it and return the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error ||
|| **movedById**
[`integer`](../../../data-types.md) | ID of the user who last changed the stage.

Read-only. The field is present in the `field.list` response, but the `get` and `list` methods do not accept it and return the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error ||
|| **movedBy**
[`object`](#employee) | User who last changed the stage. An object of type [employee](#employee).

Read-only. The field is present in the `field.list` response, but the `get` and `list` methods do not accept it and return the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error ||
|| **closed**
[`boolean`](../../../data-types.md) | Whether the estimate is closed: `true` — yes, `false` — no.

Read-only. Can be filtered and sorted ||
|| **stageSemanticId**
[`string`](../../../data-types.md) | Stage group. Possible values:
- `P` — in progress
- `S` — successful
- `F` — unsuccessful

Read-only. The field is present in the `field.list` response, but the `get` and `list` methods do not accept it and return the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error. The stage group is returned by the `stage.semanticId` field ||
|| **stageSemantic**
[`object`](../../../data-types.md) | Stage group with its name: `id` and `name`. The reference list of groups is returned by the [crm.dictionary.stagesemantic.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Read-only. The field is present in the `field.list` response, but the `get` and `list` methods do not accept it and return the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error ||
|| **beginTime**
[`date`](../../../data-types.md) | Estimate issue date. If not passed on creation, Bitrix24 sets it to the creation date.

Can be edited, filtered, and sorted ||
|| **closeTime**
[`date`](../../../data-types.md) | Estimate due date. If not passed on creation, Bitrix24 sets it to seven days after the creation date.

Can be edited, filtered, and sorted ||
|| **contactId**
[```integer | null```](../../../data-types.md) | ID of the primary contact, which is the first one in the `contactsId` list. To set the primary contact, use the [crm.quote.setprimarycontact](./crm-entity-set-primary-contact-rest-v3.md) method.

Read-only. Can be filtered, cannot be sorted ||
|| **contact**
[```object | null```](#crm-item) | Primary contact. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **contactsId**
[`integer[]`](../../../data-types.md) | IDs of the estimate contacts. Contacts are returned by the [crm.contact.list](./crm-entity-list-rest-v3.md) method. The passed array replaces the current list of contacts.

Can be edited and filtered, cannot be sorted ||
|| **contacts**
[`object[]`](#crm-item) | Estimate contacts. An array of objects of type [CRM item](#crm-item). If there are no contacts, an empty array.

Read-only. Requested in `select` ||
|| **companyId**
[```integer | null```](../../../data-types.md) | Company ID. Companies are returned by the [crm.company.list](./crm-entity-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **company**
[```object | null```](#crm-item) | Company. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **myCompanyId**
[```integer | null```](../../../data-types.md) | ID of your company, on whose behalf the estimate is issued. The list of your companies is returned by the [crm.mycompany.list](./crm-entity-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **myCompany**
[```object | null```](#crm-item) | Your company. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **opportunity**
[`double`](../../../data-types.md) | Estimate amount, not less than `0`. The way the amount is calculated is set by the `isManualOpportunity` field.

Can be edited, filtered, and sorted ||
|| **taxValue**
[`double`](../../../data-types.md) | Tax amount. Calculated from the product rows.

Read-only. Can be filtered and sorted ||
|| **isManualOpportunity**
[`boolean`](../../../data-types.md) | Amount calculation mode: `true` — the amount is set manually, `false` — the amount is calculated from the product rows.

Can be edited, filtered, and sorted ||
|| **currencyId**
[`string`](../../../data-types.md) | Currency ID, for example, `EUR`. Currencies are returned by the [crm.currency.list](../../currency/crm-currency-list.md) method.

Can be edited, filtered, and sorted ||
|| **currency**
[`object`](#currency) | Currency. An object of type [currency](#currency).

Read-only. Requested in `select` ||
|| **utm**
[```object | null```](#utm) | UTM tags of the estimate. An object of type [UTM tags](#utm). If there are no tags, `null`.

Can be edited: pass an object with the tags you need, for example, `{"source": "google", "medium": "cpc"}`. Can be filtered by object fields using dot notation, for example, `["utm.source", "=", "google"]`, cannot be sorted ||
|| **trackingSourceId**
[```integer | null```](../../../data-types.md) | ID of the end-to-end analytics source.

Can be edited and filtered, cannot be sorted ||
|#

If Bitrix24 has a link configured in which the estimate is subordinate to another object type, the estimate gets the link fields `related<type code>Id` and `related<type code>`, for example, `relatedSmartProcess177Id`. The first one is the ID of the parent item; it can be edited and filtered. The second one is the parent item as an object of type [CRM item](#crm-item), read-only. The current list of fields is returned by the [crm.quote.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

## Related Objects {#relations}

A related object is requested in `select` by the field name without the `Id` suffix: `assignedBy`, `stage`, `personType`. The whole object name returns all of its fields, and a dot-notation path returns only the selected ones, for example, `["stage.name", "assignedBy.firstName"]`. The path supports one level of nesting.

A linked CRM item — `deal`, `lead`, `contact`, `company`, `myCompany` — returns only `id` when requested by its whole name. Request the other item fields using dot notation, for example, `["deal.title", "deal.url"]`.

If a link is not set, the object is returned as `null`, and an array of objects, for example, `contacts`, is returned as an empty array.

### Employee Object {#employee}

Fields `createdBy`, `updatedBy`, `lastActivityBy`, `assignedBy`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | User ID ||
|| **firstName**
[`string`](../../../data-types.md) | First name ||
|| **lastName**
[`string`](../../../data-types.md) | Last name ||
|| **personalGender**
[```string | null```](../../../data-types.md) | Gender: `M` — male, `F` — female. If the gender is not specified, `null` ||
|| **url**
[`string`](../../../data-types.md) | Relative link to the user profile, for example, `/company/personal/user/1/` ||
|| **personalPhoto**
[```string | null```](../../../data-types.md) | Link to the user photo. If there is no photo, `null` ||
|#

### CRM Item Object {#crm-item}

Fields `deal`, `lead`, `contact`, `contacts`, `company`, `myCompany`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Item ID ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ID, for example, `2` — deal, `3` — contact ||
|| **canRead**
[`boolean`](../../../data-types.md) | Whether the current user can view the item ||
|| **title**
[`string`](../../../data-types.md) | Item title. For a contact, the last name, first name, and middle name ||
|| **url**
[`string`](../../../data-types.md) | Relative link to the item card, for example, `/crm/deal/details/8987/` ||
|#

### Stage Object {#stage}

Field `stage`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Stage ID, for example, `DRAFT` ||
|| **name**
[`string`](../../../data-types.md) | Stage name ||
|| **sort**
[`integer`](../../../data-types.md) | Stage order ||
|| **color**
[`string`](../../../data-types.md) | Stage color in the `#RRGGBB` format ||
|| **semanticId**
[`string`](../../../data-types.md) | Stage group: `P` — in progress, `S` — successful, `F` — unsuccessful ||
|#

### Client Type Object {#person-type}

Field `personType`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Client type ID: `CONTACT` or `COMPANY` ||
|| **name**
[`string`](../../../data-types.md) | Client type name, for example, `Individual` ||
|#

### CRM Form Object {#webform}

Field `webform`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | CRM form ID ||
|| **name**
[`string`](../../../data-types.md) | CRM form name ||
|#

### Last Communication Object {#last-communication}

Field `lastCommunication`. If there has been no communication of a given kind, the corresponding field is `null`.

#|
|| **Name**
`type` | **Description** ||
|| **communicationTime**
[```datetime | null```](../../../data-types.md) | Date and time of the last communication of any kind ||
|| **callTime**
[```datetime | null```](../../../data-types.md) | Date and time of the last call ||
|| **emailTime**
[```datetime | null```](../../../data-types.md) | Date and time of the last email ||
|| **imolTime**
[```datetime | null```](../../../data-types.md) | Date and time of the last message in an open channel ||
|| **webformTime**
[```datetime | null```](../../../data-types.md) | Date and time the CRM form was last submitted ||
|#

### Currency Object {#currency}

Field `currency`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Currency code, for example, `EUR` ||
|| **fullName**
[`string`](../../../data-types.md) | Currency name ||
|| **formatString**
[`string`](../../../data-types.md) | Amount display template. The `#` character is replaced with the amount. The currency sign is returned as an HTML entity, for example, `&euro;#` ||
|| **decPoint**
[`string`](../../../data-types.md) | Decimal separator ||
|| **thousandsSep**
[`string`](../../../data-types.md) | Thousands separator. May be returned as an HTML entity, for example, `&nbsp;` ||
|| **decimals**
[`integer`](../../../data-types.md) | Number of decimal places ||
|| **hideZero**
[`boolean`](../../../data-types.md) | Whether to hide insignificant trailing zeros in the fractional part ||
|| **amount**
[`double`](../../../data-types.md) | Exchange rate relative to the base currency ||
|| **amountCount**
[`integer`](../../../data-types.md) | Number of currency units the exchange rate applies to ||
|| **base**
[`boolean`](../../../data-types.md) | Base currency: `true` — yes, `false` — no ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order ||
|| **numericCode**
[`string`](../../../data-types.md) | Numeric currency code, for example, `978` ||
|| **languageId**
[`string`](../../../data-types.md) | Language for which the name and format are set ||
|| **createdTime**
[```datetime | null```](../../../data-types.md) | Date and time the currency was created ||
|| **updatedTime**
[```datetime | null```](../../../data-types.md) | Date and time the currency was updated ||
|| **createdById**
[```integer | null```](../../../data-types.md) | ID of the user who created the currency ||
|| **updatedById**
[```integer | null```](../../../data-types.md) | ID of the user who updated the currency ||
|#

### UTM Tags Object {#utm}

Field `utm`. Each tag is a string of up to 255 characters. Tags that were not passed are returned as `null`.

#|
|| **Name**
`type` | **Description** ||
|| **source**
[```string | null```](../../../data-types.md) | Ad platform, `utm_source` ||
|| **medium**
[```string | null```](../../../data-types.md) | Traffic type, `utm_medium`, for example, `cpc` ||
|| **campaign**
[```string | null```](../../../data-types.md) | Ad campaign, `utm_campaign` ||
|| **content**
[```string | null```](../../../data-types.md) | Ad content, `utm_content` ||
|| **term**
[```string | null```](../../../data-types.md) | Keyword, `utm_term` ||
|#

## Custom Fields {#userfields}

Custom estimate fields are named `UF_CRM_<code>`, for example, `UF_CRM_6710EFCBAF22C`. They are passed and requested by name in the same way as standard fields. The list of custom fields is returned by the [crm.quote.field.custom.list](../fields/crm-entity-field-custom-list-rest-v3.md) method.

Each custom field has a paired `UF_CRM_…_data` field, which is read-only and requested in `select`. It contains the value in several representations:

- `value` — the value in the same format as in the field itself
- `rawValue` — the value in the storage format, for example, `100.5|EUR` for a "Money" field
- `formattedValue` — the value for display to the user: `100.50 €` for money, the employee name for a "Link to employee" field, the option text for a list
- `extra` — additional data: for money, `amount`, `currencyId`, and `currencyFullName`; for an employee, the fields of the [employee](#employee) object. For a string, date, and list, `null`

For a multiple custom field, `UF_CRM_…_data` is an array of such objects.

## Continue Learning

- [{#T}](./crm-entity-add-rest-v3.md)
- [{#T}](./crm-entity-update-rest-v3.md)
- [{#T}](./crm-entity-get-rest-v3.md)
- [{#T}](./crm-entity-list-rest-v3.md)
- [{#T}](../fields/crm-entity-field-list-rest-v3.md)
- [{#T}](../../../rest-v3.md)
