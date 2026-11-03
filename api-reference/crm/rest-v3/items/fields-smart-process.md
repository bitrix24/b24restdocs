# SPA Fields in REST 3.0

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These fields belong to REST 3.0 methods. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

SPA item fields are returned by the [`crm.smartprocess<entityTypeId>.get`](./crm-entity-get-rest-v3.md) and [`crm.smartprocess<entityTypeId>.list`](./crm-entity-list-rest-v3.md) methods and accepted by the [`crm.smartprocess<entityTypeId>.add`](./crm-entity-add-rest-v3.md) and [`crm.smartprocess<entityTypeId>.update`](./crm-entity-update-rest-v3.md) methods. The method name includes the SPA type ID `entityTypeId`, for example, `crm.smartprocess1038.get`. Type IDs and settings are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method.

The set of fields depends on the SPA type settings: without pipelines, an item has no `categoryId`, and without stages, it has no `stageId` and other stage fields. For such fields, the [SPA Object](#item) table specifies which type setting is required. The list of fields of a specific type is returned by the [`crm.smartprocess<entityTypeId>.field.list`](../fields/crm-entity-field-list-rest-v3.md) method.

Some fields exist in two forms: as an ID and as a related object, for example, `assignedById` and `assignedBy`. The ID can be passed when writing data, while the related object can only be requested in `select`.

## Which Fields the Methods Accept and Return {#methods}

#|
|| **Method** | **Accepts** | **Returns** ||
|| [`crm.smartprocess<entityTypeId>.add`](./crm-entity-add-rest-v3.md) | Fields of the [SPA Object](#item) table that can be edited, and custom fields. There are no required fields: an item is created even with an empty `fields` object | ID of the created item `id` ||
|| [`crm.smartprocess<entityTypeId>.update`](./crm-entity-update-rest-v3.md) | Item `id` and the same fields as `add`. Fields not included in `fields` keep their current values | Success flag `true` ||
|| [`crm.smartprocess<entityTypeId>.get`](./crm-entity-get-rest-v3.md) | Item `id` and `select` | Without `select`, only `id`. With `select`, only the listed fields; `id` is not added automatically. A whole related object, for example, `assignedBy`, returns all of its fields, while a dot-notation path, for example, `assignedBy.firstName`, returns only the selected ones. For details, see the [Related Objects](#relations) section ||
|| [`crm.smartprocess<entityTypeId>.list`](./crm-entity-list-rest-v3.md) | `select`, `filter`, `order`, `pagination` | Without `select`, only the `id` of each item. Fields and related objects are requested in `select` the same way as in `get` ||
|#

The `add` and `update` methods do not accept fields marked "read-only" and return the `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION` error with the message 'Field "contactId" is read-only.' Related objects, for example, `company` and `stage`, are also read-only: to write data, pass the ID — `companyId`, `stageId`.

The method validates fields with values from reference lists when writing data. If the value is not in the reference list, the method returns the error 'Field value is not one of the possible values'. For the stage, source, and currency, the error message lists the allowed values.

Boolean fields accept and return `true` or `false`. The values `Y`, `N`, `1`, and `0` are not accepted: the method returns the error 'This request requires data type "bool" in field "opened"'.

Fields of the `date` type accept and return a date in the `YYYY-MM-DD` format, for example, `2026-11-03`. Such a field does not accept a date with time or a date in the `03.11.2026` format. Fields of the `datetime` type are returned in ISO 8601 format, for example, `2026-11-03T14:47:23+03:00`. In a filter by a `datetime` field, also pass a date with time: the method does not accept the value `2026-11-03`.

You can filter and sort only by the fields whose description says so. For other fields, the `list` method returns an error stating that the field 'requires attribute "Filterable"' or '"Sortable"'. Dot notation lets you filter by fields of linked CRM items and of the `lastCommunication` object, for example, `["company.title", "=", "Müller GmbH"]`. Filtering by fields of employees, stage, pipeline, and source, for example, `assignedBy.id` or `stage.semanticId`, is not supported: the method returns the `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION` error. The filter format is described on the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) page and in the [REST API 3.0 Overview](../../../rest-v3.md#filter) article.

{% note warning "" %}

The `closed`, `stageSemanticId`, and `stageSemantic` fields are present in the `crm.smartprocess<entityTypeId>.field.list` response, but the `get` and `list` methods do not accept them. For such a field in `select`, `filter`, or `order`, the method returns the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error. The stage group can be retrieved from the `semanticId` field of the stage object: `"select": ["stage.semanticId"]`.

{% endnote %}

## SPA Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../../data-types.md) | Item title, up to 255 characters. If the title is not passed on creation, Bitrix24 sets a title in the `<type name> #<id>` format.

Can be edited, filtered, and sorted ||
|| **xmlId**
[`string`](../../../data-types.md) | External code of the item, up to 255 characters. Use it to link the item to a record in an external system.

Can be edited, filtered, and sorted ||
|| **id**
[`integer`](../../../data-types.md) | Item ID. Assigned on creation.

Read-only. Can be filtered and sorted ||
|| **createdTime**
[`datetime`](../../../data-types.md) | Date and time the item was created.

Read-only. Can be filtered and sorted ||
|| **createdById**
[`integer`](../../../data-types.md) | ID of the user who created the item.

Read-only. Can be filtered and sorted ||
|| **createdBy**
[`object`](#employee) | User who created the item. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **updatedTime**
[`datetime`](../../../data-types.md) | Date and time the item was last updated.

Read-only. Can be filtered and sorted ||
|| **updatedById**
[`integer`](../../../data-types.md) | ID of the user who last updated the item.

Read-only. Can be filtered and sorted ||
|| **updatedBy**
[`object`](#employee) | User who last updated the item. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **lastActivityTime**
[`datetime`](../../../data-types.md) | Date and time of the last activity in the item timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityById**
[`integer`](../../../data-types.md) | ID of the author of the last activity in the timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityBy**
[`object`](#employee) | Author of the last activity in the timeline. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **assignedById**
[`integer`](../../../data-types.md) | ID of the person responsible for the item. User IDs are returned by the [user.get](../../../user/user-get.md) method. If the user does not exist, the method returns the error 'Field value is not one of the possible values'.

Can be edited, filtered, and sorted ||
|| **assignedBy**
[`object`](#employee) | Responsible person. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **opened**
[`boolean`](../../../data-types.md) | Whether the item is available to all users: `true` — yes, `false` — no.

Can be edited, filtered, and sorted ||
|| **webformId**
[```integer | null```](../../../data-types.md) | ID of the CRM form through which the item was created. If the item was not created by a form, `null`. The list of forms is returned by the [crm.form.list](../../form/crm-form-list.md) method.

Read-only. Can be filtered and sorted ||
|| **webform**
[```object | null```](#webform) | CRM form through which the item was created. An object of type [CRM form](#webform).

Read-only. Requested in `select` ||
|| **lastCommunication**
[`object`](#last-communication) | Dates of the last communications with the client on the item. An object of type [last communication](#last-communication).

Read-only. Requested in `select`. Can be filtered by object fields using dot notation, for example, `["lastCommunication.callTime", ">=", "2026-11-01T00:00:00+03:00"]` ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | SPA type ID, for example, `1038`.

Read-only ||
|| **canRead**
[`boolean`](../../../data-types.md) | Whether the current user can view the item: `true` — yes, `false` — no.

Read-only ||
|| **url**
[`string`](../../../data-types.md) | Relative link to the item card, for example, `/crm/type/1038/details/29/?categoryId=69`.

Read-only ||
|| **stageId**
[`string`](../../../data-types.md) | Stage ID, for example, `DT1038_69:NEW`. The stage must belong to the item's pipeline. Pipeline stages are returned by the [`crm.smartprocess<entityTypeId>.category.stage.list`](../categories/crm-entity-category-stage-list-rest-v3.md) method.

Can be edited, filtered, and sorted.

Available if stages are enabled in the type settings: `isStagesEnabled` ||
|| **stage**
[`object`](#stage) | Item stage. An object of type [stage](#stage).

Read-only. Requested in `select`.

Available if stages are enabled in the type settings: `isStagesEnabled` ||
|| **movedTime**
[`datetime`](../../../data-types.md) | Date and time of the last stage change.

Read-only. Can be filtered and sorted.

Available if stages are enabled in the type settings: `isStagesEnabled` ||
|| **movedById**
[`integer`](../../../data-types.md) | ID of the user who last changed the stage.

Read-only. Can be filtered and sorted.

Available if stages are enabled in the type settings: `isStagesEnabled` ||
|| **movedBy**
[`object`](#employee) | User who last changed the stage. An object of type [employee](#employee).

Read-only. Requested in `select`.

Available if stages are enabled in the type settings: `isStagesEnabled` ||
|| **closed**
[`boolean`](../../../data-types.md) | Whether the item is closed: `true` — yes, `false` — no.

Read-only. The field is present in the `field.list` response, but the `get` and `list` methods do not accept it and return the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error.

Available if stages are enabled in the type settings: `isStagesEnabled` ||
|| **stageSemanticId**
[`string`](../../../data-types.md) | Stage group. Possible values:
- `P` — in progress
- `S` — successful
- `F` — unsuccessful

Read-only. The field is present in the `field.list` response, but the `get` and `list` methods do not accept it and return the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error. The stage group is returned by the `stage.semanticId` field.

Available if stages are enabled in the type settings: `isStagesEnabled` ||
|| **stageSemantic**
[`object`](../../../data-types.md) | Stage group with its name: `id` and `name`. The reference list of groups is returned by the [crm.dictionary.stagesemantic.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Read-only. The field is present in the `field.list` response, but the `get` and `list` methods do not accept it and return the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error.

Available if stages are enabled in the type settings: `isStagesEnabled` ||
|| **beginTime**
[`date`](../../../data-types.md) | Start date. If not passed on creation, Bitrix24 sets the item creation date.

Can be edited, filtered, and sorted.

Available if start and end dates are enabled in the type settings: `isBeginCloseDatesEnabled` ||
|| **closeTime**
[`date`](../../../data-types.md) | End date. If not passed on creation, Bitrix24 sets the date seven days after the item is created.

Can be edited, filtered, and sorted.

Available if start and end dates are enabled in the type settings: `isBeginCloseDatesEnabled` ||
|| **categoryId**
[`integer`](../../../data-types.md) | Pipeline ID. If not passed on creation, the item is placed in the default pipeline. Type pipelines are returned by the [`crm.smartprocess<entityTypeId>.category.list`](../categories/crm-entity-category-list-rest-v3.md) method.

Can be edited, filtered, and sorted.

Available if pipelines are enabled in the type settings: `isCategoriesEnabled` ||
|| **category**
[`object`](#category) | Item pipeline. An object of type [pipeline](#category).

Read-only. Requested in `select`.

Available if pipelines are enabled in the type settings: `isCategoriesEnabled` ||
|| **observersId**
[`integer[]`](../../../data-types.md) | Observer IDs. User IDs are returned by the [user.get](../../../user/user-get.md) method.

Can be edited and filtered, cannot be sorted. In a filter, `["observersId", "=", 1]` finds items that have user `1` among their observers.

Available if observers are enabled in the type settings: `isObserversEnabled` ||
|| **observers**
[`object[]`](#employee) | Observers. An array of objects of type [employee](#employee). If there are no observers, an empty array.

Read-only. Requested in `select`.

Available if observers are enabled in the type settings: `isObserversEnabled` ||
|| **contactId**
[```integer | null```](../../../data-types.md) | ID of the primary contact, which is the first one in the `contactsId` list. To set the primary contact, use the [`crm.smartprocess<entityTypeId>.setprimarycontact`](./crm-entity-set-primary-contact-rest-v3.md) method.

Read-only. Can be filtered, cannot be sorted.

Available if the client link is enabled in the type settings: `isClientEnabled` ||
|| **contact**
[```object | null```](#crm-item) | Primary contact. An object of type [CRM item](#crm-item).

Read-only. Requested in `select`.

Available if the client link is enabled in the type settings: `isClientEnabled` ||
|| **contactsId**
[`integer[]`](../../../data-types.md) | IDs of the item contacts. Contacts are returned by the [crm.contact.list](./crm-entity-list-rest-v3.md) method. The passed array replaces the current list of contacts.

Can be edited and filtered, cannot be sorted.

Available if the client link is enabled in the type settings: `isClientEnabled` ||
|| **contacts**
[`object[]`](#crm-item) | Item contacts. An array of objects of type [CRM item](#crm-item). If there are no contacts, an empty array.

Read-only. Requested in `select`.

Available if the client link is enabled in the type settings: `isClientEnabled` ||
|| **companyId**
[```integer | null```](../../../data-types.md) | Company ID. Companies are returned by the [crm.company.list](./crm-entity-list-rest-v3.md) method.

Can be edited, filtered, and sorted.

Available if the client link is enabled in the type settings: `isClientEnabled` ||
|| **company**
[```object | null```](#crm-item) | Company. An object of type [CRM item](#crm-item).

Read-only. Requested in `select`.

Available if the client link is enabled in the type settings: `isClientEnabled` ||
|| **myCompanyId**
[```integer | null```](../../../data-types.md) | ID of your company, on whose behalf you work with the client. The list of your companies is returned by the [crm.mycompany.list](./crm-entity-list-rest-v3.md) method.

Can be edited, filtered, and sorted.

Available if your company requisites are enabled in the type settings: `isMycompanyEnabled` ||
|| **myCompany**
[```object | null```](#crm-item) | Your company. An object of type [CRM item](#crm-item).

Read-only. Requested in `select`.

Available if your company requisites are enabled in the type settings: `isMycompanyEnabled` ||
|| **sourceId**
[`string`](../../../data-types.md) | Source ID, for example, `CALL`. Sources are returned by the [crm.dictionary.source.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited, filtered, and sorted.

Available if the source field is enabled in the type settings: `isSourceEnabled` ||
|| **source**
[```object | null```](#dictionary) | Source. An object of type [reference list value](#dictionary).

Read-only. Requested in `select`.

Available if the source field is enabled in the type settings: `isSourceEnabled` ||
|| **sourceDescription**
[`string`](../../../data-types.md) | Additional information about the source.

Can be edited and filtered, cannot be sorted.

Available if the source field is enabled in the type settings: `isSourceEnabled` ||
|| **opportunity**
[`double`](../../../data-types.md) | Item amount, not less than `0`. The way the amount is calculated is set by the `isManualOpportunity` field.

Can be edited, filtered, and sorted.

Available if product linking is enabled in the type settings: `isLinkWithProductsEnabled` ||
|| **taxValue**
[`double`](../../../data-types.md) | Tax amount. Calculated from the product rows.

Read-only. Can be filtered and sorted.

Available if product linking is enabled in the type settings: `isLinkWithProductsEnabled` ||
|| **isManualOpportunity**
[`boolean`](../../../data-types.md) | Amount calculation mode: `true` — the amount is set manually, `false` — the amount is calculated from the product rows.

Can be edited, filtered, and sorted.

Available if product linking is enabled in the type settings: `isLinkWithProductsEnabled` ||
|| **currencyId**
[`string`](../../../data-types.md) | Currency ID, for example, `EUR`. Currencies are returned by the [crm.currency.list](../../currency/crm-currency-list.md) method.

Can be edited, filtered, and sorted.

Available if product linking is enabled in the type settings: `isLinkWithProductsEnabled` ||
|| **currency**
[`object`](#currency) | Currency. An object of type [currency](#currency).

Read-only. Requested in `select`.

Available if product linking is enabled in the type settings: `isLinkWithProductsEnabled` ||
|| **related&lt;type code&gt;Id**
[```integer | null```](../../../data-types.md) | ID of the parent item of another object type, for example, `relatedLeadId`, `relatedSmartInvoiceId`, `relatedSmartProcess177Id`. For details, see the [Fields Linking to Other Types](#related) section.

Can be edited and filtered, cannot be sorted ||
|| **related&lt;type code&gt;**
[```object | null```](#crm-item) | Parent item of another object type, for example, `relatedLead`. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|#

### Fields Linking to Other Types {#related}

The `related<type code>Id` and `related<type code>` fields appear when Bitrix24 has a link configured in which the SPA is subordinate to another object type. For each parent type, the SPA has a pair of fields:

- `related<type code>Id` — ID of the parent item, for example, `relatedLeadId` for a lead, `relatedSmartInvoiceId` for an invoice, `relatedSmartProcess177Id` for the SPA with `entityTypeId = 177`
- `related<type code>` — the parent item as an object of type [CRM item](#crm-item), for example, `relatedLead`

The field title in the `field.list` response matches the name of the parent type, for example, "Lead" or "Equipment purchase". The method checks that an item with the passed ID exists. If there is no such item, the method returns the error 'Field value is not one of the possible values'.

Each type has its own set of these fields. The current list is returned by the [`crm.smartprocess<entityTypeId>.field.list`](../fields/crm-entity-field-list-rest-v3.md) method.

## Related Objects {#relations}

A related object is requested in `select` by the field name without the `Id` suffix: `assignedBy`, `stage`, `category`. The whole object name returns all of its fields, and a dot-notation path returns only the selected ones, for example, `["stage.name", "assignedBy.firstName"]`. The path supports one level of nesting.

A linked CRM item — `contact`, `company`, `myCompany`, `related<type code>` — returns only `id` when requested by its whole name. Request the other item fields using dot notation, for example, `["company.title", "company.url"]`.

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

Fields `contact`, `contacts`, `company`, `myCompany`, `related<type code>`.

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

Field `stage`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Stage ID, for example, `DT1038_69:NEW` ||
|| **name**
[`string`](../../../data-types.md) | Stage name ||
|| **sort**
[`integer`](../../../data-types.md) | Stage order in the pipeline ||
|| **color**
[`string`](../../../data-types.md) | Stage color in the `#RRGGBB` format ||
|| **semanticId**
[`string`](../../../data-types.md) | Stage group: `P` — in progress, `S` — successful, `F` — unsuccessful ||
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

## Custom Fields {#userfields}

Custom SPA fields are named `UF_CRM_<type id>_<code>`, for example, `UF_CRM_3_1628508893`. The name contains the type ID `id`, not `entityTypeId`: the field in the example belongs to the type with `id = 3` and `entityTypeId = 174`. Both IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. Custom fields are passed and requested by name in the same way as standard fields. The list of custom fields of the type is returned by the [`crm.smartprocess<entityTypeId>.field.custom.list`](../fields/crm-entity-field-custom-list-rest-v3.md) method.

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
