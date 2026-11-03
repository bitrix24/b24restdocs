# Lead Fields in REST 3.0

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These fields belong to REST 3.0 methods. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

Lead fields are returned by the [crm.lead.get](./crm-entity-get-rest-v3.md) and [crm.lead.list](./crm-entity-list-rest-v3.md) methods and accepted by the [crm.lead.add](./crm-entity-add-rest-v3.md) and [crm.lead.update](./crm-entity-update-rest-v3.md) methods. The list of fields with their attributes is returned by the [crm.lead.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

Some fields exist in two forms: as an ID and as a related object, for example, `assignedById` and `assignedBy`. The ID can be passed when writing data, while the related object can only be requested in `select`.

## Which Fields the Methods Accept and Return {#methods}

#|
|| **Method** | **Accepts** | **Returns** ||
|| [crm.lead.add](./crm-entity-add-rest-v3.md) | Fields of the [Lead Object](#item) table that can be edited, and custom fields. The required field is `title` | ID of the created lead `id` ||
|| [crm.lead.update](./crm-entity-update-rest-v3.md) | Lead `id` and the same fields as `add`. Fields not included in `fields` keep their current values | Success flag `true` ||
|| [crm.lead.get](./crm-entity-get-rest-v3.md) | Lead `id` and `select` | Without `select`, only `id`. With `select`, only the listed fields; `id` is not added automatically. A whole related object, for example, `assignedBy`, returns all of its fields, while a dot-notation path, for example, `assignedBy.firstName`, returns only the selected ones. For details, see the [Related Objects](#relations) section ||
|| [crm.lead.list](./crm-entity-list-rest-v3.md) | `select`, `filter`, `order`, `pagination` | Without `select`, only the `id` of each lead. Fields and related objects are requested in `select` the same way as in `get` ||
|#

The `add` and `update` methods do not accept fields marked "read-only" and return the `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION` error with the message 'Field "hasPhone" is read-only.' Related objects, for example, `company` and `stage`, are also read-only: to write data, pass the ID — `companyId`, `stageId`.

The method validates fields with values from reference lists when writing data. If the value is not in the reference list, the method returns the error 'Field value is not one of the possible values'. For the stage, salutation, source, and currency, the error message lists the allowed values.

Boolean fields accept and return `true` or `false`. The values `Y`, `N`, `1`, and `0` are not accepted: the method returns the error 'This request requires data type "bool" in field "opened"'.

Fields of the `date` type accept and return a date in the `YYYY-MM-DD` format, for example, `1990-05-15`. Such a field does not accept a date with time or a date in the `15.05.1990` format. Fields of the `datetime` type are returned in ISO 8601 format, for example, `2026-11-03T14:48:48+03:00`. In a filter by a `datetime` field, also pass a date with time: the method does not accept the value `2026-11-03`.

You can filter and sort only by the fields whose description says so. For other fields, the `list` method returns an error stating that the field 'requires attribute "Filterable"' or '"Sortable"'. Dot notation lets you filter by fields of linked CRM items, of phone, email, website, and messenger values, and of the `lastCommunication` and `utm` objects, for example, `["company.title", "=", "Müller GmbH"]` or `["im.valueTypeId", "=", "IMOL"]`. Filtering by fields of employees, stage, and source, for example, `assignedBy.id` or `stage.semanticId`, is not supported: the method returns the `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION` error. The filter format is described on the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) page and in the [REST API 3.0 Overview](../../../rest-v3.md#filter) article.

{% note warning "" %}

The `closed` and `taxValue` fields are present in the `crm.lead.field.list` response, but the `get` and `list` methods do not accept them. For such a field in `select`, `filter`, or `order`, the method returns the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error.

{% endnote %}

## Lead Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../../data-types.md) | Lead title, up to 255 characters. Required for [creating a lead](./crm-entity-add-rest-v3.md).

Can be edited, filtered, and sorted ||
|| **honorificId**
[```string | null```](../../../data-types.md) | Salutation ID, for example, `HNR_RU_1`. Salutations are returned by the [crm.dictionary.honorific.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **honorific**
[```object | null```](#dictionary) | Salutation. An object of type [reference list value](#dictionary).

Read-only. Requested in `select` ||
|| **name**
[```string | null```](../../../data-types.md) | First name, up to 50 characters.

Can be edited, filtered, and sorted ||
|| **secondName**
[```string | null```](../../../data-types.md) | Middle name, up to 50 characters.

Can be edited, filtered, and sorted ||
|| **lastName**
[```string | null```](../../../data-types.md) | Last name, up to 50 characters.

Can be edited, filtered, and sorted ||
|| **post**
[```string | null```](../../../data-types.md) | Position, up to 255 characters.

Can be edited, filtered, and sorted ||
|| **companyTitle**
[```string | null```](../../../data-types.md) | Name of the client's company, up to 255 characters. This is text in the lead card, not a link to a CRM company — the link is set by the `companyId` field.

Can be edited, filtered, and sorted ||
|| **birthdayTime**
[```date | null```](../../../data-types.md) | Date of birth.

Can be edited, filtered, and sorted ||
|| **statusDescription**
[```string | null```](../../../data-types.md) | Additional information about the lead stage.

Can be edited, filtered, and sorted ||
|| **id**
[`integer`](../../../data-types.md) | Lead ID. Assigned on creation.

Read-only. Can be filtered and sorted ||
|| **createdTime**
[`datetime`](../../../data-types.md) | Date and time the lead was created.

Read-only. Can be filtered and sorted ||
|| **createdById**
[`integer`](../../../data-types.md) | ID of the user who created the lead.

Read-only. Can be filtered and sorted ||
|| **createdBy**
[`object`](#employee) | User who created the lead. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **updatedTime**
[`datetime`](../../../data-types.md) | Date and time the lead was last updated.

Read-only. Can be filtered and sorted ||
|| **updatedById**
[`integer`](../../../data-types.md) | ID of the user who last updated the lead.

Read-only. Can be filtered and sorted ||
|| **updatedBy**
[`object`](#employee) | User who last updated the lead. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **lastActivityTime**
[`datetime`](../../../data-types.md) | Date and time of the last activity in the lead timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityById**
[`integer`](../../../data-types.md) | ID of the author of the last activity in the timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityBy**
[`object`](#employee) | Author of the last activity in the timeline. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **assignedById**
[`integer`](../../../data-types.md) | ID of the person responsible for the lead. User IDs are returned by the [user.get](../../../user/user-get.md) method. If the user does not exist, the method returns the error 'Field value is not one of the possible values'.

Can be edited, filtered, and sorted ||
|| **assignedBy**
[`object`](#employee) | Responsible person. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **opened**
[`boolean`](../../../data-types.md) | Whether the lead is available to all users: `true` — yes, `false` — no.

Can be edited, filtered, and sorted ||
|| **comments**
[```string | null```](../../../data-types.md) | Comment on the lead. Supports BB codes, for example, `[b]text[/b]`.

Can be edited. Cannot be filtered or sorted ||
|| **originatorId**
[```string | null```](../../../data-types.md) | ID of the external system the lead came from. Use together with `originId` to link the lead to an external source.

Can be edited, filtered, and sorted ||
|| **originId**
[```string | null```](../../../data-types.md) | Lead ID in the external system.

Can be edited, filtered, and sorted ||
|| **webformId**
[```integer | null```](../../../data-types.md) | ID of the CRM form through which the lead was created. If the lead was not created by a form, `null`. The list of forms is returned by the [crm.form.list](../../form/crm-form-list.md) method.

Read-only. Can be filtered and sorted ||
|| **webform**
[```object | null```](#webform) | CRM form through which the lead was created. An object of type [CRM form](#webform).

Read-only. Requested in `select` ||
|| **lastCommunication**
[`object`](#last-communication) | Dates of the last communications with the client on the lead. An object of type [last communication](#last-communication).

Read-only. Requested in `select`. Can be filtered by object fields using dot notation, for example, `["lastCommunication.imolTime", ">=", "2026-11-01T00:00:00+03:00"]` ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ID. For a lead, `1`.

Read-only ||
|| **canRead**
[`boolean`](../../../data-types.md) | Whether the current user can view the lead: `true` — yes, `false` — no.

Read-only ||
|| **url**
[`string`](../../../data-types.md) | Relative link to the lead card, for example, `/crm/lead/details/1001253/`.

Read-only ||
|| **stageId**
[`string`](../../../data-types.md) | Lead stage ID, for example, `NEW`, `IN_PROCESS`, `CONVERTED`. Leads have a single pipeline. Lead stages are returned by the [crm.status.list](../../status/crm-status-list.md) method with the `ENTITY_ID = STATUS` filter.

Can be edited, filtered, and sorted ||
|| **stage**
[`object`](#stage) | Lead stage. An object of type [stage](#stage).

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
[`boolean`](../../../data-types.md) | Whether the lead is closed.

Read-only. The field is present in the `field.list` response, but the `get` and `list` methods do not accept it and return the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error. The lead stage group is returned by the `stageSemanticId` field ||
|| **stageSemanticId**
[`string`](../../../data-types.md) | Stage group. Possible values:
- `P` — in progress
- `S` — successful
- `F` — unsuccessful

Read-only. Can be filtered and sorted ||
|| **stageSemantic**
[`object`](#stage-semantic) | Stage group with its name. An object of type [stage group](#stage-semantic).

Read-only. Requested in `select` ||
|| **observersId**
[`integer[]`](../../../data-types.md) | Observer IDs. User IDs are returned by the [user.get](../../../user/user-get.md) method.

Can be edited and filtered, cannot be sorted. In a filter, `["observersId", "=", 1]` finds leads that have user `1` among their observers ||
|| **observers**
[`object[]`](#employee) | Observers. An array of objects of type [employee](#employee). If there are no observers, an empty array.

Read-only. Requested in `select` ||
|| **contactId**
[```integer | null```](../../../data-types.md) | ID of the primary contact, which is the first one in the `contactsId` list. To set the primary contact, use the [crm.lead.setprimarycontact](./crm-entity-set-primary-contact-rest-v3.md) method.

Read-only. Can be filtered, cannot be sorted ||
|| **contact**
[```object | null```](#crm-item) | Primary contact. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **contactsId**
[`integer[]`](../../../data-types.md) | IDs of the contacts the lead is linked to. Contacts are returned by the [crm.contact.list](./crm-entity-list-rest-v3.md) method. The passed array replaces the current list of contacts.

Can be edited and filtered, cannot be sorted ||
|| **contacts**
[`object[]`](#crm-item) | Lead contacts. An array of objects of type [CRM item](#crm-item). If there are no contacts, an empty array.

Read-only. Requested in `select` ||
|| **companyId**
[```integer | null```](../../../data-types.md) | ID of the CRM company the lead is linked to. Companies are returned by the [crm.company.list](./crm-entity-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **company**
[```object | null```](#crm-item) | Company. An object of type [CRM item](#crm-item).

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
[`double`](../../../data-types.md) | Lead amount, not less than `0`. The way the amount is calculated is set by the `isManualOpportunity` field.

Can be edited, filtered, and sorted ||
|| **taxValue**
[`double`](../../../data-types.md) | Tax amount.

Read-only. The field is present in the `field.list` response, but the `get` and `list` methods do not accept it and return the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error ||
|| **isManualOpportunity**
[`boolean`](../../../data-types.md) | Amount calculation mode: `true` — the amount is set manually, `false` — the amount is calculated from the product rows.

Can be edited, filtered, and sorted ||
|| **currencyId**
[`string`](../../../data-types.md) | Currency ID, for example, `EUR`. Currencies are returned by the [crm.currency.list](../../currency/crm-currency-list.md) method.

Can be edited, filtered, and sorted ||
|| **currency**
[`object`](#currency) | Currency. An object of type [currency](#currency).

Read-only. Requested in `select` ||
|| **phone**
[`object[]`](#multifield) | Phone numbers. An array of objects of type [multiple field value](#multifield). Phone types are returned by the [crm.dictionary.phonetype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited: the passed array replaces all phone numbers of the lead, for example, `[{"valueTypeId": "WORK", "value": "+49300000001"}]`. Can be filtered by object fields using dot notation, for example, `["phone.value", "=", "+49300000001"]`, cannot be sorted ||
|| **email**
[`object[]`](#multifield) | Email addresses. An array of objects of type [multiple field value](#multifield). Email types are returned by the [crm.dictionary.emailtype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited: the passed array replaces all email addresses of the lead. Can be filtered by object fields using dot notation, cannot be sorted ||
|| **web**
[`object[]`](#multifield) | Websites. An array of objects of type [multiple field value](#multifield). Website types are returned by the [crm.dictionary.webtype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited: the passed array replaces all websites of the lead. Can be filtered by object fields using dot notation, cannot be sorted ||
|| **im**
[`object[]`](#multifield) | Messengers and open channels. An array of objects of type [multiple field value](#multifield). Messenger types are returned by the [crm.dictionary.imtype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited: the passed array replaces all values of this field. Can be filtered by object fields using dot notation, for example, `["im.valueTypeId", "=", "IMOL"]`, cannot be sorted ||
|| **hasPhone**
[`boolean`](../../../data-types.md) | Whether the lead has at least one phone number: `true` — yes, `false` — no.

Read-only. Cannot be filtered or sorted ||
|| **hasEmail**
[`boolean`](../../../data-types.md) | Whether the lead has at least one email address: `true` — yes, `false` — no.

Read-only. Cannot be filtered or sorted ||
|| **hasWeb**
[`boolean`](../../../data-types.md) | Whether the lead has at least one website: `true` — yes, `false` — no.

Read-only. Cannot be filtered or sorted ||
|| **hasIm**
[`boolean`](../../../data-types.md) | Whether the lead has at least one value in the `im` field: `true` — yes, `false` — no.

Read-only. Cannot be filtered or sorted ||
|| **utm**
[```object | null```](#utm) | UTM tags of the lead. An object of type [UTM tags](#utm). If there are no tags, `null`.

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

The `related<type code>Id` and `related<type code>` fields appear when Bitrix24 has a link configured in which the lead is subordinate to another object type, for example, an SPA. For each parent type, the lead has a pair of fields:

- `related<type code>Id` — ID of the parent item, for example, `relatedSmartProcess177Id` for the SPA with `entityTypeId = 177`
- `related<type code>` — the parent item as an object of type [CRM item](#crm-item), for example, `relatedSmartProcess177`

The field title in the `crm.lead.field.list` response matches the name of the parent type, for example, "Equipment purchase". The method checks that an item with the passed ID exists. If there is no such item, the method returns the error 'Field value is not one of the possible values'.

The set of these fields depends on the links configured in Bitrix24. The current list is returned by the [crm.lead.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

## Related Objects {#relations}

A related object is requested in `select` by the field name without the `Id` suffix: `assignedBy`, `stage`, `honorific`. The whole object name returns all of its fields, and a dot-notation path returns only the selected ones, for example, `["stage.name", "assignedBy.firstName"]`. The path supports one level of nesting.

A linked CRM item — `contact`, `company`, `related<type code>` — returns only `id` when requested by its whole name. Request the other item fields using dot notation, for example, `["company.title", "company.url"]`.

If a link is not set, the object is returned as `null`, and an array of objects, for example, `observers`, `contacts`, or `phone`, is returned as an empty array.

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

Fields `contact`, `contacts`, `company`, `related<type code>`.

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

### Phone Numbers, Email, Websites, and Messengers {#multifield}

Fields `phone`, `email`, `web`, `im`. When writing data, pass `valueTypeId` and `value`, both are required. If the value type is not in the reference list, the method returns the error 'Field value is not one of the possible values'.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Value ID. Assigned when writing data; when the list is replaced, the values receive new IDs ||
|| **valueTypeId**
[`string`](../../../data-types.md) | Value type, for example, `WORK` or `MOBILE` for a phone number ||
|| **valueType**
[`object`](../../../data-types.md) | Value type with its name: `id`, `name` — full name, `nameShort` — short name. Read-only ||
|| **value**
[`string`](../../../data-types.md) | Value: phone number, email address, website address, or messenger ID ||
|#

### Stage Object {#stage}

Field `stage`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Stage ID, for example, `NEW` ||
|| **name**
[`string`](../../../data-types.md) | Stage name ||
|| **sort**
[`integer`](../../../data-types.md) | Stage order ||
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
[`string`](../../../data-types.md) | Group name, for example, `Stage group "Lead in progress"` ||
|#

### Reference List Value Object {#dictionary}

Fields `honorific` and `source`.

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

Custom lead fields are named `UF_CRM_<code>`, for example, `UF_CRM_1687961618`. They are passed and requested by name in the same way as standard fields. The list of custom fields is returned by the [crm.lead.field.custom.list](../fields/crm-entity-field-custom-list-rest-v3.md) method.

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
