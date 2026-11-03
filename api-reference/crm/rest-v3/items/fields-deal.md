# Deal Fields in REST 3.0

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These fields belong to REST 3.0 methods. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

Deal fields are returned by the [crm.deal.get](./crm-entity-get-rest-v3.md) and [crm.deal.list](./crm-entity-list-rest-v3.md) methods and accepted by the [crm.deal.add](./crm-entity-add-rest-v3.md) and [crm.deal.update](./crm-entity-update-rest-v3.md) methods. The list of fields with their attributes is returned by the [crm.deal.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

Some fields exist in two forms: as an ID and as a related object, for example, `assignedById` and `assignedBy`. The ID can be passed when writing data, while the related object can only be requested in `select`.

## Which Fields the Methods Accept and Return {#methods}

#|
|| **Method** | **Accepts** | **Returns** ||
|| [crm.deal.add](./crm-entity-add-rest-v3.md) | Fields of the [Deal Object](#item) table that can be edited, and custom fields. There are no required fields: a deal is created even with an empty `fields` object | ID of the created deal `id` ||
|| [crm.deal.update](./crm-entity-update-rest-v3.md) | Deal `id` and the same fields as `add`. Fields not included in `fields` keep their current values | Success flag `true` ||
|| [crm.deal.get](./crm-entity-get-rest-v3.md) | Deal `id` and `select` | Without `select`, only `id`. With `select`, only the listed fields; `id` is not added automatically. A whole related object, for example, `assignedBy`, returns all of its fields, while a dot-notation path, for example, `assignedBy.firstName`, returns only the selected ones. For details, see the [Related Objects](#relations) section ||
|| [crm.deal.list](./crm-entity-list-rest-v3.md) | `select`, `filter`, `order`, `pagination` | Without `select`, only the `id` of each deal. Fields and related objects are requested in `select` the same way as in `get` ||
|#

The `add` and `update` methods do not accept fields marked "read-only" and return the `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION` error with the message 'Field "contactId" is read-only.' Related objects, for example, `company` and `stage`, are also read-only: to write data, pass the ID — `companyId`, `stageId`.

The method validates fields with values from reference lists when writing data. If the value is not in the reference list, the method returns the error 'Field value is not one of the possible values'. For the stage, deal type, source, and currency, the error message lists the allowed values.

Boolean fields accept and return `true` or `false`. The values `Y`, `N`, `1`, and `0` are not accepted: the method returns the error 'This request requires data type "bool" in field "opened"'.

Fields of the `date` type accept and return a date in the `YYYY-MM-DD` format, for example, `2026-11-03`. Such a field does not accept a date with time or a date in the `03.11.2026` format. Fields of the `datetime` type are returned in ISO 8601 format, for example, `2026-11-03T14:47:23+03:00`. In a filter by a `datetime` field, also pass a date with time: the method does not accept the value `2026-11-03`.

You can filter and sort only by the fields whose description says so. For other fields, the `list` method returns an error stating that the field 'requires attribute "Filterable"' or '"Sortable"'. Dot notation lets you filter by fields of linked CRM items and of the `lastCommunication` and `utm` objects, for example, `["company.title", "=", "Müller GmbH"]` or `["utm.source", "=", "google"]`. Filtering by fields of employees, stage, pipeline, deal type, and source, for example, `assignedBy.id` or `stage.semanticId`, is not supported: the method returns the `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION` error. The filter format is described on the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) page and in the [REST API 3.0 Overview](../../../rest-v3.md#filter) article.

## Deal Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../../data-types.md) | Deal title, up to 255 characters.

Can be edited, filtered, and sorted ||
|| **typeId**
[```string | null```](../../../data-types.md) | Deal type ID, for example, `SALE`. Deal types are returned by the [crm.dictionary.dealtype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **type**
[```object | null```](#deal-type) | Deal type. An object of type [deal type](#deal-type).

Read-only. Requested in `select` ||
|| **quoteId**
[```integer | null```](../../../data-types.md) | ID of the estimate from which the deal was created.

Read-only. Can be filtered and sorted ||
|| **quote**
[```object | null```](#crm-item) | Estimate from which the deal was created. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **leadId**
[```integer | null```](../../../data-types.md) | ID of the lead from which the deal was created.

Read-only. Can be filtered and sorted ||
|| **lead**
[```object | null```](#crm-item) | Lead from which the deal was created. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **previousStageId**
[`string`](../../../data-types.md) | ID of the stage the deal was at before the current one, for example, `EXECUTING`. If the stage has not changed, an empty string.

Read-only. Can be filtered and sorted ||
|| **previousStage**
[```object | null```](#stage) | Previous stage. An object of type [stage](#stage). If the stage has not changed, `null`.

Read-only. Requested in `select` ||
|| **locationId**
[```integer | null```](../../../data-types.md) | Client location ID. A system field. The method checks that the location exists.

Can be edited. Cannot be filtered or sorted ||
|| **isNew**
[`boolean`](../../../data-types.md) | Whether the deal is at the first stage of the pipeline: `true` — yes, `false` — no.

Read-only. Can be filtered and sorted ||
|| **isRepeatedApproach**
[`boolean`](../../../data-types.md) | Whether the deal is a repeated inquiry from the client: `true` — yes, `false` — no.

Read-only. Can be filtered and sorted ||
|| **isReturnCustomer**
[`boolean`](../../../data-types.md) | Whether this is a repeat deal with the client: `true` — yes, `false` — no.

Read-only. Can be filtered and sorted ||
|| **id**
[`integer`](../../../data-types.md) | Deal ID. Assigned on creation.

Read-only. Can be filtered and sorted ||
|| **createdTime**
[`datetime`](../../../data-types.md) | Date and time the deal was created.

Read-only. Can be filtered and sorted ||
|| **createdById**
[`integer`](../../../data-types.md) | ID of the user who created the deal.

Read-only. Can be filtered and sorted ||
|| **createdBy**
[`object`](#employee) | User who created the deal. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **updatedTime**
[`datetime`](../../../data-types.md) | Date and time the deal was last updated.

Read-only. Can be filtered and sorted ||
|| **updatedById**
[`integer`](../../../data-types.md) | ID of the user who last updated the deal.

Read-only. Can be filtered and sorted ||
|| **updatedBy**
[`object`](#employee) | User who last updated the deal. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **lastActivityTime**
[`datetime`](../../../data-types.md) | Date and time of the last activity in the deal timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityById**
[`integer`](../../../data-types.md) | ID of the author of the last activity in the timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityBy**
[`object`](#employee) | Author of the last activity in the timeline. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **assignedById**
[`integer`](../../../data-types.md) | ID of the person responsible for the deal. User IDs are returned by the [user.get](../../../user/user-get.md) method. If the user does not exist, the method returns the error 'Field value is not one of the possible values'.

Can be edited, filtered, and sorted ||
|| **assignedBy**
[`object`](#employee) | Responsible person. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **opened**
[`boolean`](../../../data-types.md) | Whether the deal is available to all users: `true` — yes, `false` — no.

Can be edited, filtered, and sorted ||
|| **comments**
[```string | null```](../../../data-types.md) | Comment on the deal. Supports BB codes, for example, `[b]text[/b]`.

Can be edited. Cannot be filtered or sorted ||
|| **originatorId**
[```string | null```](../../../data-types.md) | ID of the external system the deal came from. Use together with `originId` to link the deal to an external source.

Can be edited, filtered, and sorted ||
|| **originId**
[```string | null```](../../../data-types.md) | Deal ID in the external system.

Can be edited, filtered, and sorted ||
|| **webformId**
[```integer | null```](../../../data-types.md) | ID of the CRM form through which the deal was created. If the deal was not created by a form, `null`. The list of forms is returned by the [crm.form.list](../../form/crm-form-list.md) method.

Read-only. Can be filtered and sorted ||
|| **webform**
[```object | null```](#webform) | CRM form through which the deal was created. An object of type [CRM form](#webform).

Read-only. Requested in `select` ||
|| **lastCommunication**
[`object`](#last-communication) | Dates of the last communications with the client on the deal. An object of type [last communication](#last-communication).

Read-only. Requested in `select`. Can be filtered by object fields using dot notation, for example, `["lastCommunication.callTime", ">=", "2026-11-01T00:00:00+03:00"]` ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ID. For a deal, `2`.

Read-only ||
|| **canRead**
[`boolean`](../../../data-types.md) | Whether the current user can view the deal: `true` — yes, `false` — no.

Read-only ||
|| **url**
[`string`](../../../data-types.md) | Relative link to the deal card, for example, `/crm/deal/details/8983/?category_id=9`.

Read-only ||
|| **stageId**
[`string`](../../../data-types.md) | Stage ID. In the general pipeline, the ID has no prefix, for example, `NEW`; in other pipelines, it has the pipeline prefix, for example, `C9:NEW`. The stage must belong to the deal's pipeline. Pipeline stages are returned by the [crm.deal.category.stage.list](../categories/crm-entity-category-stage-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **stage**
[`object`](#stage) | Deal stage. An object of type [stage](#stage).

Read-only. Requested in `select` ||
|| **movedTime**
[`datetime`](../../../data-types.md) | Date and time of the last stage change.

Read-only. Can be filtered and sorted ||
|| **movedById**
[`integer`](../../../data-types.md) | ID of the user who last changed the stage.

Read-only. Can be filtered and sorted ||
|| **movedBy**
[`object`](#employee) | User who last changed the stage. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **closed**
[`boolean`](../../../data-types.md) | Whether the deal is closed: `true` — yes, `false` — no.

Read-only. Can be filtered and sorted ||
|| **stageSemanticId**
[`string`](../../../data-types.md) | Stage group. Possible values:
- `P` — in progress
- `S` — successful
- `F` — unsuccessful

Read-only. Can be filtered and sorted ||
|| **stageSemantic**
[`object`](#stage-semantic) | Stage group with its name. An object of type [stage group](#stage-semantic).

Read-only. Requested in `select` ||
|| **beginTime**
[`date`](../../../data-types.md) | Start date.

Can be edited, filtered, and sorted ||
|| **closeTime**
[`date`](../../../data-types.md) | Close date.

Can be edited, filtered, and sorted ||
|| **categoryId**
[`integer`](../../../data-types.md) | Pipeline ID. The general pipeline is `0`. If not passed on creation, the deal is placed in the general pipeline. Deal pipelines are returned by the [crm.deal.category.list](../categories/crm-entity-category-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **category**
[`object`](#category) | Deal pipeline. An object of type [pipeline](#category).

Read-only. Requested in `select` ||
|| **observersId**
[`integer[]`](../../../data-types.md) | Observer IDs. User IDs are returned by the [user.get](../../../user/user-get.md) method.

Can be edited and filtered, cannot be sorted. In a filter, `["observersId", "=", 1]` finds deals that have user `1` among their observers ||
|| **observers**
[`object[]`](#employee) | Observers. An array of objects of type [employee](#employee). If there are no observers, an empty array.

Read-only. Requested in `select` ||
|| **contactId**
[```integer | null```](../../../data-types.md) | ID of the primary contact, which is the first one in the `contactsId` list. To set the primary contact, use the [crm.deal.setprimarycontact](./crm-entity-set-primary-contact-rest-v3.md) method.

Read-only. Can be filtered, cannot be sorted ||
|| **contact**
[```object | null```](#crm-item) | Primary contact. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **contactsId**
[`integer[]`](../../../data-types.md) | IDs of the deal contacts. Contacts are returned by the [crm.contact.list](./crm-entity-list-rest-v3.md) method. The passed array replaces the current list of contacts.

Can be edited and filtered, cannot be sorted ||
|| **contacts**
[`object[]`](#crm-item) | Deal contacts. An array of objects of type [CRM item](#crm-item). If there are no contacts, an empty array.

Read-only. Requested in `select` ||
|| **companyId**
[```integer | null```](../../../data-types.md) | Company ID. Companies are returned by the [crm.company.list](./crm-entity-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **company**
[```object | null```](#crm-item) | Company. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **myCompanyId**
[```integer | null```](../../../data-types.md) | ID of your company, on whose behalf you work with the client. The list of your companies is returned by the [crm.mycompany.list](./crm-entity-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **myCompany**
[```object | null```](#crm-item) | Your company. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **sourceId**
[```string | null```](../../../data-types.md) | Source ID, for example, `CALL`. Sources are returned by the [crm.dictionary.source.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **source**
[```object | null```](#dictionary) | Source. An object of type [reference list value](#dictionary).

Read-only. Requested in `select` ||
|| **sourceDescription**
[```string | null```](../../../data-types.md) | Additional information about the source.

Can be edited and filtered, cannot be sorted ||
|| **opportunity**
[`double`](../../../data-types.md) | Deal amount, not less than `0`. The way the amount is calculated is set by the `isManualOpportunity` field.

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
[```object | null```](#utm) | UTM tags of the deal. An object of type [UTM tags](#utm). If there are no tags, `null`.

Can be edited: pass an object with the tags you need, for example, `{"source": "google", "medium": "cpc"}`. Can be filtered by object fields using dot notation, for example, `["utm.source", "=", "google"]`, cannot be sorted ||
|| **trackingSourceId**
[```integer | null```](../../../data-types.md) | ID of the end-to-end analytics source.

Can be edited and filtered, cannot be sorted ||
|| **related&lt;type code&gt;Id**
[```integer | null```](../../../data-types.md) | ID of the parent item of another object type, for example, `relatedSmartProcess177Id`. For details, see the [Fields Linking to Other Types](#related) section.

Can be edited and filtered, cannot be sorted ||
|| **related&lt;type code&gt;**
[```object | null```](#crm-item) | Parent item of another object type, for example, `relatedSmartProcess177`. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|#

### Fields Linking to Other Types {#related}

The `related<type code>Id` and `related<type code>` fields appear when Bitrix24 has a link configured in which the deal is subordinate to another object type, for example, an SPA. For each parent type, the deal has a pair of fields:

- `related<type code>Id` — ID of the parent item, for example, `relatedSmartProcess177Id` for the SPA with `entityTypeId = 177`
- `related<type code>` — the parent item as an object of type [CRM item](#crm-item), for example, `relatedSmartProcess177`

The field title in the `crm.deal.field.list` response matches the name of the parent type, for example, "Equipment purchase". The method checks that an item with the passed ID exists. If there is no such item, the method returns the error 'Field value is not one of the possible values'.

The set of these fields depends on the links configured in Bitrix24. The current list is returned by the [crm.deal.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

## Related Objects {#relations}

A related object is requested in `select` by the field name without the `Id` suffix: `assignedBy`, `stage`, `category`. The whole object name returns all of its fields, and a dot-notation path returns only the selected ones, for example, `["stage.name", "assignedBy.firstName"]`. The path supports one level of nesting.

A linked CRM item — `contact`, `company`, `myCompany`, `lead`, `quote`, `related<type code>` — returns only `id` when requested by its whole name. Request the other item fields using dot notation, for example, `["company.title", "company.url"]`.

If a link is not set, the object is returned as `null`, and an array of objects, for example, `observers` or `contacts`, is returned as an empty array.

### Employee Object {#employee}

Fields `createdBy`, `updatedBy`, `lastActivityBy`, `assignedBy`, `movedBy`, `observers`.

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

Fields `contact`, `contacts`, `company`, `myCompany`, `lead`, `quote`, `related<type code>`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Item ID ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ID, for example, `3` — contact, `4` — company ||
|| **canRead**
[`boolean`](../../../data-types.md) | Whether the current user can view the item ||
|| **title**
[`string`](../../../data-types.md) | Item title. For a contact, the last name, first name, and middle name ||
|| **url**
[`string`](../../../data-types.md) | Relative link to the item card, for example, `/crm/company/details/3/` ||
|#

### Stage Object {#stage}

Fields `stage` and `previousStage`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Stage ID, for example, `C9:NEW` ||
|| **name**
[`string`](../../../data-types.md) | Stage name ||
|| **sort**
[`integer`](../../../data-types.md) | Stage order in the pipeline ||
|| **color**
[`string`](../../../data-types.md) | Stage color in the `#RRGGBB` format ||
|| **semanticId**
[`string`](../../../data-types.md) | Stage group: `P` — in progress, `S` — successful, `F` — unsuccessful ||
|#

### Stage Group Object {#stage-semantic}

Field `stageSemantic`. The reference list of groups is returned by the [crm.dictionary.stagesemantic.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Group ID: `P`, `S`, or `F` ||
|| **name**
[`string`](../../../data-types.md) | Group name, for example, `Stage group "Deal in progress"` ||
|#

### Pipeline Object {#category}

Field `category`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Pipeline ID ||
|| **name**
[`string`](../../../data-types.md) | Pipeline name ||
|| **sort**
[`integer`](../../../data-types.md) | Pipeline sort order ||
|| **isDefault**
[`boolean`](../../../data-types.md) | Default pipeline: `true` — yes, `false` — no ||
|#

### Deal Type Object {#deal-type}

Field `type`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Type ID, for example, `SALE` ||
|| **name**
[`string`](../../../data-types.md) | Type name, for example, `Sale` ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order ||
|| **isSystem**
[`boolean`](../../../data-types.md) | System type: `true` — yes, `false` — no ||
|#

### Reference List Value Object {#dictionary}

Field `source`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Value ID, for example, `CALL` ||
|| **name**
[`string`](../../../data-types.md) | Value name, for example, `Call` ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order ||
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
[```string | null```](../../../data-types.md) | Ad platform, the `utm_source` tag, for example, `google` ||
|| **medium**
[```string | null```](../../../data-types.md) | Traffic type, the `utm_medium` tag, for example, `cpc` ||
|| **campaign**
[```string | null```](../../../data-types.md) | Ad campaign, the `utm_campaign` tag ||
|| **content**
[```string | null```](../../../data-types.md) | Ad content, the `utm_content` tag ||
|| **term**
[```string | null```](../../../data-types.md) | Keyword, the `utm_term` tag ||
|#

## Custom Fields {#userfields}

Custom deal fields are named `UF_CRM_<code>`, for example, `UF_CRM_1687529546`. They are passed and requested by name in the same way as standard fields. The list of custom fields is returned by the [crm.deal.field.custom.list](../fields/crm-entity-field-custom-list-rest-v3.md) method.

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
