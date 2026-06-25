# Create a New CRM object crm.item.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with the "add" access permission for the CRM object

This method provides a universal approach for creating CRM objects. It allows you to create various types of objects, such as deals, contacts, companies, and others.

To create an object, you must pass the appropriate parameters, including the object type and its information: title, description, contact details, and other specifics.

Upon successful execution of the request, a new object is created.

This method provides a flexible opportunity to automate the object creation process and integrate the CRM with other systems.

When creating an entity, a standard series of checks, modifications, and automatic actions are performed:

- access permissions are checked
- required fields are validated
- stage-dependent required fields are validated
- field values are checked for correctness
- default values are assigned to fields
- automation rules are triggered after saving

Next, we will take a closer look at how to use this method and which parameters must be passed.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type`          | **Description**                                                                                                                        ||
|| **entityTypeId***
[`integer`][1] | [System](../data-types.md#object_type) or [custom type](./user-defined-object-types/index.md) identifier of the item we want to create.

Numerical values for system types (Lead — 1, Deal — 2, Contact — 3, Company — 4, Invoice — 31, etc.) are provided in the [CRM object type directory](../data-types.md#object_type). The SPA identifier can be found using the [crm.type.list](./user-defined-object-types/crm-type-list.md) method. ||
|| **fields***
[`object`][1]  | Format object.

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where
- `field_n` — field name
- `value_n` — field value

Each CRM object type has its own set of fields. This means that the set of fields for creating a Lead does not necessarily match the set of fields for creating a Contact or SPA.

The list of available fields for each entity type is described [below](#parametr-fields).

An incorrect field in `fields` will be ignored.
||
|| **useOriginalUfNames**
[`boolean`][1] | Parameter to control the format of custom field names in the request and response.   
Possible values:

- `Y` — original names of custom fields, e.g., `UF_CRM_2_1639669411830`
- `N` — custom field names in camelCase, e.g., `ufCrm2_1639669411830`

Default is `N`. ||
|#

### Parameter fields

{% include [Note on parameters](../../../_includes/required.md) %}

{% list tabs %}

- Lead

  CRM object identifier **entityTypeId:** `1`

    #|
    || **Name**
    `type` | **Description** ||
    || **title**
    [`string`][1] | Item name.

    By default, it is generated according to the template `{entityTypeName} #{id}`, where
    - `entityTypeName` — entity name
    - `id` — item identifier
  
    For example, for a lead with `id = 13` — 'Lead #13' 
    ||
    || **honorific**
    [`crm_status`](../data-types.md) | String identifier of the lead's salutation (e.g., `'HNR_US_1' = 'Mr.'`).

    The list of available salutations can be found using [`crm.status.list`][2] by applying the filter `{ ENTITY_ID: "HONOFIRIC" }`.

    Default is `null` ||
    || **name**
    [`string`][1] | First name.

    Default is `null` ||
    || **secondName**
    [`string`][1] | Middle name.

    Default is `null` ||
    || **lastName**
    [`string`][1] | Last name.

    Default is `null` ||
    || **birthdate**
    [`date`][1] | Date of birth.

    Default is `null` ||
    || **companyTitle**
    [`string`][1] | Company name.

    Default is `null` ||
    || **sourceId**
    [`crm_status`](../data-types.md) | String identifier of the source.
  
    For example, `'CALL' = 'Call'`.
  
    The list of available sources can be found using [`crm.status.list`][2] by applying the filter `{ ENTITY_ID: "SOURCE" }`.

    By default, it is set to of the first available source ||
    || **sourceDescription**
    [`text`][1] | Additional information about the source.

    Default is `null` ||
    || **stageId**
    [`crm_status`](../data-types.md) | String identifier of the item stage.
  
    For example, `'NEW' = 'Unprocessed'`.

    The list of available stages can be found using [`crm.status.list`][2] by applying the filter `{ ENTITY_ID: "STATUS" }`

    By default, it is set to of the first available stage ||
    || **statusDescription**
    [`text`][1] | Additional information about the stage.

    Default is `null` ||
    || **post**
    [`string`][1] | Job title.

    Default is `null` ||
    || **currencyId**
    [`crm_currency`](../data-types.md) | Item currency identifier

    By default, it is set to of the default currency ||
    || **isManualOpportunity**
[`boolean`][1] | Amount calculation mode. Possible values:

- `Y` — manual
- `N` — automatic

Default — `N` ||
    || **opportunity**
[`double`][1] | Amount.

    Default is `null` ||
    || **opened**
[`boolean`][1] | Whether the item is available to everyone. Possible values:

- `Y` — yes
- `N` — no

Default — `Y`. The default value can be changed in the CRM settings ||
    || **comments**
[`text`][1] | Comment.

    Default is `null` ||
    || **assignedById**
[`user`][1] | Identifier of the person responsible for the item.

By default, this is the identifier of the user calling the method ||
    || **companyId**
[`crm_company`](../data-types.md) | Company identifier linked to the item.

The company list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

    Default is `null` ||
    || **contactId**
[`crm_contact`](../data-types.md) | Contact identifier linked to the item.

The contact list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

    Default is `null` ||
    || **contactIds**
[`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item.

The contact list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

    Default is `null` ||
    || **originatorId**
[`string`][1] | External source.

    Default is `null` ||
    || **originId**
[`string`][1] | Item identifier in the external source.

    Default is `null` ||
    || **webformId**
[`integer`][1] | CRM Form identifier.

    Default is `null` ||
    || **observers**
[`user[]`][1] | Array of user identifiers who will be Observers in the item.

    Default is `null` ||
    || **utmSource**
[`string`][1] | Ad system. For example: Search Ads, Display Ads, etc.

    Default is `null` ||
    || **utmMedium**
[`string`][1] | Traffic type. Possible values:
  
- CPC — ads
- CPM — banners

    Default is `null` ||
    || **utmCampaign**
[`string`][1] | Advertising campaign designation.

    Default is `null` ||
    || **utmContent**
[`string`][1] | Campaign contents. For example, for contextual ads.

    Default is `null` ||
    || **utmTerm**
[`string`][1] | Campaign search condition. For example, contextual advertising keywords.

Defaults to `null` ||
    || **ufCrm...**
[`crm_userfield`](../data-types.md) | User field.
  
Read the [{#T}](./user-defined-fields/index.md) section about user fields.
  
Values of multiple fields are passed as an array.
  
To upload a file, you must pass an array as the user field value, where the first item is the filename and the second is the file content encoded in [base64](../../files/how-to-upload-files.md).
    ||
    || **parentId...**
[`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

Each such field has the code `parentId + {parentEntityTypeId}`
    ||
    || **fm**
[`multifield[]`](../data-types.md) | Array of multifields.

You can read more about multifields in the [{#T}](../data-types.md#crm_multifield) section.

Multifield structure:

- `typeId` — Multifield type
- `valueType` — Value type
- `value` — Value

Example:

    ```bash
    fm: [
      {
        "valueType": "WORK",
        "value": "+79999999",
        "typeId": "PHONE"
      },
      {
        "valueType": "WORK",
        "value": "bitrix@bitrix.com",
        "typeId": "EMAIL"
      }
    ]
    ```
Default — `null`
    ||
    |#


- Deal

  CRM object identifier **entityTypeId:** `2`

    #|
    || **Name**
    `type` | **Description** ||
    || **title**
[`string`][1] | Item name

    By default, it is generated according to the template `{entityTypeName} #{id}`, where
    - `entityTypeName` — entity name
    - `id` — item identifier
For example, for a deal with `id = 13` => 'Deal #13' ||
    || **typeId**
[`crm_status`](../data-types.md) | String identifier of the entity type.

For example, for a deal: `'SALE' = 'Sale'`

You can find the list of available entity types using [`crm.status.list`][2] by applying the filter `{ ENTITY_ID: "DEAL_TYPE" }`

Default — the first available entity type ||
    || **categoryId**
[`integer`][1] | Identifier of the deal [direction](./category/index.md) (funnel).

Default — `0` (general) ||
    || **stageId**
[`crm_status`](../data-types.md) | String identifier of the item stage.
  
    For example, `'NEW' = 'Unprocessed'`.

A list of available stages can be obtained using [`crm.status.list`][2] by applying a filter:
- If the deal is in the general funnel (direction) — `{ ENTITY_ID: "DEAL_STAGE" }`
- If the deal is not in the general funnel (direction) — `{ ENTITY_ID: "DEAL_STAGE_{categoryId}" }`, where
`categoryId` is the identifier of the deal's funnel ([direction](./category/index.md))

Default — the first available stage relative to the funnel ||
    || **isRecurring**
[`boolean`][1] | Whether the deal is recurring. Possible values:

- `Y` — yes
- `N` — no

Default — `N`||
    || **probability**
[`integer`][1] | Probability %.

    Default is `null` ||
    || **currencyId**
[`crm_currency`](../data-types.md) | Identifier of the item currency.

Default — default currency ||
    || **isManualOpportunity**
[`boolean`][1] | Amount calculation mode. Possible values:

- `Y` — manual
- `N` — automatic

Default — `N` ||
    || **opportunity**
[`double`][1] | Amount.

    Default is `null` ||
    || **taxValue**
[`double`][1] | Tax amount.

    Default is `null` ||
    || **companyId**
[`crm_company`](../data-types.md) | Company identifier linked to the item.

The company list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

    Default is `null` ||
    || **contactId**
[`crm_contact`](../data-types.md) | Contact identifier linked to the item.

The contact list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

    Default is `null` ||
    || **contactIds**
[`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item.

The contact list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

    Default is `null` ||
    || **quoteId**
[`crm_quote`](../data-types.md) | Identifier of the quote that will be linked to the deal ||
    || **begindate**
[`date`][1] | Item start date.

Default — Create date ||
    || **closedate**
[`date`][1] | Item end date.

Default — Create date item + 7 days ||
    || **opened**
[`boolean`][1] | Whether the item is available to everyone. Possible values:

- `Y` — yes
- `N` — no

Default — `Y`. The default value can be changed in the CRM settings ||
    || **comments**
[`text`][1] | Comment.

    Default is `null` ||
    || **assignedById**
[`user`][1] | Identifier of the person responsible for the item.

Default — identifier of the user calling the method ||
    || **sourceId**
[`crm_status`](../data-types.md) | String identifier of the source.
  
    For example, `'CALL' = 'Call'`.
  
    The list of available sources can be found using [`crm.status.list`][2] by applying the filter `{ ENTITY_ID: "SOURCE" }`.

Default — First available source ||
    || **sourceDescription**
    [`text`][1] | Additional information about the source.

Default — `null`||
    || **leadId**
[`crm_lead`](../data-types.md) | Lead identifier on which the item is created.

Default — `null`||
    || **additionalInfo**
[`string`][1] | Additional information.

    Default is `null` ||
    || **originatorId**
[`string`][1] | External source.

Default — `null`||
    || **originId**
[`string`][1] | Item identifier in the external source.

Default — `null`||
    || **observers**
[`user[]`][1] | Array of user identifiers who will be Observers in the item.

    Default is `null` ||
    || **locationId**
[`location`][1] | Location identifier. Service field.

    Default is `null` ||
    || **utmSource**
[`string`][1] | Ad system. Search Ads, Display Ads, and others.

    Default is `null` ||
    || **utmMedium**
[`string`][1] | Traffic type. Possible values:
  
- CPC — ads
- CPM — banners

    Default is `null` ||
|| **utmCampaign** [`string`][1] | Advertising campaign designation.

    Default is `null` ||
    || **utmContent**
[`string`][1] | Campaign contents. For example, for contextual ads.

    Default is `null` ||
    || **utmTerm**
[`string`][1] | Campaign search condition. For example, contextual advertising keywords.

    Default is `null` ||
    || **ufCrm...**
[`crm_userfield`](../data-types.md) | Custom field. See section [{#T}](./user-defined-fields/index.md)

- Values of multiple fields are passed as an array
- To upload a file, you must pass an array as the custom field value, where the first item is the filename and the second is the [base64](../../files/how-to-upload-files.md) encoded file content.
  
    ||
    || **parentId...**
[`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

Each such field has the code `parentId + {parentEntityTypeId}` 
    ||
    |#


- Contact

  CRM object identifier **entityTypeId:** `3`

    #|
    || **Name**
    `type` | **Description** ||
    || **honorific**
[`crm_status`](../data-types.md) | String identifier of the contact's salutation. 
  
For example, `'HNR_US_1' = 'Mr.'`.

    The list of available salutations can be found using [`crm.status.list`][2] by applying the filter `{ ENTITY_ID: "HONOFIRIC" }`.

    Default is `null` ||
    || **name**
    [`string`][1] | First name.

    Default is `null` ||
    || **secondName**
    [`string`][1] | Middle name.

    Default is `null` ||
    || **lastName**
    [`string`][1] | Last name.

    Default is `null` ||
    || **photo**
[`file`][1] | Photograph.

    Default is `null` ||
    || **birthdate**
    [`date`][1] | Date of birth.

    Default is `null` ||
    || **typeId**
[`crm_status`](../data-types.md) | String identifier of the entity type.
  
For example, for a deal: `'SALE' = 'Sale'`.
  
The list of available entity types can be found using [`crm.status.list`][2] by applying the filter `{ ENTITY_ID: "CONTACT_TYPE" }`.

Default — first available entity type ||
    || **sourceId**
    [`crm_status`](../data-types.md) | String identifier of the source.
  
    For example, `'CALL' = 'Call'`.
  
    The list of available sources can be found using [`crm.status.list`][2] by applying the filter `{ ENTITY_ID: "SOURCE" }`.

Default — first available source ||
    || **sourceDescription**
    [`text`][1] | Additional information about the source.

    Default is `null` ||
    || **post**
    [`string`][1] | Job title.

    Default is `null` ||
    || **comments**
[`text`][1] | Comment.

    Default is `null` ||
    || **opened**
[`boolean`][1] | Whether the item is available to everyone. Possible values:

- `Y` — yes
- `N` — no

Default — `Y`. The default value can be changed in the crm settings ||
    || **export**
[`boolean`][1] | Whether the contact is included in the export.

Default — `Y` ||
    || **assignedById**
[`user`][1] | Identifier of the person responsible for the item.

Default — identifier of the user calling the method ||
    || **companyId**
[`crm_company`](../data-types.md) | Company identifier linked to the item.

The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

    Default is `null` ||
    || **companyIds**
[`crm_company`](../data-types.md) | An array of company IDs that will be linked to the item ||
    || **leadId**
[`crm_lead`](../data-types.md) | Lead identifier on which the item is created.

    Default is `null` ||
    || **originatorId**
[`string`][1] | External source.

    Default is `null` ||
    || **originId**
[`string`][1] | Item identifier in the external source.

    Default is `null` ||
    || **originVersion**
[`string`][1] | Original version.

    Default is `null` ||
    || **observers**
[`user[]`][1] | Array of user identifiers who will be Observers in the item.

    Default is `null` ||
    || **utmSource**
[`string`][1] | Ad system. Search Ads, Display Ads, and others.

    Default is `null` ||
    || **utmMedium**
[`string`][1] | Traffic type. Possible values:
  
- CPC — ads
- CPM — banners

    Default is `null` ||
    || **utmCampaign**
[`string`][1] | Advertising campaign designation.

    Default is `null` ||
    || **utmContent**
[`string`][1] | Campaign contents. For example, for contextual ads.

    Default is `null` ||
    || **utmTerm**
[`string`][1] | Campaign search condition. For example, contextual advertising keywords.

    Default is `null` ||
    || **ufCrm...**
[`crm_userfield`](../data-types.md) | Custom field. See section [{#T}](./user-defined-fields/index.md)

- Values of multiple fields are passed as an array
- To upload a file, you must pass an array as the custom field value, where the first item is the filename and the second is the [base64](../../files/how-to-upload-files.md) encoded file content.

    ||
    || **parentId...**
[`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

Each such field has the code `parentId + {parentEntityTypeId}`
    ||
    || **fm**
[`multifield[]`](../data-types.md) | Array of multifields.

You can read more about multifields in the [{#T}](../data-types.md#crm_multifield) section.

Multifield structure:

- `typeId` — Multifield type
- `valueType` — Value type
- `value` — Value

Example:

    ```bash
    fm: [
      {
        "valueType": "WORK",
        "value": "+79999999",
        "typeId": "PHONE"
      },
      {
        "valueType": "WORK",
        "value": "bitrix@bitrix.com",
        "typeId": "EMAIL"
      }
    ]
    ```
Default — `null`||
    |#


- Company

  CRM object identifier **entityTypeId:** `4`

    #|
    || **Name**
    `type` | **Description** ||
    || **title**
    [`string`][1] | Item name.

    By default, it is generated according to the template `{entityTypeName} #{id}`, where
  
    - `entityTypeName` — entity name
    - `id` — item identifier
  
For example, for a company with `id = 13` => 'Company #13' ||
    || **typeId**
[`crm_status`](../data-types.md) | String identifier of the entity type.
  
For example, for a deal: `'SALE' = 'Sale'`.
  
The list of available entity types can be found using [`crm.status.list`][2] by applying the filter `{ ENTITY_ID: "COMPANY_TYPE" }`.

Default — the first available entity type ||
    || **logo**
[`file`][1] | Logo.

    Default is `null` ||
    || **bankingDetails**
[`string`][1] | Bank Company details.

    Default is `null` ||
    || **industry**
[`crm_status`](../data-types.md) | String identifier of the industry type.
  
For example `'IT' = 'Information Technology'`.
  
The list of available industry types can be found using the [`crm.status.list`][2] method by applying the filter `{ ENTITY_ID: "INDUSTRY"}`.

Default — the first available industry type ||
    || **employees**
[`crm_status`](../data-types.md) | String identifier of the employee count type.
  
The value is taken from the list of available ones, for example `'EMPLOYEES_1' = 'less than 50'`.

The list of available employee count types can be found using the [`crm.status.list`][2] method by applying the filter `{ ENTITY_ID: "EMPLOYEES" }`.

Default — the first available employee count type ||
    || **currencyId**
[`crm_currency`](../data-types.md) | Identifier of the item currency.

Default — default currency ||
    || **revenue**
[`double`][1] | Annual turnover.

Default — `0` ||
    || **opened**
[`boolean`][1] | Whether the item is available to everyone. Possible values:

- `Y` — yes
- `N` — no

Default — `Y`. The default value can be changed in the CRM settings ||
    || **comments**
[`text`][1] | Comment.

    Default is `null` ||
    || **isMyCompany**
[`boolean`][1] | Whether the company is my company.

Default — `N` ||
    || **assignedById**
[`user`][1] | Identifier of the person responsible for the item.

Default — identifier of the user calling the method ||
    || **contactIds**
[`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item.

The contact list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

Default — `null`||
    || **leadId**
[`crm_lead`](../data-types.md) | Lead identifier on which the item is created.

Default — `null`||
    || **originatorId**
[`string`][1] | External source.

    Default is `null` ||
    || **originId**
[`string`][1] | Item identifier in the external source.

    Default is `null` ||
    || **originVersion**
[`string`][1] | Original version.

    Default is `null` ||
    || **observers**
[`user[]`][1] | Array of user identifiers who will be Observers in the item.

    Default is `null` ||
    || **utmSource**
[`string`][1] | Ad system. Search Ads, Display Ads, and others.

    Default is `null` ||
    || **utmMedium**
[`string`][1] | Traffic type. Possible values:
- CPC — ads
- CPM — banners

    Default is `null` ||
    || **utmCampaign**
[`string`][1] | Advertising campaign designation.

    Default is `null` ||
    || **utmContent**
[`string`][1] | Campaign contents. For example, for contextual ads.

    Default is `null` ||
    || **utmTerm**
[`string`][1] | Campaign search condition. For example, contextual advertising keywords.

    Default is `null` ||
    || **ufCrm...**
[`crm_userfield`](../data-types.md) | Custom field. See section [{#T}](./user-defined-fields/index.md)

- Values of multiple fields are passed as an array
    - To upload a file, you must pass an array as the custom field value, where the first item is the filename and the second is the [base64](../../files/how-to-upload-files.md) encoded file content

    ||
    || **parentId...**
[`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

Each such field has the code `parentId + {parentEntityTypeId}`
    ||
    || **fm**
[`multifield[]`](../data-types.md) | Array of multifields.

You can read more about multifields in the [{#T}](../data-types.md#crm_multifield) section.

Multifield structure:

- `typeId` — Multifield type
- `valueType` — Value type
- `value` — Value

Example:

    ```bash
    fm: [
      {
        "valueType": "WORK",
        "value": "+79999999",
        "typeId": "PHONE"
      },
      {
        "valueType": "WORK",
        "value": "bitrix@bitrix.com",
        "typeId": "EMAIL"
      }
    ]

    ```
Default — `null`||
    |#


- Estimate

  CRM object identifier **entityTypeId:** `7`

    #|
    || **Name**
    `type` | **Description** ||
    || **title**
    [`string`][1] | Item name.

    By default, it is generated according to the template `{entityTypeName} #{id}`, where
    - `entityTypeName` — entity name
    - `id` — item identifier
  
    For example, for an offer with `id = 13` => 'Offer #13' ||
    || **assignedById**
[`user`][1] | Identifier of the person responsible for the item.

Default — identifier of the user calling the method ||
    || **opened**
[`boolean`][1] | Whether the item is available to everyone. Possible values:

- `Y` — yes
- `N` — no

    Default — `Y`. The default value can be changed in the CRM settings ||
    || **content**
    [`text`][1] | Content.

    Default is `null` ||
    || **terms**
    [`text`][1] | Conditions.

    Default is `null` ||
    || **comments**
[`text`][1] | Comment.

    Default is `null` ||
    || **dealId**
    [`crm_deal`](../data-types.md)        | Linked deal identifier.

    Default is `null` ||
    || **leadId**
[`crm_lead`](../data-types.md) | Lead identifier on which the item is created.

    Default is `null` ||
    || **storageTypeId**
    [`integer`][1] | Storage type identifier. Possible values:
    - `1` — file
    - `2` — WebDAV
    - `3` — disk

    Default:
    1. If the `disk` module is installed -> Disk
    2. If the `webdav` module is installed -> WebDAV
    3. File 
    ||
    || **storageElementIds**
    [`integer`][1] | Array of files.

    Default is `null` ||
    || **webformId**
[`integer`][1] | CRM Form identifier.

    Default is `null` ||
    || **companyId**
[`crm_company`](../data-types.md) | Company identifier linked to the item.

The company list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

    Default is `null` ||
    || **contactId**
[`crm_contact`](../data-types.md) | Contact identifier linked to the item.

    The contact list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`

    Default is `null` ||
    || **contactIds**
[`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item.

The contact list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

    Default is `null` ||
    || **locationId**
[`location`][1] | Location identifier. Service field.

    Default is `null` ||
    || **currencyId**
[`crm_currency`](../data-types.md) | Identifier of the item currency.

Default — default currency ||
    || **isManualOpportunity**
    [`boolean`][1] | Amount calculation mode.

- `Y` — manual
- `N` — automatic

Default — `N` ||
    || **opportunity**
[`double`][1] | Amount.

    Default is `null` ||
    || **taxValue**
[`double`][1] | Tax amount.

    Default is `null` ||
    || **stageId**
[`crm_status`](../data-types.md) | String identifier of the item stage.
  
    For example `'DRAFT' = 'New'`.

    The list of available stages can be found using [`crm.status.list`][2] by applying the filter `{ ENTITY_ID: "QUOTE_STATUS" }`.

    Default — the first available stage ||
    || **begindate**
[`date`][1] | Item start date.

    Default — Create date item ||
    || **closedate**
[`date`][1] | Item end date.

Default — Create date item + 7 days ||
    || **actualDate**
    [`date`][1] | Valid until.

Default — Create date item + 7 days ||
    || **mycompanyId**
    [`crm_company`](../data-types.md) | My company identifier.

    Default — the identifier of the first available "my" company ||
    || **utmSource**
[`string`][1] | Ad system. Search Ads, Display Ads, and others.

    Default is `null` ||
    || **utmMedium**
    [`string`][1] | Traffic type.
  
- CPC — ads
- CPM — banners

    Default is `null` ||
    || **utmCampaign**
[`string`][1] | Advertising campaign designation.

    Default is `null` ||
    || **utmContent**
[`string`][1] | Campaign contents. For example, for contextual ads.

    Default is `null` ||
    || **utmTerm**
[`string`][1] | Campaign search condition. For example, contextual advertising keywords.

    Default is `null` ||
    || **ufCrm...**
    [`crm_userfield`](../data-types.md) | Custom field. See the [{#T}](./user-defined-fields/index.md) section.

- Values of multiple fields are passed as an array
- To upload a file, you must pass an array as the custom field value, where the first item is the filename and the second is the [base64](../../files/how-to-upload-files.md) encoded file content.

    ||
    || **parentId...**
[`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

Each such field has the code `parentId + {parentEntityTypeId}`
    ||
    |#


- Invoice

  CRM object identifier **entityTypeId:** `31`

    #|
    || **Name**
    `type` | **Description** ||
    || **title**
    [`string`][1] | Item name.

    By default, it is generated according to the template `{entityTypeName} #{id}`, where
  
    - `entityTypeName` — entity name
    - `id` — item identifier
  
For example, for an account with `id = 13` => 'Account #13'
    ||
    || **xmlId**
[`string`][1] | External code.

    Default is `null` ||
    || **assignedById**
[`user`][1] | Identifier of the person responsible for the item.

Default — identifier of the user calling the method ||
    || **opened**
[`boolean`][1] | Whether the item is available to everyone. Possible values:

- `Y` — yes
- `N` — no

    Default — `Y`. The default value can be changed in the CRM settings ||
    || **webformId**
[`integer`][1] | CRM Form identifier.

    Default is `null` ||
    || **begindate**
[`date`][1] | Item start date.

    Default — Create date item ||
    || **closedate**
[`date`][1] | Item end date.

Default — Create date item + 7 days ||
    || **companyId**
[`crm_company`](../data-types.md) | Company identifier linked to the item.

The company list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

    Default is `null` ||
    || **contactId**
[`crm_contact`](../data-types.md) | Contact identifier linked to the item.

The contact list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

    Default is `null` ||
    || **contactIds**
[`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item.

The contact list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

    Default is `null` ||
    || **observers**
[`user[]`][1] | Array of user identifiers who will be Observers in the item.

    Default is `null` ||
    || **stageId**
[`crm_status`](../data-types.md) | String identifier of the item stage.
  
For example `'DT31_13:N' = 'New'`.

The list of available stages can be found using [`crm.status.list`][2] by applying the filter: `{ ENTITY_ID: "SMART_INVOICE_STAGE_{categoryId}" }`, where
`categoryId` is the default invoice pipeline identifier. It can be found using [`crm.category.list`](category/crm-category-list.md) by `entityTypeId = 31`.

    Default — the first available stage ||
    || **sourceId**
    [`crm_status`](../data-types.md) | String identifier of the source.
  
    For example, `'CALL' = 'Call'`.
  
    The list of available sources can be found using [`crm.status.list`][2] by applying the filter `{ ENTITY_ID: "SOURCE" }`.

Default — the first available source ||
    || **sourceDescription**
    [`text`][1] | Additional information about the source.

    Default is `null` ||
    || **currencyId**
[`crm_currency`](../data-types.md) | Identifier of the item currency.

Default — default currency ||
    || **isManualOpportunity**
[`boolean`][1] | Amount calculation mode. Possible values:

- `Y` — manual
- `N` — automatic

Default — `N` ||
    || **opportunity**
[`double`][1] | Amount.

    Default is `null` ||
    || **taxValue**
[`double`][1] | Tax amount.

    Default is `null` ||
    || **mycompanyId**
    [`crm_company`](../data-types.md) | My company identifier.

    Default — the identifier of the first available "my" company ||
    || **comments**
[`text`][1] | Comment.

    Default is `null` ||
    || **locationId**
[`location`][1] | Location identifier. Service field.

    Default is `null` ||
    || **ufCrm...**
    [`crm_userfield`](../data-types.md) | Custom field. See the [{#T}](./user-defined-fields/index.md) section.

- Values of multiple fields are passed as an array
- To upload a file, you must pass an array as the custom field value, where the first item is the filename and the second is the [base64](../../files/how-to-upload-files.md) encoded file content.

    ||
    || **parentId...**
[`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

Each such field has the code `parentId + {parentEntityTypeId}`
    ||
    |#


- SPA

  CRM object identifier **entityTypeId:** can be obtained using the [`crm.type.list`](user-defined-object-types/crm-type-list.md) method or created using the [`crm.type.add`](user-defined-object-types/crm-type-add.md) method.

    #|
    || **Name**
    `type` | **Description** ||
    || **title**
    [`string`][1] | Item name.

    By default, it is generated according to the template `{entityTypeName} #{id}`, where
- `entityTypeName` — the SPA name
    - `id` — item identifier
  
For example, for SPA item "HR" with `id = 13` => 'HR #13'  ||
    || **xmlId**
[`string`][1] | External code.

    Default is `null` ||
    || **assignedById**
[`user`][1] | Identifier of the person responsible for the item.

Default — the user ID who calls the method  ||
    || **opened**
[`boolean`][1] | Whether the item is available to everyone.

- `Y` — yes
- `N` — no

Default — `Y`. The default value can be changed in the CRM settings ||
    || **webformId**
[`integer`][1] | CRM Form identifier.

    Default is `null` ||
    || **begindate**
[`date`][1] | Item start date.

Only available if the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA.

Default — Create date item  ||
    || **closedate**
[`date`][1] | Item end date.

Only available if the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA.

Default — Create date item + 7 days  ||
    || **companyId**
[`crm_company`](../data-types.md) | Company identifier linked to the item.

The company list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

Only available if the `isClientEnabled` setting is enabled for the corresponding SPA.

    Default is `null` ||
    || **contactId**
[`crm_contact`](../data-types.md) | Contact identifier linked to the item.

The contact list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

Only available if the `isClientEnabled` setting is enabled for the corresponding SPA.

    Default is `null` ||
    || **contactIds**
[`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item.

The contact list can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

Only available if the `isClientEnabled` setting is enabled for the corresponding SPA.

    Default is `null` ||
    || **observers**
[`user[]`][1] | Array of user identifiers who will be Observers in the item.

Only available if the `isObserversEnabled` setting is enabled for the corresponding SPA.

    Default is `null` ||
    || **categoryId**
[`crm_category`](../data-types.md) | SPA item pipeline identifier.

The list of available pipelines can be found using [`crm.category.list`](category/crm-category-list.md) by applying the corresponding `entityTypeId` ||
    || **stageId**
[`crm_status`](../data-types.md) | String identifier of the item stage.
  
For example `'DT1220_30:NEW' = 'Start'`.

The list of available stages can be found using [`crm.status.list`][2] by applying the filter `{ ENTITY_ID: "DYNAMIC_{entityTypeId}_STAGE_{categoryId}" }`, where
    - `entityTypeId` — SPA type identifier
    - `categoryId` — SPA item funnel (direction) identifier

    [More details about funnels (directions)](category/index.md).

    Only available if the `isStagesEnabled` setting is enabled for the corresponding SPA.

    Default — the first available stage relative to the funnel  ||
    || **sourceId**
    [`crm_status`](../data-types.md) | String source identifier. (e.g., `'CALL' = 'Call'`).
  
    The list of available sources can be found using [`crm.status.list`][2] by applying the filter `{ ENTITY_ID: "SOURCE" }`.

    Only available if the `isSourceEnabled` setting is enabled for the corresponding SPA.

Default — first available source ||
    || **sourceDescription**
    [`text`][1] | Additional information about the source.

    Only available if the `isSourceEnabled` setting is enabled for the corresponding SPA.

    Default is `null` ||
    || **currencyId**
[`crm_currency`](../data-types.md) | Identifier of the item currency.

    Only available if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

    Default — default currency  ||
    || **isManualOpportunity**
[`boolean`][1] | Amount calculation mode. Possible values:

- `Y` — manual
- `N` — automatic

    Only available if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

Default — `N` ||
    || **opportunity**
[`double`][1] | Amount.

    Only available if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

    Default is `null` ||
    || **taxValue**
[`double`][1] | Tax amount.

    Only available if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

    Default is `null` ||
    || **mycompanyId**
    [`crm_company`](../data-types.md) | My company identifier.

    Only available if the `isMycompanyEnabled` setting is enabled for the corresponding SPA.

    Default — Identifier of the first available "my" company ||
    || **ufCrm...**
    [`crm_userfield`](../data-types.md) | Custom field. See the [{#T}](./user-defined-fields/index.md) section.

- Values of multiple fields are passed as an array
- To upload a file, you must pass an array as the custom field value, where the first item is the filename and the second is the [base64](../../files/how-to-upload-files.md) encoded file content.

    ||
    || **parentId...**
[`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

Each such field has the code `parentId + {parentEntityTypeId}`
    ||
    |#

  {% note info "SPA settings" %}

  For more information on managing SPA settings, you can read in [{#T}](./user-defined-object-types/index.md)

  {% endnote %}

{% endlist %}


## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

1. Example of creating a deal

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"fields":{"title":"New deal (specifically for REST method examples)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be an additional description about the source here","leadId":102,"additionalInfo":"There should be additional information here","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":2}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.add
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"fields":{"title":"New deal (specifically for REST method examples)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be an additional description about the source here","leadId":102,"additionalInfo":"There should be additional information here","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":2},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.add
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        type CrmItem = {
          id: number
          title: string
        }

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type ItemAddResult = {
          item: CrmItem
        }

        const formatDate = (date: Date): string => date.toISOString().slice(0, 10)
        const day = 60 * 60 * 24 * 1000
        const now = new Date()
        const twelveDaysInAdvance = new Date(now.getTime() + 12 * day)
        const monthAgo = new Date(now.getTime() - 30 * day)

        const commentsExample = [
          'Example comment inside a deal',
          '',
          '[B]Bold text[/B]',
          '[I]Italic[/I]',
          '[U]Underlined[/U]',
          '[S]Strikethrough[/S]',
          '[B][I][U][S]Mix[/S][/U][/I][/B]',
          '',
          '[LIST]',
          '[*]List item #1',
          '[*]List item #2',
          '[*]List item #3',
          '[/LIST]',
        ].join('\n')

        try {
          const response = await $b24.actions.v2.call.make<ItemAddResult>({
            method: 'crm.item.add',
            params: {
              entityTypeId: 2,
              fields: {
                title: 'New deal (specifically for the REST methods example)',
                typeId: 'SERVICE',
                categoryId: 9,
                stageId: 'C9:UC_KN8KFI',
                isReccurring: 'Y',
                probability: 50,
                currencyId: 'USD',
                isManualOpportunity: 'Y',
                opportunity: 999.99,
                taxValue: 99.9,
                companyId: 5,
                contactId: 4,
                contactIds: [4, 5],
                quoteId: 7,
                begindate: formatDate(monthAgo),
                closedate: formatDate(twelveDaysInAdvance),
                opened: 'N',
                comments: commentsExample,
                assignedById: 6,
                sourceId: 'WEB',
                sourceDescription: 'Additional description about the source goes here',
                leadId: 102,
                additionalInfo: 'Additional information goes here',
                observers: [2, 3],
                utmSource: 'google',
                utmMedium: 'CPC',
                ufCrm_1721244707107: 1111.1,
                parentId1220: 2,
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info(`Created item #${result.item.id} (${result.item.title})`)
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
        ```

    - JS (UMD)

        ```html
        <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
        <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
        <script>
          async function addCrmItem() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const formatDate = (date) => date.toISOString().slice(0, 10)
              const day = 60 * 60 * 24 * 1000
              const now = new Date()
              const twelveDaysInAdvance = new Date(now.getTime() + 12 * day)
              const monthAgo = new Date(now.getTime() - 30 * day)

              const commentsExample = [
                'Example comment inside a deal',
                '',
                '[B]Bold text[/B]',
                '[I]Italic[/I]',
                '[U]Underlined[/U]',
                '[S]Strikethrough[/S]',
                '[B][I][U][S]Mix[/S][/U][/I][/B]',
                '',
                '[LIST]',
                '[*]List item #1',
                '[*]List item #2',
                '[*]List item #3',
                '[/LIST]',
              ].join('\n')

              const response = await $b24.actions.v2.call.make({
                method: 'crm.item.add',
                params: {
                  entityTypeId: 2,
                  fields: {
                    title: 'New deal (specifically for the REST methods example)',
                    typeId: 'SERVICE',
                    categoryId: 9,
                    stageId: 'C9:UC_KN8KFI',
                    isReccurring: 'Y',
                    probability: 50,
                    currencyId: 'USD',
                    isManualOpportunity: 'Y',
                    opportunity: 999.99,
                    taxValue: 99.9,
                    companyId: 5,
                    contactId: 4,
                    contactIds: [4, 5],
                    quoteId: 7,
                    begindate: formatDate(monthAgo),
                    closedate: formatDate(twelveDaysInAdvance),
                    opened: 'N',
                    comments: commentsExample,
                    assignedById: 6,
                    sourceId: 'WEB',
                    sourceDescription: 'Additional description about the source goes here',
                    leadId: 102,
                    additionalInfo: 'Additional information goes here',
                    observers: [2, 3],
                    utmSource: 'google',
                    utmMedium: 'CPC',
                    ufCrm_1721244707107: 1111.1,
                    parentId1220: 2,
                  },
                },
                requestId: B24Js.Text.getUuidRfc4122()
              })

              // The payload is available only on a successful response
              if (!response.isSuccess) {
                console.error(response.getErrorMessages().join('; '))
                return
              }

              const result = response.getData().result
              console.info(`Created item #${result.item.id} (${result.item.title})`)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', addCrmItem)
        </script>
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.add',
            [
                'entityTypeId' => 2,
                'fields' => [
                    'title' => "New deal (specifically for REST method examples)",
                    'typeId' => "SERVICE",
                    'categoryId' => 9,
                    'stageId' => "C9:UC_KN8KFI",
                    'isReccurring' => "Y",
                    'probability' => 50,
                    'currencyId' => "USD",
                    'isManualOpportunity' => "Y",
                    'opportunity' => 999.99,
                    'taxValue' => 99.9,
                    'companyId' => 5,
                    'contactId' => 4,
                    'contactIds' => [4, 5],
                    'quoteId' => 7,
                    'begindate' => formatDate(monthAgo),
                    'closedate' => formatDate(twelveDaysInAdvance),
                    'opened' => "N",
                    'comments' => $commentsExample,
                    'assignedById' => 6,
                    'sourceId' => "WEB",
                    'sourceDescription' => "There should be an additional description about the source here",
                    'leadId' => 102,
                    'additionalInfo' => "There should be additional information here",
                    'observers' => [2, 3],
                    'utmSource' => "google",
                    'utmMedium' => "CPC",
                    'ufCrm_1721244707107' => 1111.1,
                    'parentId1220' => 2,
                ],
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

   - PHP (B24PhpSdk)

        ```php
       try {
           $entityTypeId = 1; // Example entity type ID
           $fields = [
               'title' => 'New Item',
               'createdTime' => (new DateTime())->format(DateTime::ATOM),
               'updatedTime' => (new DateTime())->format(DateTime::ATOM),
               'begindate' => (new DateTime())->format(DateTime::ATOM),
               'closedate' => (new DateTime())->format(DateTime::ATOM),
               // Add other necessary fields as required
           ];

           $result = $serviceBuilder
               ->getCRMScope()
               ->item()
               ->add($entityTypeId, $fields);

           print("ID: " . $result->item()->id . PHP_EOL);
           print("Title: " . $result->item()->title . PHP_EOL);
           print("Created By: " . $result->item()->createdBy . PHP_EOL);
           print("Updated By: " . $result->item()->updatedBy . PHP_EOL);
           print("Created Time: " . $result->item()->createdTime->format(DateTime::ATOM) . PHP_EOL);
           print("Updated Time: " . $result->item()->updatedTime->format(DateTime::ATOM) . PHP_EOL);
       } catch (Throwable $e) {
           print("Error: " . $e->getMessage() . PHP_EOL);
       }
        ```

    - Python

        Example

        ```python
        from b24pysdk.client import BaseClient
        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        client: BaseClient

        try:
            bitrix_response = client.crm.item.add(
                entity_type_id=2,
                fields={
                    "title": "New deal (specifically for REST method examples)",
                    "typeId": "SERVICE",
                    "categoryId": 9,
                    "stageId": "C9:UC_KN8KFI",
                    "isReccurring": "Y",
                    "probability": 50,
                    "currencyId": "USD",
                    "isManualOpportunity": "Y",
                    "opportunity": 999.99,
                    "taxValue": 99.9,
                    "companyId": 5,
                    "contactId": 4,
                    "contactIds": [4, 5],
                    "quoteId": 7,
                    "begindate": "formatDate(monthAgo)",
                    "closedate": "formatDate(twelveDaysInAdvance)",
                    "opened": "N",
                    "comments": "commentsExample",
                    "assignedById": 6,
                    "sourceId": "WEB",
                    "sourceDescription": "There should be an additional description about the source here",
                    "leadId": 102,
                    "additionalInfo": "There should be additional information here",
                    "observers": [2, 3],
                    "utmSource": "google",
                    "utmMedium": "CPC",
                    "ufCrm_1721244707107": 1111.1,
                    "parentId1220": 2,
                },
            ).response
            result = bitrix_response.result
            print(result)
        except BitrixAPIError as error:
            print(
                "Bitrix API error",
                f"error: {error.error}",
                f"error_description: {error.error_description}",
                sep="\n",
            )
        except BitrixSDKException as error:
            print(f"Bitrix SDK error: {error.message}")
        except Exception as error:
            print(f"Unexpected error: {error}")
        ```

    {% endlist %}


2. Example of creating an SPA item with a set of custom fields

    {% cut "Custom fields involved in the example" %}

    ```json
    {
    "ufCrm44_1721812760630": {
        "type": "string",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": true,
        "title": "Custom field (string)",
        "listLabel": "Custom field (string)",
        "formLabel": "Custom field (string)",
        "filterLabel": "Custom field (string)",
        "settings": {
        "SIZE": 20,
        "ROWS": 1,
        "REGEXP": "",
        "MIN_LENGTH": 0,
        "MAX_LENGTH": 0,
        "DEFAULT_VALUE": ""
        },
        "upperName": "UF_CRM_44_1721812760630"
    },
    "ufCrm44_1721812814433": {
        "type": "enumeration",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": true,
        "items": [
        {
            "ID": "79",
            "VALUE": "List item #1"
        },
        {
            "ID": "80",
            "VALUE": "List item #2"
        },
        {
            "ID": "81",
            "VALUE": "List item #3"
        },
        {
            "ID": "82",
            "VALUE": "List item #4"
        }
        ],
        "title": "Custom field (list)",
        "listLabel": "Custom field (list)",
        "formLabel": "Custom field (list)",
        "filterLabel": "Custom field (list)",
        "settings": {
        "DISPLAY": "LIST",
        "LIST_HEIGHT": 1,
        "CAPTION_NO_VALUE": "",
        "SHOW_NO_VALUE": "Y"
        },
        "upperName": "UF_CRM_44_1721812814433"
    },
    "ufCrm44_1721812853419": {
        "type": "date",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": true,
        "title": "Custom field (date)",
        "listLabel": "Custom field (date)",
        "formLabel": "Custom field (date)",
        "filterLabel": "Custom field (date)",
        "settings": {
        "DEFAULT_VALUE": {
            "TYPE": "NONE",
            "VALUE": ""
        }
        },
        "upperName": "UF_CRM_44_1721812853419"
    },
    "ufCrm44_1721812885588": {
        "type": "url",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": true,
        "isDynamic": true,
        "title": "Multiple custom field (link)",
        "listLabel": "Multiple custom field (link)",
        "formLabel": "Multiple custom field (link)",
        "filterLabel": "Multiple custom field (link)",
        "settings": {
        "POPUP": "Y",
        "SIZE": 20,
        "MIN_LENGTH": 0,
        "MAX_LENGTH": 0,
        "DEFAULT_VALUE": "",
        "ROWS": 1
        },
        "upperName": "UF_CRM_44_1721812885588"
    },
    "ufCrm44_1721812898903": {
        "type": "file",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": true,
        "title": "Custom field (file)",
        "listLabel": "Custom field (file)",
        "formLabel": "Custom field (file)",
        "filterLabel": "Custom field (file)",
        "settings": {
        "SIZE": 20,
        "LIST_WIDTH": 0,
        "LIST_HEIGHT": 0,
        "MAX_SHOW_SIZE": 0,
        "MAX_ALLOWED_SIZE": 0,
        "EXTENSIONS": [],
        "TARGET_BLANK": "Y"
        },
        "upperName": "UF_CRM_44_1721812898903"
    },
    "ufCrm44_1721812915476": {
        "type": "money",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": true,
        "title": "Custom field (money)",
        "listLabel": "Custom field (money)",
        "formLabel": "Custom field (money)",
        "filterLabel": "Custom field (money)",
        "settings": {
        "DEFAULT_VALUE": ""
        },
        "upperName": "UF_CRM_44_1721812915476"
    },
    "ufCrm44_1721812935209": {
        "type": "boolean",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": true,
        "title": "Custom field (Yes/No)",
        "listLabel": "Custom field (Yes/No)",
        "formLabel": "Custom field (Yes/No)",
        "filterLabel": "Custom field (Yes/No)",
        "settings": {
        "DEFAULT_VALUE": 0,
        "DISPLAY": "CHECKBOX",
        "LABEL": [
            "",
            ""
        ],
        "LABEL_CHECKBOX": {
            "en": "Custom field (Yes/No)",
            "de": "Custom field (Yes/No)",
            "th": "Custom field (Yes/No)",
            "la": "Custom field (Yes/No)",
            "tc": "Custom field (Yes/No)",
            "sc": "Custom field (Yes/No)",
            "br": "Custom field (Yes/No)",
            "ar": "Custom field (Yes/No)",
            "fr": "Custom field (Yes/No)",
            "vn": "Custom field (Yes/No)",
            "pl": "Custom field (Yes/No)",
            "tr": "Custom field (Yes/No)",
            "ja": "Custom field (Yes/No)",
            "it": "Custom field (Yes/No)",
            "ms": "Custom field (Yes/No)",
            "id": "Custom field (Yes/No)"
        }
        },
        "upperName": "UF_CRM_44_1721812935209"
    },
    "ufCrm44_1721812948498": {
        "type": "double",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": true,
        "title": "Custom field (number)",
        "listLabel": "Custom field (number)",
        "formLabel": "Custom field (number)",
        "filterLabel": "Custom field (number)",
        "settings": {
        "PRECISION": 2,
        "SIZE": 20,
        "MIN_VALUE": 0,
        "MAX_VALUE": 0,
        "DEFAULT_VALUE": null
        },
        "upperName": "UF_CRM_44_1721812948498"
    }
    }
    ```

    {% endcut %}

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{
            "entityTypeId": 1302,
            "fields": {
                "ufCrm44_1721812760630": "String for a String type custom field",
                "ufCrm44_1721812814433": 81,
                "ufCrm44_1721812853419": "'"$(date '+%Y-%m-%d')"'",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "300|USD",
                "ufCrm44_1721812935209": "Y",
                "ufCrm44_1721812948498": 9999.9
            }
        }' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.add
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{
            "entityTypeId": 1302,
            "fields": {
                "ufCrm44_1721812760630": "String for a String type custom field",
                "ufCrm44_1721812814433": 81,
                "ufCrm44_1721812853419": "'"$(date '+%Y-%m-%d')"'",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "300|USD",
                "ufCrm44_1721812935209": "Y",
                "ufCrm44_1721812948498": 9999.9
            },
            "auth": "**put_access_token_here**"
        }' \
        https://**put_your_bitrix24_address**/rest/crm.item.add
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        type CrmItem = {
          id: number
          title: string
        }

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type ItemAddResult = {
          item: CrmItem
        }

        const greenPixelInBase64 = 'iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=='

        try {
          const response = await $b24.actions.v2.call.make<ItemAddResult>({
            method: 'crm.item.add',
            params: {
              entityTypeId: 1302,
              fields: {
                ufCrm44_1721812760630: 'Value for a String-type custom field',
                ufCrm44_1721812814433: 81,
                ufCrm44_1721812853419: new Date().toISOString().slice(0, 10),
                ufCrm44_1721812885588: [
                  'example.com',
                  'second-example.com',
                ],
                ufCrm44_1721812898903: [
                  'green_pixel.png',
                  greenPixelInBase64,
                ],
                ufCrm44_1721812915476: '300|USD',
                ufCrm44_1721812935209: 'Y',
                ufCrm44_1721812948498: 9999.9,
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info(`Created item #${result.item.id} (${result.item.title})`)
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
        ```

    - JS (UMD)

        ```html
        <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
        <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
        <script>
          async function addCrmItemWithCustomFields() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const greenPixelInBase64 = 'iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=='

              const response = await $b24.actions.v2.call.make({
                method: 'crm.item.add',
                params: {
                  entityTypeId: 1302,
                  fields: {
                    ufCrm44_1721812760630: 'Value for a String-type custom field',
                    ufCrm44_1721812814433: 81,
                    ufCrm44_1721812853419: new Date().toISOString().slice(0, 10),
                    ufCrm44_1721812885588: [
                      'example.com',
                      'second-example.com',
                    ],
                    ufCrm44_1721812898903: [
                      'green_pixel.png',
                      greenPixelInBase64,
                    ],
                    ufCrm44_1721812915476: '300|USD',
                    ufCrm44_1721812935209: 'Y',
                    ufCrm44_1721812948498: 9999.9,
                  },
                },
                requestId: B24Js.Text.getUuidRfc4122()
              })

              // The payload is available only on a successful response
              if (!response.isSuccess) {
                console.error(response.getErrorMessages().join('; '))
                return
              }

              const result = response.getData().result
              console.info(`Created item #${result.item.id} (${result.item.title})`)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', addCrmItemWithCustomFields)
        </script>
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.add',
            [
                'entityTypeId' => 1302,
                'fields' => [
                    'ufCrm44_1721812760630' => "String for a String type custom field",
                    'ufCrm44_1721812814433' => 81,
                    'ufCrm44_1721812853419' => date('Y-m-d'),
                    'ufCrm44_1721812885588' => [
                        "example.com",
                        "second-example.com",
                    ],
                    'ufCrm44_1721812898903' => [
                        "green_pixel.png",
                        "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==",
                    ],
                    'ufCrm44_1721812915476' => "300|USD",
                    'ufCrm44_1721812935209' => "Y",
                    'ufCrm44_1721812948498' => 9999.9,
                ],
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

## Response Handling

HTTP status: **200**

{% note info %}

Disabled fields always return `null`.

{% endnote %}

```json
{
    "result": {
        "item": {
            "id": 342,
            "createdTime": "2024-07-18T14:00:14+02:00",
            "dateCreateShort": null,
            "updatedTime": "2024-07-18T14:00:14+02:00",
            "dateModifyShort": null,
            "createdBy": 1,
            "updatedBy": 1,
            "assignedById": 6,
            "opened": "N",
            "leadId": 102,
            "companyId": 5,
            "contactId": 4,
            "quoteId": 7,
            "title": "New deal (specifically for rest method examples)",
            "productId": null,
            "categoryId": 9,
            "stageId": "C9:UC_KN8KFI",
            "stageSemanticId": "P",
            "isNew": "N",
            "isRecurring": "N",
            "isReturnCustomer": "N",
            "isRepeatedApproach": "Y",
            "closed": "N",
            "typeId": "SERVICE",
            "opportunity": 999.99,
            "isManualOpportunity": "Y",
            "taxValue": 0,
            "currencyId": "USD",
            "probability": 50,
            "comments": "\nExample comment inside the deal\n\n[B]Bold text[/B]\n[I]Italic[/I]\n[U]Underlined[/U]\n[S]Strikethrough[/S]\n[B][I][U][S]Mix[/S][/U][/I][/B]\n\n[LIST]\n[*]List item #1\n[*]List item #2\n[*]List item #3\n[/LIST]\n\n[LIST=1]\n[*]Numbered list item #1\n[*]Numbered list item #2\n[*]Numbered list item #3\n[/LIST]\n",
            "begindate": "2024-06-18T02:00:00+02:00",
            "begindateShort": null,
            "closedate": "2024-07-30T02:00:00+02:00",
            "closedateShort": null,
            "eventDate": null,
            "eventDateShort": null,
            "eventId": null,
            "eventDescription": null,
            "locationId": null,
            "webformId": null,
            "sourceId": "WEB",
            "sourceDescription": "There should be an additional description about the source here",
            "originatorId": null,
            "originId": null,
            "additionalInfo": "There should be additional information here",
            "searchContent": null,
            "orderStage": null,
            "movedBy": 1,
            "movedTime": "2024-07-18T14:00:14+02:00",
            "lastActivityBy": 1,
            "lastActivityTime": "2024-07-18T14:00:14+02:00",
            "isWork": null,
            "isWon": null,
            "isLose": null,
            "receivedAmount": null,
            "lostAmount": null,
            "hasProducts": null,
            "ufCrm_1721244707107": 1111.1,
            "parentId1220": 2,
            "utmSource": "google",
            "utmMedium": "CPC",
            "utmCampaign": null,
            "utmContent": null,
            "utmTerm": null,
            "observers": [
                2,
                3
            ],
            "contactIds": [
                4,
                5
            ],
            "entityTypeId": 2
        }
    },
    "time": {
        "start": 1721304013.245896,
        "finish": 1721304015.555471,
        "duration": 2.309574842453003,
        "processing": 1.8328988552093506,
        "date_start": "2024-07-18T14:00:13+02:00",
        "date_finish": "2024-07-18T14:00:15+02:00",
        "operating": 1.8328571319580078
    }
}
```

{% note info " " %}

By default, custom field names are passed and returned in camelCase, for example `ufCrm2_1639669411830`.
When passing the parameter `useOriginalUfNames` with the value `Y`, custom fields will be returned with their original names, for example `UF_CRM_2_1639669411830`.

{% endnote %}

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | Root element of the response, contains a single key `item` ||
|| **item**
[`item`](./object-fields.md) | Information about the created item, [field description](./object-fields.md) ||
|| **time**
[`time`][1] | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Smart process not found"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code**                           | **Description**                                                       | **Value**                                                                                    ||
|| `403`      | `allowed_only_intranet_user`      | Action is allowed only for intranet users                   | User is not an intranet user                                                 ||
|| `400`      | `NOT_FOUND`                       | SPA not found                                            | Occurs when an invalid `entityTypeId` is passed                                              ||
|| `400`      | `ACCESS_DENIED`                   | Access denied                                                    | User does not have permission to add items of type `entityTypeId`                             ||
|| `400`      | `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid value for field "`field`"                                   | Incorrect value passed for field `field`                                                     ||
|| `400`      | `100`                             | Expected iterable value for multiple field, but got `type` instead | One of the multiple fields received a value of type `type`, while an iterable type was expected ||
|| `400`      | `CREATE_DYNAMIC_ITEM_RESTRICTED`  | You cannot create a new item due to your plan restrictions | Plan restrictions do not allow creating SPA items                              ||
|#

{% include [System errors](./../../../_includes/system-errors.md) %}


## Continue Learning


- [{#T}](crm-item-update.md)
- [{#T}](crm-item-get.md)
- [{#T}](crm-item-list.md)
- [{#T}](crm-item-delete.md)
- [{#T}](crm-item-fields.md)
- [{#T}](./object-fields.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-contractor.md)

[1]: ../../data-types.md
[2]: ../status/crm-status-list.md