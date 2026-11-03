# Invoice Fields in REST 3.0

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These fields belong to REST 3.0 methods. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

Invoice fields are returned by the [crm.smartinvoice.get](./crm-entity-get-rest-v3.md) and [crm.smartinvoice.list](./crm-entity-list-rest-v3.md) methods and accepted by the [crm.smartinvoice.add](./crm-entity-add-rest-v3.md) and [crm.smartinvoice.update](./crm-entity-update-rest-v3.md) methods. Values of multiple fields are added and removed one at a time by the [crm.smartinvoice.addfieldvalue](./crm-entity-add-field-value-rest-v3.md) and [crm.smartinvoice.deletefieldvalue](./crm-entity-delete-field-value-rest-v3.md) methods. The list of fields with their attributes is returned by the [crm.smartinvoice.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

The `crm.smartinvoice.*` methods work with new-type invoices, which have `entityTypeId = 31`. Invoice products are stored separately from the fields and are returned by the [crm.smartinvoice.productrow.list](../product-rows/crm-entity-product-row-list-rest-v3.md) method.

Some fields exist in two forms: as an ID and as a related object, for example, `assignedById` and `assignedBy`. The ID can be passed when writing data, while the related object can only be requested in `select`.

## Which Fields the Methods Accept and Return {#methods}

#|
|| **Method** | **Accepts** | **Returns** ||
|| [crm.smartinvoice.add](./crm-entity-add-rest-v3.md) | Fields of the [Invoice Object](#item) table that can be edited, and custom fields. There are no required fields: an invoice is created even with an empty `fields` object | ID of the created invoice `id` ||
|| [crm.smartinvoice.update](./crm-entity-update-rest-v3.md) | Invoice `id` and the same fields as `add`. Fields not included in `fields` keep their current values | Success flag `true` ||
|| [crm.smartinvoice.get](./crm-entity-get-rest-v3.md) | Invoice `id` and `select` | Without `select`, only `id`. With `select`, only the listed fields; `id` is not added automatically. A whole related object, for example, `assignedBy`, returns all of its fields, while a dot-notation path, for example, `assignedBy.firstName`, returns only the selected ones. For details, see the [Related Objects](#relations) section ||
|| [crm.smartinvoice.list](./crm-entity-list-rest-v3.md) | `select`, `filter`, `order`, `pagination` | Without `select`, only the `id` of each invoice. Fields and related objects are requested in `select` the same way as in `get` ||
|| [crm.smartinvoice.addfieldvalue](./crm-entity-add-field-value-rest-v3.md), [crm.smartinvoice.deletefieldvalue](./crm-entity-delete-field-value-rest-v3.md) | Invoice `id`, the multiple field name `fieldName`, and the value `value`. Supported fields are `contactsId`, `observersId`, and multiple custom fields. For other fields, the method returns the error "The field is not supported." | Success flag `true` ||
|#

The `add` and `update` methods do not accept fields marked "read-only" and return the `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION` error with the message 'Field "taxValue" is read-only.' Related objects, for example, `company` and `stage`, are also read-only: to write data, pass the ID — `companyId`, `stageId`.

The method validates fields with values from reference lists when writing data. If the value is not in the reference list, the method returns the error 'Field value is not one of the possible values'. For the stage, source, and currency, the error message lists the allowed values.

Boolean fields accept and return `true` or `false`. The values `Y`, `N`, `1`, and `0` are not accepted: the method returns the error 'This request requires data type "bool" in field "opened"'.

Fields of the `date` type — `beginTime` and `closeTime` — accept and return a date in the `YYYY-MM-DD` format, for example, `2026-11-03`. Such a field does not accept a date with time or a date in the `03.11.2026` format. Fields of the `datetime` type are returned in ISO 8601 format, for example, `2026-11-03T14:52:21+03:00`. In a filter by a `datetime` field, also pass a date with time.

You can filter and sort only by the fields whose description says so. For other fields, the `list` method returns an error stating that the field 'requires attribute "Filterable"' or '"Sortable"'. Dot notation lets you filter by fields of linked CRM items and of the `lastCommunication` object, for example, `["company.title", "=", "Müller GmbH"]` or `["myCompany.title", "=", "My company"]`. Filtering by fields of employees, stage, and source, for example, `assignedBy.id` or `stage.semanticId`, is not supported: the method returns the `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION` error. The filter format is described on the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) page and in the [REST API 3.0 Overview](../../../rest-v3.md#filter) article.

{% note warning "" %}

The `closed`, `stageSemanticId`, and `stageSemantic` fields are present in the `crm.smartinvoice.field.list` response, but the `get` and `list` methods do not accept them. For such a field in `select`, `filter`, or `order`, the method returns the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error. The stage group can be retrieved from the `semanticId` field of the stage object: `"select": ["stage.semanticId"]`.

{% endnote %}

## Invoice Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Invoice ID. Assigned on creation.

Read-only. Can be filtered and sorted ||
|| **title**
[`string`](../../../data-types.md) | Invoice title, up to 255 characters. If the title is not passed on creation, Bitrix24 sets the word "Invoice" and the ID, for example, `Invoice #141`. `crm.smartinvoice.update` does not write an empty string: the title stays unchanged.

Can be edited, filtered, and sorted ||
|| **accountNumber**
[`string`](../../../data-types.md) | Invoice number, for example, `INV-001`. If the number is not passed on creation, Bitrix24 assigns it according to the invoice numbering settings.

Can be edited, filtered, and sorted ||
|| **xmlId**
[```string | null```](../../../data-types.md) | External code of the invoice, up to 255 characters. Use it to link the invoice to a record in an external system.

Can be edited, filtered, and sorted ||
|| **stageId**
[`string`](../../../data-types.md) | Stage ID, for example, `DT31_5:N`. If the stage is not passed on creation, the invoice is placed at the first stage. REST 3.0 has no separate method for invoice stages. Stages are returned by the [crm.status.list](../../status/crm-status-list.md) method with the filter `{"ENTITY_ID": "SMART_INVOICE_STAGE_5"}`, where `5` is the pipeline number from `stageId`. The allowed stages are also listed in the error message if an invalid value is passed.

Can be edited, filtered, and sorted ||
|| **stage**
[`object`](#stage) | Invoice stage. An object of type [stage](#stage).

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
[`boolean`](../../../data-types.md) | Whether the invoice is closed: `true` — yes, `false` — no.

Read-only. The field is present in the `field.list` response, but the `get` and `list` methods do not accept it and return the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error ||
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
[`date`](../../../data-types.md) | Invoice date. If not passed on creation, Bitrix24 sets the invoice creation date.

Can be edited, filtered, and sorted ||
|| **closeTime**
[`date`](../../../data-types.md) | Payment due date. If not passed on creation, Bitrix24 sets the date seven days after the invoice is created.

Can be edited, filtered, and sorted ||
|| **opportunity**
[`double`](../../../data-types.md) | Invoice amount, not less than `0`. For a negative value, the method returns the error 'Field value must be 0 or greater'. The way the amount is calculated is set by the `isManualOpportunity` field.

Can be edited, filtered, and sorted ||
|| **isManualOpportunity**
[`boolean`](../../../data-types.md) | Amount calculation mode: `true` — the amount is set manually, `false` — the amount is calculated from the product rows. Defaults to `false`.

Can be edited, filtered, and sorted ||
|| **taxValue**
[`double`](../../../data-types.md) | Tax amount. Calculated from the product rows.

Read-only. Can be filtered and sorted ||
|| **currencyId**
[`string`](../../../data-types.md) | Currency ID, for example, `EUR`. If not passed on creation, Bitrix24 sets the base currency. Currencies are returned by the [crm.currency.list](../../currency/crm-currency-list.md) method.

Can be edited, filtered, and sorted ||
|| **currency**
[`object`](#currency) | Currency. An object of type [currency](#currency).

Read-only. Requested in `select` ||
|| **companyId**
[```integer | null```](../../../data-types.md) | Client company ID. Companies are returned by the [crm.company.list](./crm-entity-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **company**
[```object | null```](#crm-item) | Client company. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **contactsId**
[`integer[]`](../../../data-types.md) | IDs of the invoice contacts. Contacts are returned by the [crm.contact.list](./crm-entity-list-rest-v3.md) method. The passed array replaces the current list of contacts.

Can be edited and filtered, cannot be sorted ||
|| **contacts**
[`object[]`](#crm-item) | Invoice contacts. An array of objects of type [CRM item](#crm-item). If there are no contacts, an empty array.

Read-only. Requested in `select` ||
|| **contactId**
[```integer | null```](../../../data-types.md) | ID of the primary contact, which is the first one in the `contactsId` list. To set the primary contact, use the [crm.smartinvoice.setprimarycontact](./crm-entity-set-primary-contact-rest-v3.md) method.

Read-only. Can be filtered, cannot be sorted ||
|| **contact**
[```object | null```](#crm-item) | Primary contact. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **myCompanyId**
[```integer | null```](../../../data-types.md) | ID of your company on whose behalf the invoice is issued. If not passed on creation, Bitrix24 sets your default company. The list of your companies is returned by the [crm.mycompany.list](./crm-entity-list-rest-v3.md) method. The method does not accept a client company ID and returns the error 'Field value is not one of the possible values'. The requisites of your company in the invoice are set by the [crm.smartinvoice.mycompanyrequisite.set](../requisites/crm-entity-my-company-requisite-set-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **myCompany**
[```object | null```](#crm-item) | Your company. An object of type [CRM item](#crm-item). Its fields are described in the [My Company Fields in REST 3.0](./fields-my-company.md) article.

Read-only. Requested in `select` ||
|| **relatedDealId**
[```integer | null```](../../../data-types.md) | ID of the deal the invoice belongs to. If there is no such deal, the method returns the error 'Field value is not one of the possible values'.

Can be edited and filtered, cannot be sorted ||
|| **relatedDeal**
[```object | null```](#crm-item) | Deal the invoice belongs to. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **relatedQuoteId**
[```integer | null```](../../../data-types.md) | ID of the estimate the invoice belongs to. If there is no such estimate, the method returns the error 'Field value is not one of the possible values'.

Can be edited and filtered, cannot be sorted ||
|| **relatedQuote**
[```object | null```](#crm-item) | Estimate the invoice belongs to. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **locationId**
[```integer | null```](../../../data-types.md) | Location ID. If the location is not set, `0`. If the location does not exist in Bitrix24, the method returns the error 'Field value is not one of the possible values'.

Can be edited, filtered, and sorted ||
|| **sourceId**
[```string | null```](../../../data-types.md) | Source ID, for example, `WEB`. Sources are returned by the [crm.dictionary.source.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **source**
[```object | null```](#dictionary) | Source. An object of type [reference list value](#dictionary).

Read-only. Requested in `select` ||
|| **sourceDescription**
[```string | null```](../../../data-types.md) | Additional information about the source.

Can be edited and filtered, cannot be sorted ||
|| **assignedById**
[`integer`](../../../data-types.md) | ID of the person responsible for the invoice. If not passed on creation, the user who called the method becomes the responsible person. User IDs are returned by the [user.get](../../../user/user-get.md) method. If the user does not exist, the method returns the error 'Field value is not one of the possible values'.

Can be edited, filtered, and sorted ||
|| **assignedBy**
[`object`](#employee) | Responsible person. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **observersId**
[`integer[]`](../../../data-types.md) | Observer IDs. User IDs are returned by the [user.get](../../../user/user-get.md) method. The passed array replaces the current list of observers.

Can be edited and filtered, cannot be sorted. In a filter, `["observersId", "=", 1]` finds invoices that have user `1` among their observers ||
|| **observers**
[`object[]`](#employee) | Observers. An array of objects of type [employee](#employee). If there are no observers, an empty array.

Read-only. Requested in `select` ||
|| **opened**
[`boolean`](../../../data-types.md) | Whether the invoice is available to all users: `true` — yes, `false` — no.

Can be edited, filtered, and sorted ||
|| **comments**
[```string | null```](../../../data-types.md) | Comment on the invoice.

Can be edited. Cannot be filtered or sorted ||
|| **createdTime**
[`datetime`](../../../data-types.md) | Date and time the invoice was created.

Read-only. Can be filtered and sorted ||
|| **createdById**
[`integer`](../../../data-types.md) | ID of the user who created the invoice.

Read-only. Can be filtered and sorted ||
|| **createdBy**
[`object`](#employee) | User who created the invoice. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **updatedTime**
[`datetime`](../../../data-types.md) | Date and time the invoice was last updated.

Read-only. Can be filtered and sorted ||
|| **updatedById**
[`integer`](../../../data-types.md) | ID of the user who last updated the invoice.

Read-only. Can be filtered and sorted ||
|| **updatedBy**
[`object`](#employee) | User who last updated the invoice. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **lastActivityTime**
[`datetime`](../../../data-types.md) | Date and time of the last activity in the invoice timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityById**
[`integer`](../../../data-types.md) | ID of the author of the last activity in the timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityBy**
[`object`](#employee) | Author of the last activity in the timeline. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **webformId**
[```integer | null```](../../../data-types.md) | ID of the CRM form through which the invoice was created. If the invoice was not created by a form, `null`. The list of forms is returned by the [crm.form.list](../../form/crm-form-list.md) method.

Read-only. Can be filtered and sorted ||
|| **webform**
[```object | null```](#webform) | CRM form through which the invoice was created. An object of type [CRM form](#webform).

Read-only. Requested in `select` ||
|| **lastCommunication**
[`object`](#last-communication) | Dates of the last communications on the invoice. An object of type [last communication](#last-communication).

Read-only. Requested in `select`. Can be filtered by object fields using dot notation, for example, `["lastCommunication.callTime", ">=", "2026-11-01T00:00:00+03:00"]` ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ID. For an invoice, `31`.

Read-only ||
|| **canRead**
[`boolean`](../../../data-types.md) | Whether the current user can view the invoice: `true` — yes, `false` — no.

Read-only ||
|| **url**
[`string`](../../../data-types.md) | Relative link to the invoice card, for example, `/crm/type/31/details/139/?categoryId=5`.

Read-only ||
|#

## Related Objects {#relations}

A related object is requested in `select` by the field name without the `Id` suffix: `assignedBy`, `stage`, `company`. The whole object name returns all of its fields, and a dot-notation path returns only the selected ones, for example, `["stage.name", "assignedBy.firstName"]`. The path supports one level of nesting.

A linked CRM item — `contact`, `contacts`, `company`, `myCompany`, `relatedDeal`, `relatedQuote` — returns only `id` when requested by its whole name. Request the other item fields using dot notation. Dot notation gives access to the fields of the [CRM item object](#crm-item) and to any fields of the linked item's type, for example, `["company.title", "company.phone", "myCompany.title"]`.

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

Fields `contact`, `contacts`, `company`, `myCompany`, `relatedDeal`, `relatedQuote`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Item ID ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ID, for example, `3` — contact, `4` — company, `2` — deal ||
|| **canRead**
[`boolean`](../../../data-types.md) | Whether the current user can view the item ||
|| **title**
[`string`](../../../data-types.md) | Item title. For a contact, the last name, first name, and middle name ||
|| **url**
[`string`](../../../data-types.md) | Relative link to the item card, for example, `/crm/company/details/3159/` ||
|#

### Stage Object {#stage}

Field `stage`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Stage ID, for example, `DT31_5:P` ||
|| **name**
[`string`](../../../data-types.md) | Stage name, for example, `Paid` ||
|| **sort**
[`integer`](../../../data-types.md) | Stage order in the pipeline ||
|| **color**
[`string`](../../../data-types.md) | Stage color in the `#RRGGBB` format ||
|| **semanticId**
[`string`](../../../data-types.md) | Stage group: `P` — in progress, `S` — successful, `F` — unsuccessful ||
|#

### Reference List Value Object {#dictionary}

Field `source`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Value ID, for example, `WEB` ||
|| **name**
[`string`](../../../data-types.md) | Value name, for example, `Website` ||
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

Custom invoice fields are named `UF_CRM_SMART_INVOICE_<code>` or `UF_CRM_<code>`, for example, `UF_CRM_619F45A0AF6DB`. They are passed and requested by name in the same way as standard fields. The list of custom invoice fields is returned by the [crm.smartinvoice.field.custom.list](../fields/crm-entity-field-custom-list-rest-v3.md) method.

The value of a "Money" field is passed as an object `{"sum": 150.5, "currencyId": "EUR"}`. A string such as `150.5|EUR` is not accepted: the method returns the error 'This request requires data type "MoneyDto" in field "UF_CRM_…"'.

Each custom field has a paired `UF_CRM_…_data` field, which is read-only and requested in `select`. It contains the value in several representations:

- `value` — the value in the same format as in the field itself
- `rawValue` — the value in the storage format, for example, `150.5|EUR` for a "Money" field
- `formattedValue` — the value for display to the user: `150.50 €` for money, the employee name for a "Link to employee" field
- `extra` — additional data: for money, `amount`, `currencyId`, and `currencyFullName`; for an employee, the fields of the [employee](#employee) object. For a string, number, and date, `null`

For a multiple custom field, `UF_CRM_…_data` is an array of such objects.

## Continue Learning

- [{#T}](./crm-entity-add-rest-v3.md)
- [{#T}](./crm-entity-update-rest-v3.md)
- [{#T}](./crm-entity-get-rest-v3.md)
- [{#T}](./crm-entity-list-rest-v3.md)
- [{#T}](./crm-entity-set-primary-contact-rest-v3.md)
- [{#T}](../fields/crm-entity-field-list-rest-v3.md)
- [{#T}](../../../rest-v3.md)
