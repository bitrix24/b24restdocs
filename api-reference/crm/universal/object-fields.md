# CRM Object Fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

CRM object fields are the set of standard item fields that the universal [crm.item.*](./index.md) methods accept in the `fields` parameter and return in the response. This applies to the [crm.item.add](./crm-item-add.md), [crm.item.update](./crm-item-update.md), [crm.item.get](./crm-item-get.md), and [crm.item.list](./crm-item-list.md) methods.

Field names are given in `camelCase`. The methods of individual object types — [crm.lead.*](../leads/index.md), [crm.deal.*](../deals/index.md), [crm.contact.*](../contacts/index.md), [crm.company.*](../companies/index.md) — work with the same data in `UPPER_CASE`. For example, `stageId` is named `STATUS_ID` or `STAGE_ID` there. Their field lists are returned by the [crm.lead.fields](../leads/crm-lead-fields.md), [crm.deal.fields](../deals/crm-deal-fields.md), [crm.contact.fields](../contacts/crm-contact-fields.md), and [crm.company.fields](../companies/crm-company-fields.md) methods. The field names from this article do not work with the methods of individual object types.

> Scope: [`crm`](../../scopes/permissions.md)
>
> Access permissions for CRM object items apply to their fields as well: without permission to read an item and without permission to modify it, the method returns an access error instead of a response

Custom fields, length limits, and data type formats are described separately:

- custom fields — [{#T}](./user-defined-fields/index.md)
- length limits — [{#T}](../field-length-limits.md)
- data types — [{#T}](../../data-types.md) and [{#T}](../data-types.md)

## How the Item Field Set Is Built

The field set of an item depends on the CRM object type `entityTypeId`. Fields fall into five groups:

- **Common fields.** Available for every object type. Described in the [{#T}](#common) section
- **Fields by object type.** Available only for a lead, deal, contact, company, estimate, invoice, or smart process. Described in the [{#T}](#by-object) section
- **Custom fields `ufCrm...`.** Created in Bitrix24. Covered by a row in the table of the [{#T}](#common) section, with details in [{#T}](./user-defined-fields/index.md)
- **Parent fields `parentId...`.** Link an item to an item of another object type. Covered by a row in the table of the [{#T}](#common) section
- **Multifield `fm`.** Stores contact details. Described in the [{#T}](#fm) section

For smart processes, the field set also depends on the object type settings — see the [{#T}](#spa) section. For an invoice, the field set is fixed — see the [{#T}](#invoice) section.

Choose the source of field information that matches your task:

#|
|| **Task** | **What to use** ||
|| Learn the composition and purpose of standard fields | The tables in the [{#T}](#common) and [{#T}](#by-object) sections ||
|| Retrieve the current field list of a specific object type, including custom fields, with the `isRequired`, `isReadOnly`, `isImmutable`, and `isMultiple` flags | [crm.item.fields](./crm-item-fields.md) ||
|| Find out which fields arrive in the response of a specific method | The [{#T}](#select) section ||
|| Learn the rules for converting names between `UPPER_CASE` and `camelCase` | [Field Naming Conventions](./index.md#field-naming-conventions) ||
|#

{% note warning %}

The `crm.item.fields` method returns the system field `contacts` — for a contact, `companies` is returned instead. Neither works over REST: they do not arrive in the `crm.item.get` and `crm.item.list` responses, and the method returns error `100` when writing them. Link contacts and companies with the `contactIds` and `companyIds` fields.

{% endnote %}

## How to Read the Field Tables

### Access Column {#access}

The Access column states what you can do with a field through the `crm.item.*` methods:

#|
|| **Value** | **Meaning** ||
|| Read and write | The field is present in the `crm.item.fields` response. Its value arrives in responses and is accepted by `crm.item.add` and `crm.item.update` ||
|| Read-only | The field is present in the `crm.item.fields` response with the `isReadOnly` flag. Its value arrives in responses and is ignored on write without an error ||
|| Set on creation only | The field is present in the `crm.item.fields` response with the `isImmutable` flag. Its value is accepted by `crm.item.add` and ignored by `crm.item.update` ||
|| Response only | The field is absent from the `crm.item.fields` response. Its value arrives in `crm.item.get` and `crm.item.list` and is ignored on write without an error ||
|| Not returned | The field is hidden: it arrives in no response and is not accepted on write. It does work in `filter` and `order` — see the [{#T}](#select) section ||
|#

When access to a field differs between object types, the column lists both values.

Among the fields in the tables below, the only one that interrupts the request when passed in `fields` is `id` in `crm.item.update` — this is stated in its description. For fields in `select`, `filter`, and `order`, see the [{#T}](#select) section.

### Fields in select, filter, and order {#select}

The fields from the tables can be listed in the `select`, `filter`, and `order` parameters of the [crm.item.list](./crm-item-list.md) method. The [crm.item.get](./crm-item-get.md) method does not accept these parameters and always returns the complete item.

Exceptions:

- the system fields `entityTypeId`, `contacts`, and `companies` are dropped from `select`, and in `filter` and `order` they interrupt the request with an error. These fields do not work over REST: the method returns error `100` on write, and contacts and companies must be linked with the `contactIds` and `companyIds` fields
- fields marked Not returned are dropped from `select`, but they do work in `filter` and `order`
- the multifield `fm` arrives only when `select` is omitted or equals `["*"]`; in `filter` and `order` the method returns error `100`

### Deprecation Markers {#deprecated}

Two markers are used in field descriptions:

- **Deprecated, use `X`** — the field has a current replacement: another field or a separate set of methods, named in the description. The `crm.item.fields` method returns such a field with the `isDeprecated` flag
- **Legacy field** — the field is left over from earlier CRM versions, has no replacement, and is absent from the `crm.item.fields` response as well. Its value may be empty. Do not use such fields in new integrations

## Field Values

### Required Fields {#required}

There are no required standard fields: an item is created with an empty `fields` parameter. If you do not pass a name, it is filled in automatically — for example, `Deal #8455` in the `title` field of a deal or `Contact #2759` in the `lastName` field of a contact. The `crm.item.*` methods do not check whether custom fields are required.

### Value Formats {#value-formats}

- `boolean` — the strings `Y` and `N`. On write, the values `true`, `"true"`, `"y"`, and `"Y"` are treated as true. Any other value is stored as `N`
- `datetime` and `date` — an ISO-8601 string with the Bitrix24 time zone offset, for example `2026-08-25T16:16:04+03:00`. The `crm.item.*` methods return fields of the `date` type with a time component as well, rather than in the short `YYYY-MM-DD` format from the data type reference
- fields with the `Short` suffix — deprecated copies of dates. Despite the name, the `crm.item.*` methods return them in the same full ISO-8601 format; there is no short format in the response
- `user` — an integer user identifier, `user[]` — an array of identifiers
- `file` in the standard fields `photo` and `logo` — an integer file identifier. On write, you also pass the identifier of an already uploaded file. If you pass a file name and base64 content instead, the method returns the `FILE_NOT_FOUND` error

### Multifield Structure {#fm}

The `fm` field of a lead, contact, and company stores an array of multifields — phone numbers, emails, messengers, and websites. Each array element consists of four keys:

- `id` — unique identifier
- `typeId` — multifield type: `PHONE`, `EMAIL`, `WEB`, `IM`, or `LINK`
- `valueType` — value type, for example `WORK`, `HOME`, or `MAILING`
- `value` — the value

The `crm.item.update` method only adds values: the array you pass does not replace the existing multifields but extends them. The `id` key is ignored on write, so you cannot modify or delete a value through `crm.item.*` — use the methods of individual object types instead, for example [crm.contact.update](../contacts/crm-contact-update.md) with the `PHONE` field.

The complete lists of `typeId` and `valueType` values are given in the description of the [{#T}](../data-types.md#crm_multifield) type.

## Sample Request {#example}

The request creates a deal and fills in three fields from the [{#T}](#deal) table. In the response, the method returns an `item` object with the fields of the created deal — the response structure is described on the [{#T}](./crm-item-add.md) page.

```bash
curl -X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{"entityTypeId":2,"fields":{"title":"Equipment supply","opportunity":150000,"currencyId":"EUR"}}' \
https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.add
```

## Common Fields {#common}

#|
|| **Name**
`type` | **Access** | **Description** ||
|| **assignedById**
[`user`](../../data-types.md#standart-objects) | Read and write | Identifier of the user responsible for the item ||
|| **createdBy**
[`user`](../../data-types.md#standart-objects) | Read-only | Identifier of the user who created the item ||
|| **createdTime**
[`datetime`](../../data-types.md#standart-types) | Read-only | Date and time when the item was created ||
|| **entityTypeId**
[`integer`](../../data-types.md#standart-types) | Response only | Identifier of the [CRM object type](../data-types.md#object_type). This is not a field of the item itself: the methods add it to the response only when `select` is omitted or equals `["*"]` ||
|| **id**
[`integer`](../../data-types.md#standart-types) | Read-only | Identifier of the item, unique within the object type. In `crm.item.add` the value is ignored, and in `crm.item.update` it interrupts the request with the error `Setting value for Primary ID is not allowed, it is read-only field` — do not pass `id` inside `fields` ||
|| **lastActivityBy**
[`user`](../../data-types.md#standart-objects) | Read and write | Identifier of the user who last interacted in the timeline ||
|| **lastActivityTime**
[`datetime`](../../data-types.md#standart-types) | Read and write | Date and time of the last activity in the timeline ||
|| **lastCommunicationCallTime**
[`datetime`](../../data-types.md#standart-types) | Read-only | Date and time of the last call ||
|| **lastCommunicationEmailTime**
[`datetime`](../../data-types.md#standart-types) | Read-only | Date and time of the last email ||
|| **lastCommunicationImolTime**
[`datetime`](../../data-types.md#standart-types) | Read-only | Date and time of the last message in Open Channels ||
|| **lastCommunicationTime**
[`string`](../../data-types.md#standart-types) | Read-only | Date and time of the last communication over any channel. The `crm.item.fields` method declares the field as `string`, while the value arrives in the same ISO-8601 format as the fields of the `datetime` type ||
|| **lastCommunicationWebformTime**
[`datetime`](../../data-types.md#standart-types) | Read-only | Date and time when a CRM form was last submitted ||
|| **opened**
[`boolean`](../../data-types.md#standart-types) | Read and write | Whether the item is available to all employees. `Y` — to everyone, `N` — only to the responsible user, their supervisors, and observers ||
|| **parentId...**
[`crm_entity`](../data-types.md#data-types) | Read and write | Parent field: the identifier of an item of another CRM object type linked to this item.

The field name consists of the `parentId` prefix and the identifier of the parent object type: a link to an item of the type `entityTypeId = 177` produces the field `parentId177`.

The fields appear only for those relations between types that are configured in Bitrix24 ||
|| **ufCrm...**
[`crm_userfield`](../data-types.md#data-types) | Read and write | Custom field. Details are in [{#T}](./user-defined-fields/index.md).

Values of multiple fields arrive as an array.

The value of a field of the `file` type arrives as an object:

- `id` — file identifier
- `url` — link to the file in Bitrix24
- `urlMachine` — link to the file for the application ||
|| **updatedBy**
[`user`](../../data-types.md#standart-objects) | Read-only | Identifier of the user who last modified the item ||
|| **updatedTime**
[`datetime`](../../data-types.md#standart-types) | Read-only | Date and time when the item was last modified ||
|| **utmCampaign**
[`string`](../../data-types.md#standart-types) | Read and write, for an invoice and smart processes — response only | Designation of the advertising campaign ||
|| **utmContent**
[`string`](../../data-types.md#standart-types) | Read and write, for an invoice and smart processes — response only | Content of the campaign, for example for contextual ads ||
|| **utmMedium**
[`string`](../../data-types.md#standart-types) | Read and write, for an invoice and smart processes — response only | Type of traffic. Possible values:

- `CPC` — ads
- `CPM` — banners ||
|| **utmSource**
[`string`](../../data-types.md#standart-types) | Read and write, for an invoice and smart processes — response only | Advertising system: `Google-Adwords` and others ||
|| **utmTerm**
[`string`](../../data-types.md#standart-types) | Read and write, for an invoice and smart processes — response only | Search condition of the campaign, for example keywords of contextual advertising ||
|| **webformId**
[`integer`](../../data-types.md#standart-types) | Read and write, for a deal, contact, and company — set on creation only | Identifier of the CRM form that created the item. The `crm.item.fields` method returns the field type as `crm_webform` ||
|#

## Fields by Object Type {#by-object}

The numeric object type identifier is required in every `crm.item.*` call. The complete table of types is available in the [CRM object types](../data-types.md#object_type) reference. The values of the Access column in the tables below are explained in the [{#T}](#access) section.

#|
|| **Object type** | **`entityTypeId`** | **Fields** ||
|| Lead | 1 | [{#T}](#lead) ||
|| Deal | 2 | [{#T}](#deal) ||
|| Contact | 3 | [{#T}](#contact) ||
|| Company | 4 | [{#T}](#company) ||
|| Estimate | 7 | [{#T}](#quote) ||
|| Invoice | 31 | [{#T}](#invoice) ||
|| Smart Process | from 128 | [{#T}](#spa) ||
|#

### Lead {#lead}

`entityTypeId = 1`

#|
|| **Name**
`type` | **Access** | **Description** ||
|| **address**
[`text`](../../data-types.md#standart-types) | Response only | Address. Legacy field ||
|| **birthdate**
[`date`](../../data-types.md#standart-types) | Read and write | Date of birth ||
|| **comments**
[`text`](../../data-types.md#standart-types) | Read and write | Comment ||
|| **companyId**
[`crm_company`](../data-types.md#data-types) | Read and write | Identifier of the company linked to the item ||
|| **companyTitle**
[`string`](../../data-types.md#standart-types) | Read and write | Company name ||
|| **contactId**
[`crm_contact`](../data-types.md#data-types) | Read and write | Identifier of the contact linked to the item. Deprecated, use `contactIds` ||
|| **contactIds**
[`crm_contact[]`](../data-types.md#data-types) | Read and write | List of identifiers of the contacts linked to the item ||
|| **currencyId**
[`crm_currency`](../data-types.md#data-types) | Read and write | Identifier of the item's currency ||
|| **dateClosed**
[`datetime`](../../data-types.md#standart-types) | Read-only | Date and time when the item was closed ||
|| **dateCreateShort**
[`datetime`](../../data-types.md#standart-types) | Response only | Deprecated copy of the item creation date. Arrives in the full ISO-8601 format ||
|| **dateModifyShort**
[`datetime`](../../data-types.md#standart-types) | Response only | Deprecated copy of the item modification date. Arrives in the full ISO-8601 format ||
|| **email**
[`string`](../../data-types.md#standart-types) | Response only | The first email address from the `fm` multifield. To change it, pass `fm` ||
|| **emailHome**
[`string`](../../data-types.md#standart-types) | Response only | Email address with the `HOME` type from the `fm` multifield ||
|| **emailMailing**
[`string`](../../data-types.md#standart-types) | Response only | Email address with the `MAILING` type from the `fm` multifield ||
|| **emailWork**
[`string`](../../data-types.md#standart-types) | Response only | Email address with the `WORK` type from the `fm` multifield ||
|| **fm**
[`crm_multifield`](../data-types.md#crm_multifield) | Read and write | Array of multifields. The key structure is described in the [{#T}](#fm) section ||
|| **hasEmail**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the item has an email ||
|| **hasImol**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the item has Open Channels ||
|| **hasPhone**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the item has a phone ||
|| **honorific**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the salutation type ||
|| **icq**
[`string`](../../data-types.md#standart-types) | Response only | ICQ. Legacy field ||
|| **imol**
[`string`](../../data-types.md#standart-types) | Response only | Open Channel identifier from the `fm` multifield ||
|| **isConvert**
[`boolean`](../../data-types.md#standart-types) | Response only | Whether the lead has been converted. Legacy field ||
|| **isManualOpportunity**
[`boolean`](../../data-types.md#standart-types) | Read and write | Whether manual mode for calculating the amount is enabled ||
|| **isReturnCustomer**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the lead is a repeat one ||
|| **lastName**
[`string`](../../data-types.md#standart-types) | Read and write | Last name ||
|| **login**
[`string`](../../data-types.md#standart-types) | Response only | Login. Legacy field ||
|| **movedBy**
[`user`](../../data-types.md#standart-objects) | Read-only | Identifier of the user who last changed the stage ||
|| **movedTime**
[`datetime`](../../data-types.md#standart-types) | Read-only | Date and time of the last stage change ||
|| **name**
[`string`](../../data-types.md#standart-types) | Read and write | First name ||
|| **observers**
[`user[]`](../../data-types.md#standart-objects) | Read and write | List of identifiers of the users who are observers ||
|| **opportunity**
[`double`](../../data-types.md#standart-types) | Read and write | Amount ||
|| **originId**
[`string`](../../data-types.md#standart-types) | Read and write | Identifier of the item in the external source ||
|| **originatorId**
[`string`](../../data-types.md#standart-types) | Read and write | External source ||
|| **phone**
[`string`](../../data-types.md#standart-types) | Response only | The first phone number from the `fm` multifield. To change it, pass `fm` ||
|| **phoneMailing**
[`string`](../../data-types.md#standart-types) | Response only | Phone number with the `MAILING` type from the `fm` multifield ||
|| **phoneMobile**
[`string`](../../data-types.md#standart-types) | Response only | Phone number with the `MOBILE` type from the `fm` multifield ||
|| **phoneWork**
[`string`](../../data-types.md#standart-types) | Response only | Phone number with the `WORK` type from the `fm` multifield ||
|| **post**
[`string`](../../data-types.md#standart-types) | Read and write | Position ||
|| **productId**
[`string`](../../data-types.md#standart-types) | Response only | Identifier of the product. Legacy field ||
|| **searchContent**
[`text`](../../data-types.md#standart-types) | Response only | System field: information for full-text search ||
|| **secondName**
[`string`](../../data-types.md#standart-types) | Read and write | Middle name ||
|| **shortName**
[`string`](../../data-types.md#standart-types) | Response only | Last name with initials: `Weber K.` Legacy field ||
|| **skype**
[`string`](../../data-types.md#standart-types) | Response only | Skype. Legacy field ||
|| **sourceDescription**
[`text`](../../data-types.md#standart-types) | Read and write | Additional information about the source ||
|| **sourceId**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the source type ||
|| **stageId**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the item's stage ||
|| **stageSemanticId**
[`string`](../../data-types.md#standart-types) | Read-only | Group of the stage. Possible values:

- `P` — in progress
- `S` — successful
- `F` — unsuccessful ||
|| **statusDescription**
[`text`](../../data-types.md#standart-types) | Read and write | Additional information about the stage ||
|| **title**
[`string`](../../data-types.md#standart-types) | Read and write | Name of the item ||
|#

### Deal {#deal}

`entityTypeId = 2`

#|
|| **Name**
`type` | **Access** | **Description** ||
|| **additionalInfo**
[`string`](../../data-types.md#standart-types) | Read and write | Additional information ||
|| **begindate**
[`date`](../../data-types.md#standart-types) | Read and write | Start date of the item ||
|| **begindateShort**
[`datetime`](../../data-types.md#standart-types) | Response only | Deprecated copy of the item start date. Arrives in the full ISO-8601 format ||
|| **categoryId**
[`crm_category`](../data-types.md#data-types) | Read and write | Identifier of the item's funnel ||
|| **closed**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the deal is closed ||
|| **closedate**
[`date`](../../data-types.md#standart-types) | Read and write | Completion date of the item ||
|| **closedateShort**
[`datetime`](../../data-types.md#standart-types) | Response only | Deprecated copy of the item completion date. Arrives in the full ISO-8601 format ||
|| **comments**
[`text`](../../data-types.md#standart-types) | Read and write | Comment ||
|| **companyId**
[`crm_company`](../data-types.md#data-types) | Read and write | Identifier of the company linked to the item ||
|| **contactId**
[`crm_contact`](../data-types.md#data-types) | Read and write | Identifier of the contact linked to the item. Deprecated, use `contactIds` ||
|| **contactIds**
[`crm_contact[]`](../data-types.md#data-types) | Read and write | List of identifiers of the contacts linked to the item ||
|| **currencyId**
[`crm_currency`](../data-types.md#data-types) | Read and write | Identifier of the item's currency ||
|| **dateCreateShort**
[`datetime`](../../data-types.md#standart-types) | Response only | Deprecated copy of the item creation date. Arrives in the full ISO-8601 format ||
|| **dateModifyShort**
[`datetime`](../../data-types.md#standart-types) | Response only | Deprecated copy of the item modification date. Arrives in the full ISO-8601 format ||
|| **eventDate**
[`datetime`](../../data-types.md#standart-types) | Response only | Event date. Legacy field ||
|| **eventDateShort**
[`datetime`](../../data-types.md#standart-types) | Response only | Deprecated copy of the event date. Arrives in the full ISO-8601 format ||
|| **eventDescription**
[`text`](../../data-types.md#standart-types) | Response only | Description of the event. Legacy field ||
|| **eventId**
[`crm_status`](../data-types.md#data-types) | Response only | String identifier of the event type. Legacy field ||
|| **hasProducts**
[`boolean`](../../data-types.md#standart-types) | Response only | Whether the item contains products ||
|| **isLose**
[`boolean`](../../data-types.md#standart-types) | Response only | Whether the deal is lost ||
|| **isManualOpportunity**
[`boolean`](../../data-types.md#standart-types) | Read and write | Whether manual mode for calculating the amount is enabled ||
|| **isNew**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the deal is new ||
|| **isRecurring**
[`boolean`](../../data-types.md#standart-types) | Read and write | Whether the deal is recurring ||
|| **isRepeatedApproach**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the deal is a repeated approach ||
|| **isReturnCustomer**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the deal is a repeat one ||
|| **isWon**
[`boolean`](../../data-types.md#standart-types) | Response only | Whether the deal is won ||
|| **isWork**
[`boolean`](../../data-types.md#standart-types) | Response only | Whether the deal is in progress ||
|| **leadId**
[`crm_lead`](../data-types.md#data-types) | Read and write | Identifier of the lead the item was created from ||
|| **locationId**
[`location`](../data-types.md#data-types) | Read and write | System field: identifier of the location ||
|| **lostAmount**
[`integer`](../../data-types.md#standart-types) | Response only | Amount lost ||
|| **movedBy**
[`user`](../../data-types.md#standart-objects) | Read-only | Identifier of the user who last changed the stage ||
|| **movedTime**
[`datetime`](../../data-types.md#standart-types) | Read-only | Date and time of the last stage change ||
|| **mycompanyId**
[`crm_company`](../data-types.md#data-types) | Read and write | Identifier of "my" company ||
|| **observers**
[`user[]`](../../data-types.md#standart-objects) | Read and write | List of identifiers of the users who are observers ||
|| **opportunity**
[`double`](../../data-types.md#standart-types) | Read and write | Amount ||
|| **orderStage**
[`string`](../../data-types.md#standart-types) | Response only | Payment status of the deal ||
|| **originId**
[`string`](../../data-types.md#standart-types) | Read and write | Identifier of the item in the external source ||
|| **originatorId**
[`string`](../../data-types.md#standart-types) | Read and write | External source ||
|| **previousStageId**
[`crm_status`](../data-types.md#data-types) | Read-only | String identifier of the previous stage ||
|| **probability**
[`integer`](../../data-types.md#standart-types) | Read and write | Probability, % ||
|| **productId**
[`string`](../../data-types.md#standart-types) | Response only | Identifier of the product. Legacy field ||
|| **quoteId**
[`crm_quote`](../data-types.md#data-types) | Read and write | Identifier of the estimate linked to the item ||
|| **receivedAmount**
[`integer`](../../data-types.md#standart-types) | Response only | Amount received ||
|| **searchContent**
[`text`](../../data-types.md#standart-types) | Response only | System field: information for full-text search ||
|| **sourceDescription**
[`text`](../../data-types.md#standart-types) | Read and write | Additional information about the source ||
|| **sourceId**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the source type ||
|| **stageId**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the item's stage ||
|| **stageSemanticId**
[`string`](../../data-types.md#standart-types) | Read-only | Group of the stage. Possible values:

- `P` — in progress
- `S` — successful
- `F` — unsuccessful ||
|| **taxValue**
[`double`](../../data-types.md#standart-types) | Read and write | Tax amount ||
|| **title**
[`string`](../../data-types.md#standart-types) | Read and write | Name of the item ||
|| **typeId**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the deal type. The possible values are returned by the [crm.status.list](../status/crm-status-list.md) method for the `DEAL_TYPE` directory ||
|#

### Contact {#contact}

`entityTypeId = 3`

#|
|| **Name**
`type` | **Access** | **Description** ||
|| **address**
[`text`](../../data-types.md#standart-types) | Response only | Address. Legacy field ||
|| **birthdate**
[`date`](../../data-types.md#standart-types) | Read and write | Date of birth ||
|| **categoryId**
[`crm_category`](../data-types.md#data-types) | Read and write | Identifier of the item's funnel ||
|| **comments**
[`text`](../../data-types.md#standart-types) | Read and write | Comment ||
|| **companyId**
[`crm_company`](../data-types.md#data-types) | Read and write | Identifier of the company linked to the item. Deprecated, use `companyIds` ||
|| **companyIds**
[`crm_company[]`](../data-types.md#data-types) | Read and write | List of identifiers of the companies linked to the item ||
|| **email**
[`string`](../../data-types.md#standart-types) | Response only | The first email address from the `fm` multifield. To change it, pass `fm` ||
|| **emailHome**
[`string`](../../data-types.md#standart-types) | Response only | Email address with the `HOME` type from the `fm` multifield ||
|| **emailMailing**
[`string`](../../data-types.md#standart-types) | Response only | Email address with the `MAILING` type from the `fm` multifield ||
|| **emailWork**
[`string`](../../data-types.md#standart-types) | Response only | Email address with the `WORK` type from the `fm` multifield ||
|| **export**
[`boolean`](../../data-types.md#standart-types) | Read and write | Whether exporting the contact is allowed ||
|| **fm**
[`crm_multifield`](../data-types.md#crm_multifield) | Read and write | Array of multifields. The key structure is described in the [{#T}](#fm) section ||
|| **hasEmail**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the item has an email ||
|| **hasImol**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the item has Open Channels ||
|| **hasPhone**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the item has a phone ||
|| **honorific**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the salutation type ||
|| **imol**
[`string`](../../data-types.md#standart-types) | Response only | Open Channel identifier from the `fm` multifield ||
|| **lastName**
[`string`](../../data-types.md#standart-types) | Read and write | Last name ||
|| **leadId**
[`crm_lead`](../data-types.md#data-types) | Read and write | Identifier of the lead the item was created from ||
|| **login**
[`string`](../../data-types.md#standart-types) | Response only | Login. Legacy field ||
|| **name**
[`string`](../../data-types.md#standart-types) | Read and write | First name ||
|| **observers**
[`user[]`](../../data-types.md#standart-objects) | Read and write | List of identifiers of the users who are observers ||
|| **originId**
[`string`](../../data-types.md#standart-types) | Read and write | Identifier of the item in the external source ||
|| **originVersion**
[`string`](../../data-types.md#standart-types) | Read and write | Version of the original ||
|| **originatorId**
[`string`](../../data-types.md#standart-types) | Read and write | External source ||
|| **phone**
[`string`](../../data-types.md#standart-types) | Response only | The first phone number from the `fm` multifield. To change it, pass `fm` ||
|| **phoneMailing**
[`string`](../../data-types.md#standart-types) | Response only | Phone number with the `MAILING` type from the `fm` multifield ||
|| **phoneMobile**
[`string`](../../data-types.md#standart-types) | Response only | Phone number with the `MOBILE` type from the `fm` multifield ||
|| **phoneWork**
[`string`](../../data-types.md#standart-types) | Response only | Phone number with the `WORK` type from the `fm` multifield ||
|| **photo**
[`file`](../../data-types.md#standart-types) | Read and write | Photo. Arrives and is written as a file identifier ||
|| **post**
[`string`](../../data-types.md#standart-types) | Read and write | Position ||
|| **searchContent**
[`text`](../../data-types.md#standart-types) | Response only | System field: information for full-text search ||
|| **secondName**
[`string`](../../data-types.md#standart-types) | Read and write | Middle name ||
|| **shortName**
[`string`](../../data-types.md#standart-types) | Response only | Last name with initials: `Weber K.` Legacy field ||
|| **sourceDescription**
[`text`](../../data-types.md#standart-types) | Read and write | Additional information about the source ||
|| **sourceId**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the source type ||
|| **typeId**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the contact type: client, supplier, partner, and others. The possible values are returned by the [crm.status.list](../status/crm-status-list.md) method for the `CONTACT_TYPE` directory ||
|#

### Company {#company}

`entityTypeId = 4`

The `ufAccountantSign`, `ufDirectorSign`, `ufLogo`, and `ufStamp` fields are predefined custom fields of the document generator. The `ufCrm...` pattern does not apply to them, and they are absent from the `crm.item.fields` response.

#|
|| **Name**
`type` | **Access** | **Description** ||
|| **address**
[`text`](../../data-types.md#standart-types) | Response only | Address. Legacy field ||
|| **addressLegal**
[`text`](../../data-types.md#standart-types) | Response only | Legal address. Legacy field ||
|| **bankingDetails**
[`text`](../../data-types.md#standart-types) | Read and write | Banking details. Deprecated, use [requisites](../requisites/index.md) ||
|| **categoryId**
[`crm_category`](../data-types.md#data-types) | Read and write | Identifier of the item's funnel ||
|| **comments**
[`text`](../../data-types.md#standart-types) | Read and write | Comment ||
|| **contactIds**
[`crm_contact[]`](../data-types.md#data-types) | Read and write | List of identifiers of the contacts linked to the item ||
|| **currencyId**
[`crm_currency`](../data-types.md#data-types) | Read and write | Identifier of the item's currency ||
|| **email**
[`string`](../../data-types.md#standart-types) | Response only | The first email address from the `fm` multifield. To change it, pass `fm` ||
|| **emailHome**
[`string`](../../data-types.md#standart-types) | Response only | Email address with the `HOME` type from the `fm` multifield ||
|| **emailMailing**
[`string`](../../data-types.md#standart-types) | Response only | Email address with the `MAILING` type from the `fm` multifield ||
|| **emailWork**
[`string`](../../data-types.md#standart-types) | Response only | Email address with the `WORK` type from the `fm` multifield ||
|| **employees**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the number of employees ||
|| **fm**
[`crm_multifield`](../data-types.md#crm_multifield) | Read and write | Array of multifields. The key structure is described in the [{#T}](#fm) section ||
|| **hasEmail**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the item has an email ||
|| **hasImol**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the item has Open Channels ||
|| **hasPhone**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the item has a phone ||
|| **imol**
[`string`](../../data-types.md#standart-types) | Response only | Open Channel identifier from the `fm` multifield ||
|| **industry**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the industry ||
|| **isMyCompany**
[`boolean`](../../data-types.md#standart-types) | Set on creation only | Whether the company is "my" company ||
|| **leadId**
[`crm_lead`](../data-types.md#data-types) | Read and write | Identifier of the lead the item was created from ||
|| **logo**
[`file`](../../data-types.md#standart-types) | Read and write | Logo. Arrives and is written as a file identifier ||
|| **observers**
[`user[]`](../../data-types.md#standart-objects) | Read and write | List of identifiers of the users who are observers ||
|| **originId**
[`string`](../../data-types.md#standart-types) | Read and write | Identifier of the item in the external source ||
|| **originVersion**
[`string`](../../data-types.md#standart-types) | Read and write | Version of the original ||
|| **originatorId**
[`string`](../../data-types.md#standart-types) | Read and write | External source ||
|| **phone**
[`string`](../../data-types.md#standart-types) | Response only | The first phone number from the `fm` multifield. To change it, pass `fm` ||
|| **phoneMailing**
[`string`](../../data-types.md#standart-types) | Response only | Phone number with the `MAILING` type from the `fm` multifield ||
|| **phoneMobile**
[`string`](../../data-types.md#standart-types) | Response only | Phone number with the `MOBILE` type from the `fm` multifield ||
|| **phoneWork**
[`string`](../../data-types.md#standart-types) | Response only | Phone number with the `WORK` type from the `fm` multifield ||
|| **revenue**
[`double`](../../data-types.md#standart-types) | Read and write | Annual turnover ||
|| **searchContent**
[`text`](../../data-types.md#standart-types) | Response only | System field: information for full-text search ||
|| **title**
[`string`](../../data-types.md#standart-types) | Read and write | Name of the item ||
|| **typeId**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the company type: client, supplier, partner, and others. The possible values are returned by the [crm.status.list](../status/crm-status-list.md) method for the `COMPANY_TYPE` directory ||
|| **ufAccountantSign**
[`file`](../../data-types.md#standart-types) | Response only | Chief accountant's signature for the document generator ||
|| **ufDirectorSign**
[`file`](../../data-types.md#standart-types) | Response only | Director's signature for the document generator ||
|| **ufLogo**
[`file`](../../data-types.md#standart-types) | Response only | Logo for the document generator ||
|| **ufStamp**
[`file`](../../data-types.md#standart-types) | Response only | Company seal for the document generator ||
|#

### Estimate {#quote}

`entityTypeId = 7`

The `commentsType`, `contentType`, and `termsType` fields use a shared directory of text formats:

- `0` — unknown
- `1` — text
- `2` — BB-code
- `3` — HTML

#|
|| **Name**
`type` | **Access** | **Description** ||
|| **actualDate**
[`date`](../../data-types.md#standart-types) | Read and write | Date until which the estimate is valid ||
|| **begindate**
[`date`](../../data-types.md#standart-types) | Read and write | Start date of the item ||
|| **begindateShort**
[`datetime`](../../data-types.md#standart-types) | Response only | Deprecated copy of the item start date. Arrives in the full ISO-8601 format ||
|| **clientAddr**
[`string`](../../data-types.md#standart-types) | Response only | Client address ||
|| **clientContact**
[`string`](../../data-types.md#standart-types) | Response only | Client contacts ||
|| **clientEmail**
[`string`](../../data-types.md#standart-types) | Response only | Client email ||
|| **clientPhone**
[`string`](../../data-types.md#standart-types) | Response only | Client phone ||
|| **clientTitle**
[`string`](../../data-types.md#standart-types) | Response only | Client name ||
|| **clientTpId**
[`string`](../../data-types.md#standart-types) | Response only | Client TIN ||
|| **clientTpaId**
[`string`](../../data-types.md#standart-types) | Response only | Client code in the chamber of commerce ||
|| **closed**
[`boolean`](../../data-types.md#standart-types) | Read-only | Whether the estimate is closed ||
|| **closedate**
[`date`](../../data-types.md#standart-types) | Read and write | Completion date of the item ||
|| **closedateShort**
[`datetime`](../../data-types.md#standart-types) | Response only | Deprecated copy of the item completion date. Arrives in the full ISO-8601 format ||
|| **comments**
[`text`](../../data-types.md#standart-types) | Read and write | Comment ||
|| **commentsType**
[`integer`](../../data-types.md#standart-types) | Response only | Identifier of the comment format from the shared directory of text formats ||
|| **companyId**
[`crm_company`](../data-types.md#data-types) | Read and write | Identifier of the company linked to the item ||
|| **contactId**
[`crm_contact`](../data-types.md#data-types) | Read and write | Identifier of the contact linked to the item. Deprecated, use `contactIds` ||
|| **contactIds**
[`crm_contact[]`](../data-types.md#data-types) | Read and write | List of identifiers of the contacts linked to the item ||
|| **content**
[`text`](../../data-types.md#standart-types) | Read and write | Content ||
|| **contentType**
[`integer`](../../data-types.md#standart-types) | Response only | Identifier of the content format from the shared directory of text formats ||
|| **currencyId**
[`crm_currency`](../data-types.md#data-types) | Read and write | Identifier of the item's currency ||
|| **dateCreateShort**
[`datetime`](../../data-types.md#standart-types) | Response only | Deprecated copy of the item creation date. Arrives in the full ISO-8601 format ||
|| **dateModifyShort**
[`datetime`](../../data-types.md#standart-types) | Response only | Deprecated copy of the item modification date. Arrives in the full ISO-8601 format ||
|| **dealId**
[`crm_deal`](../data-types.md#data-types) | Read and write | Identifier of the deal linked to the item ||
|| **hasProducts**
[`boolean`](../../data-types.md#standart-types) | Response only | Whether the item contains products ||
|| **isManualOpportunity**
[`boolean`](../../data-types.md#standart-types) | Read and write | Whether manual mode for calculating the amount is enabled ||
|| **leadId**
[`crm_lead`](../data-types.md#data-types) | Read and write | Identifier of the lead the item was created from ||
|| **locationId**
[`location`](../data-types.md#data-types) | Read and write | System field: identifier of the location ||
|| **mycompanyId**
[`crm_company`](../data-types.md#data-types) | Read and write | Identifier of "my" company ||
|| **opportunity**
[`double`](../../data-types.md#standart-types) | Read and write | Amount ||
|| **personTypeId**
[`integer`](../../data-types.md#standart-types) | Read-only | Identifier of the payer type ||
|| **quoteNumber**
[`string`](../../data-types.md#standart-types) | Read and write | Estimate number ||
|| **searchContent**
[`text`](../../data-types.md#standart-types) | Response only | System field: information for full-text search ||
|| **stageId**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the item's stage ||
|| **storageElementIds**
[`integer[]`](../../data-types.md#standart-types) | Read and write | Array of file identifiers ||
|| **storageTypeId**
[`integer`](../../data-types.md#standart-types) | Read and write | Identifier of the file storage type ||
|| **taxValue**
[`double`](../../data-types.md#standart-types) | Read and write | Tax amount ||
|| **terms**
[`text`](../../data-types.md#standart-types) | Read and write | Terms ||
|| **termsType**
[`integer`](../../data-types.md#standart-types) | Response only | Identifier of the terms format from the shared directory of text formats ||
|| **title**
[`string`](../../data-types.md#standart-types) | Read and write | Name of the item ||
|#

### Invoice {#invoice}

`entityTypeId = 31`

The `crm.item.*` methods work only with new-type invoices. For old-type invoices `entityTypeId = 5` they return the `ENTITY_TYPE_NOT_SUPPORTED` error. Details are in [{#T}](./invoice.md).

The invoice type is predefined, and its settings are not available over REST: `crm.type.get` with `entityTypeId = 31` returns error `100`. The invoice field set is fixed.

#|
|| **Name**
`type` | **Access** | **Description** ||
|| **accountNumber**
[`string`](../../data-types.md#standart-types) | Read and write | Invoice number ||
|| **begindate**
[`date`](../../data-types.md#standart-types) | Read and write | Start date of the item ||
|| **categoryId**
[`crm_category`](../data-types.md#data-types) | Read and write | Identifier of the item's funnel ||
|| **closedate**
[`date`](../../data-types.md#standart-types) | Read and write | Completion date of the item ||
|| **comments**
[`text`](../../data-types.md#standart-types) | Read and write | Comment ||
|| **companyId**
[`crm_company`](../data-types.md#data-types) | Read and write | Identifier of the company linked to the item ||
|| **contactId**
[`crm_contact`](../data-types.md#data-types) | Read and write | Identifier of the contact linked to the item. Deprecated, use `contactIds` ||
|| **contactIds**
[`crm_contact[]`](../data-types.md#data-types) | Read and write | List of identifiers of the contacts linked to the item ||
|| **currencyId**
[`crm_currency`](../data-types.md#data-types) | Read and write | Identifier of the item's currency ||
|| **isManualOpportunity**
[`boolean`](../../data-types.md#standart-types) | Read and write | Whether manual mode for calculating the amount is enabled ||
|| **isRecurring**
[`boolean`](../../data-types.md#standart-types) | Read and write | Whether the invoice is recurring ||
|| **locationId**
[`location`](../data-types.md#data-types) | Read and write | System field: identifier of the location ||
|| **movedBy**
[`user`](../../data-types.md#standart-objects) | Read-only | Identifier of the user who last changed the stage ||
|| **movedTime**
[`datetime`](../../data-types.md#standart-types) | Read-only | Date and time of the last stage change ||
|| **mycompanyId**
[`crm_company`](../data-types.md#data-types) | Read and write | Identifier of "my" company ||
|| **observers**
[`user[]`](../../data-types.md#standart-objects) | Read and write | List of identifiers of the users who are observers ||
|| **opportunity**
[`double`](../../data-types.md#standart-types) | Read and write | Amount ||
|| **previousStageId**
[`crm_status`](../data-types.md#data-types) | Read-only | String identifier of the previous stage ||
|| **sourceDescription**
[`text`](../../data-types.md#standart-types) | Read and write | Additional information about the source ||
|| **sourceId**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the source type ||
|| **stageId**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the item's stage ||
|| **taxValue**
[`double`](../../data-types.md#standart-types) | Read and write | Tax amount ||
|| **title**
[`string`](../../data-types.md#standart-types) | Read and write | Name of the item ||
|| **xmlId**
[`string`](../../data-types.md#standart-types) | Read and write | External code ||
|#

### Smart Process {#spa}

`entityTypeId` — from 128. Bitrix24 returns the identifiers of smart processes through the [crm.type.list](./user-defined-object-types/crm-type-list.md) method.

Some fields depend on the settings of the smart process type — in the table, the setting is named in the field description. The settings are returned by the [crm.type.get](./user-defined-object-types/crm-type-get.md) method, which requires administrative access to the smart process or permission to read it.

When a setting is disabled, the field does not disappear but changes its access: it drops out of the `crm.item.fields` response and behaves as Response only — it arrives in `crm.item.get` and `crm.item.list` but is ignored on write.

#|
|| **Name**
`type` | **Access** | **Description** ||
|| **accountCurrencyId**
[`crm_currency`](../data-types.md#data-types) | Not returned | System field: accounting currency ||
|| **begindate**
[`date`](../../data-types.md#standart-types) | Read and write | Start date of the item. Depends on the `isBeginCloseDatesEnabled` setting ||
|| **categoryId**
[`crm_category`](../data-types.md#data-types) | Read and write | Identifier of the item's funnel ||
|| **closedate**
[`date`](../../data-types.md#standart-types) | Read and write | Completion date of the item. Depends on the `isBeginCloseDatesEnabled` setting ||
|| **companyId**
[`crm_company`](../data-types.md#data-types) | Read and write | Identifier of the company linked to the item. Depends on the `isClientEnabled` setting ||
|| **contactId**
[`crm_contact`](../data-types.md#data-types) | Read and write | Identifier of the contact linked to the item. Deprecated, use `contactIds`. Depends on the `isClientEnabled` setting ||
|| **contactIds**
[`crm_contact[]`](../data-types.md#data-types) | Read and write | List of identifiers of the contacts linked to the item. Depends on the `isClientEnabled` setting ||
|| **currencyId**
[`crm_currency`](../data-types.md#data-types) | Read and write | Identifier of the item's currency. Depends on the `isLinkWithProductsEnabled` setting ||
|| **isManualOpportunity**
[`boolean`](../../data-types.md#standart-types) | Read and write | Whether manual mode for calculating the amount is enabled. Depends on the `isLinkWithProductsEnabled` setting ||
|| **isRecurring**
[`boolean`](../../data-types.md#standart-types) | Read and write | Whether the item is recurring ||
|| **movedBy**
[`user`](../../data-types.md#standart-objects) | Read-only | Identifier of the user who last changed the stage. Depends on the `isStagesEnabled` setting ||
|| **movedTime**
[`datetime`](../../data-types.md#standart-types) | Read-only | Date and time of the last stage change. Depends on the `isStagesEnabled` setting ||
|| **mycompanyId**
[`crm_company`](../data-types.md#data-types) | Read and write | Identifier of "my" company. Depends on the `isMycompanyEnabled` setting ||
|| **observers**
[`user[]`](../../data-types.md#standart-objects) | Read and write | List of identifiers of the users who are observers. Depends on the `isObserversEnabled` setting ||
|| **opportunity**
[`double`](../../data-types.md#standart-types) | Read and write | Amount. Depends on the `isLinkWithProductsEnabled` setting ||
|| **opportunityAccount**
[`double`](../../data-types.md#standart-types) | Not returned | System field: amount in the accounting currency ||
|| **previousStageId**
[`crm_status`](../data-types.md#data-types) | Read-only | String identifier of the previous stage. Depends on the `isStagesEnabled` setting ||
|| **sourceDescription**
[`text`](../../data-types.md#standart-types) | Read and write | Additional information about the source. Depends on the `isSourceEnabled` setting ||
|| **sourceId**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the source type. Depends on the `isSourceEnabled` setting ||
|| **stageId**
[`crm_status`](../data-types.md#data-types) | Read and write | String identifier of the item's stage. Depends on the `isStagesEnabled` setting ||
|| **taxValue**
[`double`](../../data-types.md#standart-types) | Read and write | Tax amount. Depends on the `isLinkWithProductsEnabled` setting ||
|| **taxValueAccount**
[`double`](../../data-types.md#standart-types) | Not returned | System field: tax amount in the accounting currency ||
|| **title**
[`string`](../../data-types.md#standart-types) | Read and write | Name of the item ||
|| **xmlId**
[`string`](../../data-types.md#standart-types) | Read and write | External code ||
|#

## Continue Learning

- create an item — [{#T}](./crm-item-add.md)
- modify an item — [{#T}](./crm-item-update.md)
- retrieve an item or a list of items — [{#T}](./crm-item-get.md), [{#T}](./crm-item-list.md)
- retrieve the field list of a specific object type — [{#T}](./crm-item-fields.md)
- work with custom fields — [{#T}](./user-defined-fields/index.md)
- learn the field length limits — [{#T}](../field-length-limits.md)
