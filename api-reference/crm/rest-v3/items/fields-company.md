# Company Fields in REST 3.0

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These fields belong to REST 3.0 methods. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

Company fields are returned by the [crm.company.get](./crm-entity-get-rest-v3.md) and [crm.company.list](./crm-entity-list-rest-v3.md) methods and accepted by the [crm.company.add](./crm-entity-add-rest-v3.md) and [crm.company.update](./crm-entity-update-rest-v3.md) methods. Values of multiple fields are added and removed one at a time by the [crm.company.addfieldvalue](./crm-entity-add-field-value-rest-v3.md) and [crm.company.deletefieldvalue](./crm-entity-delete-field-value-rest-v3.md) methods. The list of fields with their attributes is returned by the [crm.company.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

The `crm.company.*` methods work only with client companies. They do not find companies from the "My companies" reference list: `crm.company.get` returns the error 'Entity with ID … was not found.', and `crm.company.list` does not show them. For your own companies, use the `crm.mycompany.*` methods; their fields are described in the [My Company Fields in REST 3.0](./fields-my-company.md) article.

Some fields exist in two forms: as an ID and as a related object, for example, `assignedById` and `assignedBy`. The ID can be passed when writing data, while the related object can only be requested in `select`.

## Which Fields the Methods Accept and Return {#methods}

#|
|| **Method** | **Accepts** | **Returns** ||
|| [crm.company.add](./crm-entity-add-rest-v3.md) | Fields of the [Company Object](#item) table that can be edited, and custom fields. The `title` field is required; otherwise, the method returns the error 'Required field "title" is empty.' | ID of the created company `id` ||
|| [crm.company.update](./crm-entity-update-rest-v3.md) | Company `id` and the same fields as `add`. Fields not included in `fields` keep their current values | Success flag `true` ||
|| [crm.company.get](./crm-entity-get-rest-v3.md) | Company `id` and `select` | Without `select`, only `id`. With `select`, only the listed fields; `id` is not added automatically. A whole related object, for example, `assignedBy`, returns all of its fields, while a dot-notation path, for example, `assignedBy.firstName`, returns only the selected ones. For details, see the [Related Objects](#relations) section ||
|| [crm.company.list](./crm-entity-list-rest-v3.md) | `select`, `filter`, `order`, `pagination` | Without `select`, only the `id` of each company. Fields and related objects are requested in `select` the same way as in `get` ||
|| [crm.company.addfieldvalue](./crm-entity-add-field-value-rest-v3.md), [crm.company.deletefieldvalue](./crm-entity-delete-field-value-rest-v3.md) | Company `id`, the multiple field name `fieldName`, and the value `value`. Supported fields are `phone`, `email`, `web`, `im`, `contactsId`, `observersId`, and multiple custom fields. For other fields, the method returns the error "The field is not supported." | Success flag `true` ||
|#

The `add` and `update` methods do not accept fields marked "read-only" and return the `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION` error with the message 'Field "contactId" is read-only.' Related objects, for example, `contacts` and `assignedBy`, are also read-only: to write data, pass the ID — `contactsId`, `assignedById`.

The method validates fields with values from reference lists when writing data. If the value is not in the reference list, the method returns the error 'Field value is not one of the possible values'. For the company type, industry, number of employees, and currency, the error message lists the allowed values.

Boolean fields accept and return `true` or `false`. The values `Y`, `N`, `1`, and `0` are not accepted: the method returns the error 'This request requires data type "bool" in field "opened"'.

Fields of the `datetime` type are returned in ISO 8601 format, for example, `2026-11-03T14:42:01+03:00`. In a filter by a `datetime` field, also pass a date with time.

You can filter and sort only by the fields whose description says so. For other fields, the `list` method returns an error stating that the field 'requires attribute "Filterable"' or '"Sortable"'. Dot notation lets you filter by fields of linked CRM items, of phone, email, website, and messenger values, and of the `utm` and `lastCommunication` objects, for example, `["contact.name", "=", "Klaus"]` or `["phone.value", "=", "+49300000001"]`. Filtering by fields of employees and reference lists, for example, `assignedBy.id` or `industry.name`, is not supported: the method returns the `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION` error. The filter format is described on the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) page and in the [REST API 3.0 Overview](../../../rest-v3.md#filter) article.

## Company Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Company ID. Assigned on creation.

Read-only. Can be filtered and sorted ||
|| **title**
[`string`](../../../data-types.md) | Company name, up to 255 characters. Required when creating a company. `crm.company.update` does not write an empty string: the name does not change.

Can be edited, filtered, and sorted ||
|| **typeId**
[```string | null```](../../../data-types.md) | Company type ID, for example, `CUSTOMER`. The values are returned by the [crm.dictionary.companytype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **type**
[```object | null```](#dictionary) | Company type. An object of type [reference list value](#dictionary).

Read-only. Requested in `select` ||
|| **industryId**
[```string | null```](../../../data-types.md) | Industry ID, for example, `IT`. The values are returned by the [crm.dictionary.industry.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **industry**
[```object | null```](#dictionary) | Industry. An object of type [reference list value](#dictionary).

Read-only. Requested in `select` ||
|| **employeesId**
[```string | null```](../../../data-types.md) | ID of the employee count range, for example, `EMPLOYEES_2`. The values are returned by the [crm.dictionary.employees.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **employees**
[```object | null```](#dictionary) | Number of employees. An object of type [reference list value](#dictionary).

Read-only. Requested in `select` ||
|| **revenue**
[```double | null```](../../../data-types.md) | Annual revenue, for example, `1500000.5`. The revenue currency is set by the `currencyId` field.

Can be edited, filtered, and sorted ||
|| **currencyId**
[```string | null```](../../../data-types.md) | ID of the annual revenue currency, for example, `EUR`. Currencies are returned by the [crm.currency.list](../../currency/crm-currency-list.md) method.

Can be edited, filtered, and sorted ||
|| **currency**
[```object | null```](#currency) | Currency. An object of type [currency](#currency).

Read-only. Requested in `select` ||
|| **logo**
[```object | null```](#file) | Company logo. An object of type [file](#file). To delete the logo, pass `null`.

Can be edited. Cannot be filtered or sorted ||
|| **contactsId**
[`integer[]`](../../../data-types.md) | IDs of the company contacts. Contacts are returned by the [crm.contact.list](./crm-entity-list-rest-v3.md) method. The passed array replaces the current list of contacts.

Can be edited and filtered, cannot be sorted. In a filter, `["contactsId", "=", 3211]` finds companies that contact `3211` is linked to ||
|| **contacts**
[`object[]`](#crm-item) | Company contacts. An array of objects of type [CRM item](#crm-item). If there are no contacts, an empty array.

Read-only. Requested in `select` ||
|| **contactId**
[```integer | null```](../../../data-types.md) | ID of the contact for which this company is the primary one. If the company is not primary for any of its contacts, `null`. The primary company of a contact is set by the order in the contact's `companiesId` field; for details, see the [Contact Fields in REST 3.0](./fields-contact.md) article. The full list of the company contacts is in the `contactsId` field.

Read-only. Can be filtered, cannot be sorted ||
|| **contact**
[```object | null```](#crm-item) | Contact for which this company is the primary one. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **leadId**
[```integer | null```](../../../data-types.md) | ID of the lead from which the company was created. If the company was not created from a lead, `null`.

Read-only. Can be filtered and sorted ||
|| **lead**
[```object | null```](#crm-item) | Lead from which the company was created. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **phone**
[`object[]`](#multifield) | Company phone numbers. An array of objects of type [multiple field value](#multifield). Phone types are returned by the [crm.dictionary.phonetype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited. Can be filtered by value fields using dot notation, for example, `["phone.value", "=", "+49300000001"]`. Cannot be sorted ||
|| **email**
[`object[]`](#multifield) | Email addresses. An array of objects of type [multiple field value](#multifield). Email types are returned by the [crm.dictionary.emailtype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method. The method validates the address format and returns the error 'The field "Work E-mail" contains an incorrect address.' for an invalid address.

Can be edited. Can be filtered by value fields using dot notation, for example, `email.value`. Cannot be sorted ||
|| **web**
[`object[]`](#multifield) | Websites and social media pages. An array of objects of type [multiple field value](#multifield). Website types are returned by the [crm.dictionary.webtype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited. Can be filtered by value fields using dot notation, for example, `web.value`. Cannot be sorted ||
|| **im**
[`object[]`](#multifield) | Messengers and open channels. An array of objects of type [multiple field value](#multifield). Messenger types are returned by the [crm.dictionary.imtype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited. Can be filtered by value fields using dot notation, for example, `im.value`. Cannot be sorted ||
|| **hasPhone**
[`boolean`](../../../data-types.md) | Whether the company has at least one phone number: `true` — yes, `false` — no.

Read-only. Cannot be filtered or sorted ||
|| **hasEmail**
[`boolean`](../../../data-types.md) | Whether the company has at least one email address: `true` — yes, `false` — no.

Read-only. Cannot be filtered or sorted ||
|| **hasWeb**
[`boolean`](../../../data-types.md) | Whether the company has at least one website: `true` — yes, `false` — no.

Read-only. Cannot be filtered or sorted ||
|| **hasIm**
[`boolean`](../../../data-types.md) | Whether the company has at least one value in the `im` field: `true` — yes, `false` — no.

Read-only. Cannot be filtered or sorted ||
|| **assignedById**
[`integer`](../../../data-types.md) | ID of the person responsible for the company. User IDs are returned by the [user.get](../../../user/user-get.md) method. If the user does not exist, the method returns the error 'Field value is not one of the possible values'.

Can be edited, filtered, and sorted ||
|| **assignedBy**
[`object`](#employee) | Responsible person. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **observersId**
[`integer[]`](../../../data-types.md) | Observer IDs. User IDs are returned by the [user.get](../../../user/user-get.md) method. The passed array replaces the current list of observers.

Can be edited and filtered, cannot be sorted. In a filter, `["observersId", "=", 1]` finds companies that have user `1` among their observers ||
|| **observers**
[`object[]`](#employee) | Observers. An array of objects of type [employee](#employee). If there are no observers, an empty array.

Read-only. Requested in `select` ||
|| **opened**
[`boolean`](../../../data-types.md) | Whether the company is available to all users: `true` — yes, `false` — no.

Can be edited, filtered, and sorted ||
|| **comments**
[```string | null```](../../../data-types.md) | Comment on the company.

Can be edited. Cannot be filtered or sorted ||
|| **utm**
[`object`](#utm) | UTM tags of the company. An object of type [UTM tags](#utm).

Can be edited. Can be filtered by object fields using dot notation, for example, `["utm.source", "=", "google"]`. Cannot be sorted ||
|| **trackingSourceId**
[```integer | null```](../../../data-types.md) | ID of the end-to-end analytics source. If the source does not exist or is archived, the method returns an error stating that the end-to-end analytics source was not found or is archived.

Can be edited and filtered, cannot be sorted ||
|| **originatorId**
[```string | null```](../../../data-types.md) | ID of the external system the company was imported from. Use together with `originId` to link the company to the record in the external system.

Can be edited, filtered, and sorted ||
|| **originId**
[```string | null```](../../../data-types.md) | Company ID in the external system.

Can be edited, filtered, and sorted ||
|| **originVersion**
[```string | null```](../../../data-types.md) | Record version in the external system.

Can be edited. Cannot be filtered or sorted ||
|| **createdTime**
[`datetime`](../../../data-types.md) | Date and time the company was created.

Read-only. Can be filtered and sorted ||
|| **createdById**
[`integer`](../../../data-types.md) | ID of the user who created the company. The user data is in the `createdBy` object.

Read-only. Can be filtered and sorted ||
|| **createdBy**
[`object`](#employee) | User who created the company. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **updatedTime**
[`datetime`](../../../data-types.md) | Date and time the company was last updated.

Read-only. Can be filtered and sorted ||
|| **updatedById**
[`integer`](../../../data-types.md) | ID of the user who last updated the company.

Read-only. Can be filtered and sorted ||
|| **updatedBy**
[`object`](#employee) | User who last updated the company. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **lastActivityTime**
[`datetime`](../../../data-types.md) | Date and time of the last activity in the company timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityById**
[`integer`](../../../data-types.md) | ID of the author of the last activity in the timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityBy**
[`object`](#employee) | Author of the last activity in the timeline. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **webformId**
[```integer | null```](../../../data-types.md) | ID of the CRM form through which the company was created. If the company was not created by a form, `null`. The list of forms is returned by the [crm.form.list](../../form/crm-form-list.md) method.

Read-only. Can be filtered and sorted ||
|| **webform**
[```object | null```](#webform) | CRM form through which the company was created. An object of type [CRM form](#webform).

Read-only. Requested in `select` ||
|| **lastCommunication**
[`object`](#last-communication) | Dates of the last communications with the company. An object of type [last communication](#last-communication).

Read-only. Requested in `select`. Can be filtered by object fields using dot notation, for example, `["lastCommunication.callTime", ">=", "2026-11-01T00:00:00+03:00"]` ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ID. For a company, `4`.

Read-only ||
|| **canRead**
[`boolean`](../../../data-types.md) | Whether the current user can view the company: `true` — yes, `false` — no.

Read-only ||
|| **url**
[`string`](../../../data-types.md) | Relative link to the company card, for example, `/crm/company/details/3159/`.

Read-only ||
|| **related&lt;type code&gt;Id**
[```integer | null```](../../../data-types.md) | ID of the parent SPA item, for example, `relatedSmartProcess177Id`. For details, see the [Fields Linking to Other Types](#related) section.

Can be edited and filtered, cannot be sorted ||
|| **related&lt;type code&gt;**
[```object | null```](#crm-item) | Parent SPA item, for example, `relatedSmartProcess177`. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|#

### Phone Numbers, Email, Websites, and Messengers {#multifield}

The `phone`, `email`, `web`, and `im` fields are arrays of values. Each value is an object with the following fields:

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Value ID. Assigned when writing data and cannot be passed: the method returns the error 'Field "id" is read-only.' ||
|| **valueTypeId**
[`string`](../../../data-types.md) | Value type, for example, `WORK` or `MOBILE`. Required when writing data. The allowed types for each field are returned by the [crm.dictionary.phonetype.list, crm.dictionary.emailtype.list, crm.dictionary.webtype.list, and crm.dictionary.imtype.list](../dictionaries/crm-dictionary-list-rest-v3.md) methods ||
|| **valueType**
[`object`](../../../data-types.md) | Value type with its name: `id`, `name` — full name, for example, `Work Phone`, `nameShort` — short name, for example, `Work Phone`. Read-only ||
|| **value**
[`string`](../../../data-types.md) | Value: phone number, email address, website address, or messenger ID. Required when writing data ||
|#

When writing data, pass an array of objects with the `valueTypeId` and `value` fields:

```json
{
    "phone": [
        {"valueTypeId": "WORK", "value": "+49300000001"},
        {"valueTypeId": "MOBILE", "value": "+49300000002"}
    ]
}
```

In the response, each value is returned with `id` and the `valueType` object:

```json
{
    "phone": [
        {
            "id": 8861,
            "valueTypeId": "WORK",
            "valueType": {
                "id": "WORK",
                "name": "Work Phone",
                "nameShort": "Work Phone"
            },
            "value": "+49300000001"
        }
    ]
}
```

How writing works:

- `crm.company.update` replaces all values of the field with the passed array. Values not included in the array are deleted. Other multiple fields not included in `fields` do not change
- an empty array, for example, `"phone": []`, deletes all values of the field
- a single value is added by the [crm.company.addfieldvalue](./crm-entity-add-field-value-rest-v3.md) method and deleted by [crm.company.deletefieldvalue](./crm-entity-delete-field-value-rest-v3.md). In `value`, pass the object `{"valueTypeId": "WORK", "value": "+49300000001"}`. If such a value already exists, `addfieldvalue` returns the error "The field value already exists."
- one field can contain up to 50 values, and all four fields together up to 100. If the limit is exceeded, the method returns the error "Too many phone multifields."

### File Object {#file}

Field `logo`. To upload a logo, pass the `upload` object with the file name and the content in Base64 or a link to the file:

```json
{
    "logo": {
        "upload": {
            "name": "logo.png",
            "data": "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8DwHwAFBQIAX8jx0gAAAABJRU5ErkJggg=="
        }
    }
}
```

Instead of `data`, you can pass `url` — the file address. You cannot pass `data` and `url` at the same time.

In the response, the field contains the data of the uploaded file:

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | File ID, for example, `"39935"` ||
|| **name**
[`string`](../../../data-types.md) | File name ||
|| **url**
[`string`](../../../data-types.md) | Address for viewing the file in Bitrix24 ||
|| **downloadUrl**
[`string`](../../../data-types.md) | Address for downloading the file via REST. The address contains the webhook authorization data, do not publish it ||
|#

### UTM Tags Object {#utm}

Field `utm`. When writing data, pass only the tags you need: on creation, the other tags are set to `null`; on update, they keep their current values. Each tag is a string of up to 255 characters.

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

### Fields Linking to Other Types {#related}

The `related<type code>Id` and `related<type code>` fields appear when Bitrix24 has a link configured in which the company is subordinate to an SPA. For each parent SPA, the company has a pair of fields:

- `related<type code>Id` — ID of the parent item, for example, `relatedSmartProcess177Id` for the SPA with `entityTypeId = 177`
- `related<type code>` — the parent item as an object of type [CRM item](#crm-item), for example, `relatedSmartProcess177`

The field title in the `crm.company.field.list` response matches the name of the parent SPA, for example, "Equipment purchase". The method checks that an item with the passed ID exists. If there is no such item, the method returns the error 'Field value is not one of the possible values'.

The set of these fields depends on the links configured in Bitrix24. The current list is returned by the [crm.company.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

## Related Objects {#relations}

A related object is requested in `select` by the field name without the `Id` suffix: `assignedBy`, `industry`, `contacts`. The whole object name returns all of its fields, and a dot-notation path returns only the selected ones, for example, `["type.name", "assignedBy.firstName"]`. The path supports one level of nesting: the method does not accept `contact.createdBy.firstName`.

A linked CRM item — `contact`, `contacts`, `lead`, `related<type code>` — returns only `id` when requested by its whole name. Request the other item fields using dot notation. Dot notation gives access to the fields of the [CRM item object](#crm-item) and to any fields of the linked item's type, for example, `["contacts.title", "contacts.phone", "contact.post"]`. Contact fields are described in the [Contact Fields in REST 3.0](./fields-contact.md) article.

If a link is not set, the object is returned as `null`, and an array of objects, for example, `observers` or `contacts`, is returned as an empty array.

### Employee Object {#employee}

Fields `createdBy`, `updatedBy`, `lastActivityBy`, `assignedBy`, `observers`.

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

Fields `contact`, `contacts`, `lead`, `related<type code>`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Item ID ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ID, for example, `3` — contact, `1` — lead ||
|| **canRead**
[`boolean`](../../../data-types.md) | Whether the current user can view the item ||
|| **title**
[`string`](../../../data-types.md) | Item title. For a contact, the last name, first name, and middle name ||
|| **url**
[`string`](../../../data-types.md) | Relative link to the item card, for example, `/crm/contact/details/3211/` ||
|#

### Reference List Value Object {#dictionary}

Fields `type`, `industry`, `employees`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Value ID, for example, `IT` ||
|| **name**
[`string`](../../../data-types.md) | Value name, for example, `Information technology` ||
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

Custom company fields are named `UF_CRM_<code>`, for example, `UF_CRM_1687188367`. They are passed and requested by name in the same way as standard fields. The list of custom company fields is returned by the [crm.company.field.custom.list](../fields/crm-entity-field-custom-list-rest-v3.md) method. My companies have the same fields.

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
- [{#T}](./fields-contact.md)
- [{#T}](./fields-my-company.md)
- [{#T}](../fields/crm-entity-field-list-rest-v3.md)
- [{#T}](../../../rest-v3.md)
