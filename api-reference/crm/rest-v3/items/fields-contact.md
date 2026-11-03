# Contact Fields in REST 3.0

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These fields belong to REST 3.0 methods. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

Contact fields are returned by the [crm.contact.get](./crm-entity-get-rest-v3.md) and [crm.contact.list](./crm-entity-list-rest-v3.md) methods and accepted by the [crm.contact.add](./crm-entity-add-rest-v3.md) and [crm.contact.update](./crm-entity-update-rest-v3.md) methods. Values of multiple fields are added and removed one at a time by the [crm.contact.addfieldvalue](./crm-entity-add-field-value-rest-v3.md) and [crm.contact.deletefieldvalue](./crm-entity-delete-field-value-rest-v3.md) methods. The list of fields with their attributes is returned by the [crm.contact.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

Some fields exist in two forms: as an ID and as a related object, for example, `assignedById` and `assignedBy`. The ID can be passed when writing data, while the related object can only be requested in `select`.

## Which Fields the Methods Accept and Return {#methods}

#|
|| **Method** | **Accepts** | **Returns** ||
|| [crm.contact.add](./crm-entity-add-rest-v3.md) | Fields of the [Contact Object](#item) table that can be edited, and custom fields. At least one of the `name` or `lastName` fields is required; otherwise, the method returns the error "At least one of name or lastName is required." | ID of the created contact `id` ||
|| [crm.contact.update](./crm-entity-update-rest-v3.md) | Contact `id` and the same fields as `add`. Fields not included in `fields` keep their current values | Success flag `true` ||
|| [crm.contact.get](./crm-entity-get-rest-v3.md) | Contact `id` and `select` | Without `select`, only `id`. With `select`, only the listed fields; `id` is not added automatically. A whole related object, for example, `assignedBy`, returns all of its fields, while a dot-notation path, for example, `assignedBy.firstName`, returns only the selected ones. For details, see the [Related Objects](#relations) section ||
|| [crm.contact.list](./crm-entity-list-rest-v3.md) | `select`, `filter`, `order`, `pagination` | Without `select`, only the `id` of each contact. Fields and related objects are requested in `select` the same way as in `get` ||
|| [crm.contact.addfieldvalue](./crm-entity-add-field-value-rest-v3.md), [crm.contact.deletefieldvalue](./crm-entity-delete-field-value-rest-v3.md) | Contact `id`, the multiple field name `fieldName`, and the value `value`. Supported fields are `phone`, `email`, `web`, `im`, `companiesId`, `observersId`, and multiple custom fields. For other fields, the method returns the error "The field is not supported." | Success flag `true` ||
|#

The `add` and `update` methods do not accept fields marked "read-only" and return the `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION` error with the message 'Field "companyId" is read-only.' Related objects, for example, `company` and `assignedBy`, are also read-only: to write data, pass the ID — `companiesId`, `assignedById`.

The method validates fields with values from reference lists when writing data. If the value is not in the reference list, the method returns the error 'Field value is not one of the possible values'. For the contact type, salutation, and source, the error message lists the allowed values.

Boolean fields accept and return `true` or `false`. The values `Y`, `N`, `1`, and `0` are not accepted: the method returns the error 'This request requires data type "bool" in field "opened"'.

The `birthdayTime` field of the `date` type accepts and returns a date in the `YYYY-MM-DD` format, for example, `1990-05-15`. The field does not accept a date with time or a date in the `15.05.1990` format. Fields of the `datetime` type are returned in ISO 8601 format, for example, `2026-11-03T14:42:28+03:00`. In a filter by a `datetime` field, also pass a date with time.

You can filter and sort only by the fields whose description says so. For other fields, the `list` method returns an error stating that the field 'requires attribute "Filterable"' or '"Sortable"'. Dot notation lets you filter by fields of linked CRM items, of phone, email, website, and messenger values, and of the `utm` and `lastCommunication` objects, for example, `["company.title", "=", "Müller GmbH"]` or `["phone.value", "=", "+49300000003"]`. Filtering by fields of employees and reference lists, for example, `assignedBy.id` or `type.name`, is not supported: the method returns the `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION` error. The filter format is described on the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) page and in the [REST API 3.0 Overview](../../../rest-v3.md#filter) article.

{% note warning "" %}

Filtering by the `companiesId` field does not work at the moment: the `crm.contact.list` method responds with HTTP status 500 and the `INTERNAL_SERVER_ERROR` error. To find the contacts of a company, filter by the primary company `companyId` or request the `contactsId` field of the company with the [crm.company.get](./crm-entity-get-rest-v3.md) method.

{% endnote %}

## Contact Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Contact ID. Assigned on creation.

Read-only. Can be filtered and sorted ||
|| **name**
[`string`](../../../data-types.md) | First name, up to 50 characters. When creating a contact, `name` or `lastName` is required.

Can be edited, filtered, and sorted ||
|| **secondName**
[`string`](../../../data-types.md) | Middle name, up to 50 characters.

Can be edited, filtered, and sorted ||
|| **lastName**
[`string`](../../../data-types.md) | Last name, up to 50 characters. When creating a contact, `name` or `lastName` is required.

Can be edited, filtered, and sorted ||
|| **title**
[`string`](../../../data-types.md) | Full name of the contact, which Bitrix24 builds from the last name, first name, and middle name, for example, `Weber Klaus Peter`. To change it, pass `name`, `secondName`, or `lastName`.

Read-only. Cannot be filtered or sorted ||
|| **honorificId**
[`string`](../../../data-types.md) | Salutation ID, for example, `HNR_RU_1`. The values are returned by the [crm.dictionary.honorific.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **honorific**
[```object | null```](#dictionary) | Salutation. An object of type [reference list value](#dictionary).

Read-only. Requested in `select` ||
|| **post**
[`string`](../../../data-types.md) | Position, up to 255 characters.

Can be edited, filtered, and sorted ||
|| **birthdayTime**
[```date | null```](../../../data-types.md) | Date of birth in the `YYYY-MM-DD` format, for example, `1990-05-15`.

Can be edited, filtered, and sorted ||
|| **typeId**
[`string`](../../../data-types.md) | Contact type ID, for example, `CLIENT`. The values are returned by the [crm.dictionary.contacttype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited, filtered, and sorted ||
|| **type**
[```object | null```](#dictionary) | Contact type. An object of type [reference list value](#dictionary).

Read-only. Requested in `select` ||
|| **export**
[`boolean`](../../../data-types.md) | Whether the contact is included in contact export: `true` — yes, `false` — no.

Can be edited, filtered, and sorted ||
|| **photo**
[```object | null```](#file) | Contact photo. An object of type [file](#file). To delete the photo, pass `null`.

Can be edited. Cannot be filtered or sorted ||
|| **companiesId**
[`integer[]`](../../../data-types.md) | IDs of the contact's companies, up to 50 values. Companies are returned by the [crm.company.list](./crm-entity-list-rest-v3.md) method. The passed array replaces the current list of companies. The first company in the array becomes the primary one and is written to `companyId`.

Can be edited. A filter by this field currently returns the `INTERNAL_SERVER_ERROR` error, cannot be sorted ||
|| **companies**
[`object[]`](#crm-item) | Contact companies. An array of objects of type [CRM item](#crm-item). If there are no companies, an empty array.

Read-only. Requested in `select` ||
|| **companyId**
[```integer | null```](../../../data-types.md) | ID of the contact's primary company, which is the first one in the `companiesId` list. To change the primary company, pass it first in `companiesId`.

Read-only. Can be filtered and sorted ||
|| **company**
[```object | null```](#crm-item) | Primary company of the contact. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **leadId**
[```integer | null```](../../../data-types.md) | ID of the lead from which the contact was created. If the contact was not created from a lead, `null`.

Read-only. Can be filtered and sorted ||
|| **lead**
[```object | null```](#crm-item) | Lead from which the contact was created. An object of type [CRM item](#crm-item).

Read-only. Requested in `select` ||
|| **phone**
[`object[]`](#multifield) | Contact phone numbers. An array of objects of type [multiple field value](#multifield). Phone types are returned by the [crm.dictionary.phonetype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

Can be edited. Can be filtered by value fields using dot notation, for example, `["phone.value", "=", "+49300000003"]`. Cannot be sorted ||
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
[`boolean`](../../../data-types.md) | Whether the contact has at least one phone number: `true` — yes, `false` — no.

Read-only. Cannot be filtered or sorted ||
|| **hasEmail**
[`boolean`](../../../data-types.md) | Whether the contact has at least one email address: `true` — yes, `false` — no.

Read-only. Cannot be filtered or sorted ||
|| **hasWeb**
[`boolean`](../../../data-types.md) | Whether the contact has at least one website: `true` — yes, `false` — no.

Read-only. Cannot be filtered or sorted ||
|| **hasIm**
[`boolean`](../../../data-types.md) | Whether the contact has at least one value in the `im` field: `true` — yes, `false` — no.

Read-only. Cannot be filtered or sorted ||
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
[`integer`](../../../data-types.md) | ID of the person responsible for the contact. User IDs are returned by the [user.get](../../../user/user-get.md) method. If the user does not exist, the method returns the error 'Field value is not one of the possible values'.

Can be edited, filtered, and sorted ||
|| **assignedBy**
[`object`](#employee) | Responsible person. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **observersId**
[`integer[]`](../../../data-types.md) | Observer IDs. User IDs are returned by the [user.get](../../../user/user-get.md) method. The passed array replaces the current list of observers.

Can be edited and filtered, cannot be sorted. In a filter, `["observersId", "=", 1]` finds contacts that have user `1` among their observers ||
|| **observers**
[`object[]`](#employee) | Observers. An array of objects of type [employee](#employee). If there are no observers, an empty array.

Read-only. Requested in `select` ||
|| **opened**
[`boolean`](../../../data-types.md) | Whether the contact is available to all users: `true` — yes, `false` — no.

Can be edited, filtered, and sorted ||
|| **comments**
[```string | null```](../../../data-types.md) | Comment on the contact.

Can be edited. Cannot be filtered or sorted ||
|| **utm**
[`object`](#utm) | UTM tags of the contact. An object of type [UTM tags](#utm).

Can be edited. Can be filtered by object fields using dot notation, for example, `["utm.source", "=", "google"]`. Cannot be sorted ||
|| **trackingSourceId**
[```integer | null```](../../../data-types.md) | ID of the end-to-end analytics source. If the source does not exist or is archived, the method returns an error stating that the end-to-end analytics source was not found or is archived.

Can be edited and filtered, cannot be sorted ||
|| **originatorId**
[```string | null```](../../../data-types.md) | ID of the external system the contact was imported from. Use together with `originId` to link the contact to the record in the external system.

Can be edited, filtered, and sorted ||
|| **originId**
[```string | null```](../../../data-types.md) | Contact ID in the external system.

Can be edited, filtered, and sorted ||
|| **originVersion**
[```string | null```](../../../data-types.md) | Record version in the external system.

Can be edited. Cannot be filtered or sorted ||
|| **createdTime**
[`datetime`](../../../data-types.md) | Date and time the contact was created.

Read-only. Can be filtered and sorted ||
|| **createdById**
[`integer`](../../../data-types.md) | ID of the user who created the contact. The user data is in the `createdBy` object.

Read-only. Can be filtered and sorted ||
|| **createdBy**
[`object`](#employee) | User who created the contact. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **updatedTime**
[`datetime`](../../../data-types.md) | Date and time the contact was last updated.

Read-only. Can be filtered and sorted ||
|| **updatedById**
[`integer`](../../../data-types.md) | ID of the user who last updated the contact.

Read-only. Can be filtered and sorted ||
|| **updatedBy**
[`object`](#employee) | User who last updated the contact. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **lastActivityTime**
[`datetime`](../../../data-types.md) | Date and time of the last activity in the contact timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityById**
[`integer`](../../../data-types.md) | ID of the author of the last activity in the timeline.

Read-only. Can be filtered and sorted ||
|| **lastActivityBy**
[`object`](#employee) | Author of the last activity in the timeline. An object of type [employee](#employee).

Read-only. Requested in `select` ||
|| **webformId**
[```integer | null```](../../../data-types.md) | ID of the CRM form through which the contact was created. If the contact was not created by a form, `null`. The list of forms is returned by the [crm.form.list](../../form/crm-form-list.md) method.

Read-only. Can be filtered and sorted ||
|| **webform**
[```object | null```](#webform) | CRM form through which the contact was created. An object of type [CRM form](#webform).

Read-only. Requested in `select` ||
|| **lastCommunication**
[`object`](#last-communication) | Dates of the last communications with the contact. An object of type [last communication](#last-communication).

Read-only. Requested in `select`. Can be filtered by object fields using dot notation, for example, `["lastCommunication.callTime", ">=", "2026-11-01T00:00:00+03:00"]` ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ID. For a contact, `3`.

Read-only ||
|| **canRead**
[`boolean`](../../../data-types.md) | Whether the current user can view the contact: `true` — yes, `false` — no.

Read-only ||
|| **url**
[`string`](../../../data-types.md) | Relative link to the contact card, for example, `/crm/contact/details/3211/`.

Read-only ||
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
[`object`](../../../data-types.md) | Value type with its name: `id`, `name` — full name, for example, `Mobile`, `nameShort` — short name, for example, `Mobile`. Read-only ||
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
            "id": 8871,
            "valueTypeId": "MOBILE",
            "valueType": {
                "id": "MOBILE",
                "name": "Mobile",
                "nameShort": "Mobile"
            },
            "value": "+49300000003"
        }
    ]
}
```

How writing works:

- `crm.contact.update` replaces all values of the field with the passed array. Values not included in the array are deleted. Other multiple fields not included in `fields` do not change
- an empty array, for example, `"phone": []`, deletes all values of the field
- a single value is added by the [crm.contact.addfieldvalue](./crm-entity-add-field-value-rest-v3.md) method and deleted by [crm.contact.deletefieldvalue](./crm-entity-delete-field-value-rest-v3.md). In `value`, pass the object `{"valueTypeId": "MOBILE", "value": "+49300000008"}`. If such a value already exists, `addfieldvalue` returns the error "The field value already exists."
- one field can contain up to 50 values, and all four fields together up to 100. If the limit is exceeded, the method returns the error "Too many phone multifields."

### File Object {#file}

Field `photo`. To upload a photo, pass the `upload` object with the file name and the content in Base64 or a link to the file:

```json
{
    "photo": {
        "upload": {
            "name": "photo.png",
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
[`string`](../../../data-types.md) | File ID, for example, `"39933"` ||
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

## Related Objects {#relations}

A related object is requested in `select` by the field name without the `Id` suffix: `assignedBy`, `honorific`, `company`. The whole object name returns all of its fields, and a dot-notation path returns only the selected ones, for example, `["type.name", "assignedBy.firstName"]`. The path supports one level of nesting: the method does not accept `company.createdBy.firstName`.

A linked CRM item — `company`, `companies`, `lead` — returns only `id` when requested by its whole name. Request the other item fields using dot notation. Dot notation gives access to the fields of the [CRM item object](#crm-item) and to any fields of the linked item's type, for example, `["company.title", "company.phone", "company.industryId"]`. Company fields are described in the [Company Fields in REST 3.0](./fields-company.md) article.

If a link is not set, the object is returned as `null`, and an array of objects, for example, `observers` or `companies`, is returned as an empty array.

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

Fields `company`, `companies`, `lead`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Item ID ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ID: `4` — company, `1` — lead ||
|| **canRead**
[`boolean`](../../../data-types.md) | Whether the current user can view the item ||
|| **title**
[`string`](../../../data-types.md) | Item title ||
|| **url**
[`string`](../../../data-types.md) | Relative link to the item card, for example, `/crm/company/details/3159/` ||
|#

### Reference List Value Object {#dictionary}

Fields `honorific`, `type`, `source`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Value ID, for example, `CLIENT` ||
|| **name**
[`string`](../../../data-types.md) | Value name, for example, `Clients` ||
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

## Custom Fields {#userfields}

Custom contact fields are named `UF_CRM_<code>`, for example, `UF_CRM_1716289811`. They are passed and requested by name in the same way as standard fields. The list of custom contact fields is returned by the [crm.contact.field.custom.list](../fields/crm-entity-field-custom-list-rest-v3.md) method.

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
- [{#T}](./fields-company.md)
- [{#T}](../fields/crm-entity-field-list-rest-v3.md)
- [{#T}](../../../rest-v3.md)
