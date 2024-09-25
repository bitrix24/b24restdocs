# Update CRM Item

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "edit" access permission for CRM object items

This method updates an item of a specific type in the CRM object by assigning new values from the `fields` parameter.

When updating an item, a standard series of checks, modifications, and automatic actions are performed:
- access permissions are checked
- required fields are validated if the item's stage has changed within the same direction
- dependent required fields are validated if the item's stage has changed within the same direction
- the correctness of field entries is verified
- default values are assigned to fields
- if it turns out that no field values have been changed before saving, the save **is not performed**
- automation rules are triggered after saving

## Method Parameters

{% include [Parameter Notes](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](./index.md) or [custom type](./user-defined-object-types/index.md) whose item we want to change ||
|| **id^*^**
[`integer`][1] | Identifier of the item we want to change.

Can be obtained using the [`crm.item.list`](crm-item-list.md) or [`crm.item.add`](crm-item-add.md) methods ||
|| **fields***
[`object`][1] | Object in the format
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
- `value_n` — new field value

Each CRM entity type has its own set of fields. This means that the set of fields for updating a Lead does not have to match the set of fields for updating a Contact or SPA.

The list of available fields for each entity type is described [below](#parametr-fields).

An incorrect field in `fields` will be ignored.

{% note info %}

Only those fields that need to be changed should be passed in `fields`

{% endnote %}

|| 
|#

### Fields Parameter

{% include [Parameter Notes](../../../_includes/required.md) %}

{% list tabs %}

- Lead

  CRM object identifier **entityTypeId:** `1`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Item title 
  ||
  || **honorific**
  [`crm_status`][1] | String identifier for the lead's honorific (e.g., `'HNR_US_1' = 'Mr.'`).

  The list of available honorifics can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "HONORIFIC" }` ||
  || **name**
  [`string`][1] | First name ||
  || **secondName**
  [`string`][1] | Middle name ||
  || **lastName**
  [`string`][1] | Last name ||
  || **birthdate**
  [`date`][1] | Date of birth ||
  || **companyTitle**
  [`string`][1] | Company name ||
  || **sourceId**
  [`crm_status`][1] | String identifier for the source.
  
  For example, `'CALL' = 'Call'`.
  
  The list of available sources can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }`  ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source ||
  || **stageId**
  [`crm_status`][1] | String identifier for the item's stage.
  
  For example, `'NEW' = 'Unprocessed'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "STATUS" }` ||
  || **statusDescription**
  [`text`][1] | Additional information about the stage ||
  || **post**
  [`string`][1] | Position ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the item's currency  ||
  || **isManualOpportunity**
  [`boolean`][1] | Opportunity calculation mode. Possible values:

  - `Y` — manual
  - `N` — automatic
  ||
  || **opportunity**
  [`double`][1] | Amount ||
  || **opened**
  [`boolean`][1] | Is the item available to everyone? Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **comments**
  [`text`][1] | Comment ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the item  ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the item.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the item.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the item.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3` ||
  || **originatorId**
  [`string`][1] | External source ||
  || **originId**
  [`string`][1] | Identifier of the item in the external source ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM Form ||
  || **observers**
  [`user[]`][1] | Array of identifiers of users who will be observers of the item ||
  || **utmSource**
  [`string`][1] | Advertising system. For example: Google Ads, Bing Ads, etc. ||
  || **utmMedium**
  [`string`][1] | Traffic type. Possible values:
  
  - CPC — ads
  - CPM — banners ||
  || **utmCampaign**
  [`string`][1] | Identifier of the advertising campaign ||
  || **utmContent**
  [`string`][1] | Content of the campaign. For example, for contextual ads ||
  || **utmTerm**
  [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. 
  
  Read about custom fields in the section [{#T}](./user-defined-fields/index.md) 
  
  Values of multiple fields are passed as an array.
  
  To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.
  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An item of another type of CRM object linked to this item.

  Each such field has the code `parentId + {parentEntityTypeId}` ||
  |#


- Deal

  CRM object identifier **entityTypeId:** `2`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Item title ||
  || **typeId**
  [`crm_status`][1] | String identifier for the entity type.

  For example, for a deal: `'SALE' = 'Sale'`

  The list of available entity types can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "DEAL_TYPE" }` ||
  || **categoryId**
  [`integer`][1] | Identifier of the [direction](./category/index.md) (funnel) of the deal ||
  || **stageId**
  [`crm_status`][1] | String identifier for the item's stage. 
  
  For example, `'NEW' = 'Unprocessed'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter:
    - If the deal is in the general funnel (direction)  — `{ ENTITY_ID: "DEAL_STAGE" }`
    - If the deal is not in the general funnel (direction) — `{ ENTITY_ID: "DEAL_STAGE_{categoryId}" }`, where
      `categoryId` is the identifier of the funnel ([direction](./category/index.md)) of the deal
  ||
  || **isRecurring**
  [`boolean`][1] | Is the deal recurring? Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **probability**
  [`integer`][1] | Probability % ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the item's currency ||
  || **isManualOpportunity**
  [`boolean`][1] | Opportunity calculation mode. Possible values:

  - `Y` — manual
  - `N` — automatic
  ||
  || **opportunity**
  [`double`][1] | Amount ||
  || **taxValue**
  [`double`][1] | Tax amount ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the item.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the item.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the item.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3` ||
  || **quoteId**
  [`crm_quote`][1] | Identifier of the estimate that will be linked to the deal ||
  || **begindate**
  [`date`][1] | Start date of the item ||
  || **closedate**
  [`date`][1] | End date of the item ||
  || **opened**
  [`boolean`][1] | Is the item available to everyone? Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **comments**
  [`text`][1] | Comment ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the item ||
  || **sourceId**
  [`crm_status`][1] | String identifier for the source. 
  
  For example, `'CALL' = 'Call'`.
  
  The list of available sources can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }` ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source ||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the item is created ||
  || **additionalInfo**
  [`string`][1] | Additional information ||
  || **originatorId**
  [`string`][1] | External source ||
  || **originId**
  [`string`][1] | Identifier of the item in the external source ||
  || **observers**
  [`user[]`][1] | Array of identifiers of users who will be observers of the item ||
  || **locationId**
  [`location`][1] | Identifier of the location. System field  ||
  || **utmSource**
  [`string`][1] | Advertising system. Google Ads, Bing Ads, etc. ||
  || **utmMedium**
  [`string`][1] | Traffic type. Possible values:
  
  - CPC — ads
  - CPM — banners ||
  || **utmCampaign** [`string`][1] | Identifier of the advertising campaign ||
  || **utmContent**
  [`string`][1] | Content of the campaign. For example, for contextual ads ||
  || **utmTerm**
  [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See the section [{#T}](./user-defined-fields/index.md)

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.
  
  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An item of another type of CRM object linked to this item.

  Each such field has the code `parentId + {parentEntityTypeId}` 
  ||
  |#


- Contact

  CRM object identifier **entityTypeId:** `3`

  #|
  || **Name**
  `type` | **Description** ||
  || **honorific**
  [`crm_status`][1] | String identifier for the contact's honorific. 
  
  For example, `'HNR_US_1' = 'Mr.'`.

  The list of available honorifics can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "HONORIFIC" }` ||
  || **name**
  [`string`][1] | First name ||
  || **secondName**
  [`string`][1] | Middle name ||
  || **lastName**
  [`string`][1] | Last name ||
  || **photo**
  [`file`][1] | Photo ||
  || **birthdate**
  [`date`][1] | Date of birth ||
  || **typeId**
  [`crm_status`][1] | String identifier for the entity type.
  
  For example, for a deal: `'SALE' = 'Sale'`.
  
  The list of available entity types can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "CONTACT_TYPE" }`  ||
  || **sourceId**
  [`crm_status`][1] | String identifier for the source.
  
  For example, `'CALL' = 'Call'`.
  
  The list of available sources can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }`  ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source ||
  || **post**
  [`string`][1] | Position ||
  || **comments**
  [`text`][1] | Comment ||
  || **opened**
  [`boolean`][1] | Is the item available to everyone? Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **export**
  [`boolean`][1] | Is the contact participating in the export? ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the item ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the item.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4` ||
  || **companyIds**
  [`crm_company`][1]     | Array of identifiers of companies that will be linked to the item ||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the item is created ||
  || **originatorId**
  [`string`][1] | External source ||
  || **originId**
  [`string`][1] | Identifier of the item in the external source ||
  || **originVersion**
  [`string`][1]          | Version of the original ||
  || **observers**
  [`user[]`][1] | Array of identifiers of users who will be observers of the item ||
  || **utmSource**
  [`string`][1] | Advertising system. Google Ads, Bing Ads, etc. ||
  || **utmMedium**
  [`string`][1] | Traffic type. Possible values:
  
  - CPC — ads
  - CPM — banners ||
  || **utmCampaign**
  [`string`][1] | Identifier of the advertising campaign ||
  || **utmContent**
  [`string`][1] | Content of the campaign. For example, for contextual ads ||
  || **utmTerm**
  [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See the section [{#T}](./user-defined-fields/index.md)

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An item of another type of CRM object linked to this item.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#


- Company

  CRM object identifier **entityTypeId:** `4`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Item title ||
  || **typeId**
  [`crm_status`][1] | String identifier for the entity type.
  
  For example, for a deal: `'SALE' = 'Sale'`.
  
  The list of available entity types can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "COMPANY_TYPE" }` ||
  || **logo**
  [`file`][1] | Logo ||
  || **bankingDetails**
  [`string`][1] | Banking details ||
  || **industry**
  [`crm_status`][1] | String identifier for the industry type. 
  
  For example, `'IT' = 'Information Technology'`.
  
  The list of available industry types can be obtained using the [`crm.status.list`][2] method with the filter `{ ENTITY_ID: "INDUSTRY"}` ||
  || **employees**
  [`crm_status`][1] | String identifier for the number of employees.
  
  The value is taken from the available list, for example, `'EMPLOYEES_1' = 'less than 50'`.

  The list of available employee counts can be obtained using the [`crm.status.list`][2] method with the filter `{ ENTITY_ID: "EMPLOYEES" }` ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the item's currency ||
  || **revenue**
  [`double`][1] | Annual revenue ||
  || **opened**
  [`boolean`][1] | Is the item available to everyone? Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **comments**
  [`text`][1] | Comment ||
  || **isMyCompany**
  [`boolean`][1] | Is the company my company? ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the item ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the item.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the item is created.||
  || **originatorId**
  [`string`][1] | External source ||
  || **originId**
  [`string`][1] | Identifier of the item in the external source ||
  || **originVersion**
  [`string`][1] | Version of the original ||
  || **observers**
  [`user[]`][1] | Array of identifiers of users who will be observers of the item ||
  || **utmSource**
  [`string`][1] | Advertising system. Google Ads, Bing Ads, etc. ||
  || **utmMedium**
  [`string`][1] | Traffic type. Possible values:
  - CPC — ads
  - CPM — banners ||
  || **utmCampaign**
  [`string`][1] | Identifier of the advertising campaign ||
  || **utmContent**
  [`string`][1] | Content of the campaign. For example, for contextual ads ||
  || **utmTerm**
  [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See the section [{#T}](./user-defined-fields/index.md)

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An item of another type of CRM object linked to this item.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#


- Estimate

  CRM object identifier **entityTypeId:** `7`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Item title ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the item ||
  || **opened**
  [`boolean`][1] | Is the item available to everyone? Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **content**
  [`text`][1] | Content ||
  || **terms**
  [`text`][1] | Terms ||
  || **comments**
  [`text`][1] | Comment ||
  || **dealId**
  [`crm_deal`][1] | Identifier of the linked deal ||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the item is created ||
  || **storageTypeId**
  [`integer`][1] | Identifier of the storage type. Possible values:
  - `1` — file
  - `2` — WebDAV
  - `3` — disk
  ||
  || **storageElementIds**
  [`integer`][1] | Array of files ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM Form ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the item.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the item.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the item.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3` ||
  || **locationId**
  [`location`][1] | Identifier of the location. System field ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the item's currency ||
  || **isManualOpportunity**
  [`boolean`][1] | Opportunity calculation mode.

  - `Y` — manual
  - `N` — automatic
  ||
  || **opportunity**
  [`double`][1] | Amount ||
  || **taxValue**
  [`double`][1] | Tax amount ||
  || **stageId**
  [`crm_status`][1] | String identifier for the item's stage. 
  
  For example, `'DRAFT' = 'New'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "QUOTE_STATUS" }` ||
  || **begindate**
  [`date`][1] | Start date of the item ||
  || **closedate**
  [`date`][1] | End date of the item ||
  || **actualDate**
  [`date`][1] | Valid until ||
  || **mycompanyId**
  [`crm_company`][1] | Identifier of my company ||
  || **utmSource**
  [`string`][1] | Advertising system. Google Ads, Bing Ads, etc. ||
  || **utmMedium**
  [`string`][1] | Traffic type.
  
  - CPC — ads
  - CPM — banners ||
  || **utmCampaign**
  [`string`][1] | Identifier of the advertising campaign ||
  || **utmContent**
  [`string`][1] | Content of the campaign. For example, for contextual ads ||
  || **utmTerm**
  [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See the section [{#T}](./user-defined-fields/index.md).

  - Values of multiple fields are passed as an array
  - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An item of another type of CRM object linked to this item.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#


- Invoice

  CRM object identifier **entityTypeId:** `31`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Item title
  ||
  || **xmlId**
  [`string`][1] | External code ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the item ||
  || **opened**
  [`boolean`][1] | Is the item available to everyone? Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM Form ||
  || **begindate**
  [`date`][1] | Start date of the item ||
  || **closedate**
  [`date`][1] | End date of the item ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the item.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the item.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the item.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3` ||
  || **observers**
  [`user[]`][1] | Array of identifiers of users who will be observers of the item ||
  || **stageId**
  [`crm_status`][1] | String identifier for the item's stage. 
  
  For example, `'DT31_13:N' = 'New'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter: `{ ENTITY_ID: "SMART_INVOICE_STAGE_{categoryId}" }`, where
  `categoryId` — identifier of the default invoice funnel. It can be obtained using [`crm.category.list`](category/crm-category-list.md) with `entityTypeId = 31` ||
  || **sourceId**
  [`crm_status`][1] | String identifier for the source.
  
  For example, `'CALL' = 'Call'`.
  
  The list of available sources can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }` ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the item's currency ||
  || **isManualOpportunity**
  [`boolean`][1] | Opportunity calculation mode. Possible values:

  - `Y` — manual
  - `N` — automatic
  ||
  || **opportunity**
  [`double`][1] | Amount ||
  || **taxValue**
  [`double`][1] | Tax amount ||
  || **mycompanyId**
  [`crm_company`][1] | Identifier of my company ||
  || **comments**
  [`text`][1] | Comment ||
  || **locationId**
  [`location`][1] | Identifier of the location. System field ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See the section [{#T}](./user-defined-fields/index.md).

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An item of another type of CRM object linked to this item.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#


- SPA

  CRM object identifier **entityTypeId:** can be obtained using the [`crm.type.list`](user-defined-object-types/crm-type-list.md) method or created using the [`crm.type.add`](user-defined-object-types/crm-type-add.md) method.

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Item title  ||
  || **xmlId**
  [`string`][1] | External code ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the item  ||
  || **opened**
  [`boolean`][1] | Is the item available to everyone.

  - `Y` — yes
  - `N` — no
  ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM Form ||
  || **begindate**
  [`date`][1] | Start date of the item.

  Available only if the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA.
  ||
  || **closedate**
  [`date`][1] | End date of the item.

  Available only if the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the item.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the item.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the item.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA ||
  || **observers**
  [`user[]`][1] | Array of identifiers of users who will be observers of the item.

  Available only if the `isObserversEnabled` setting is enabled for the corresponding SPA ||
  || **categoryId**
  [`crm_category`][1] | Identifier of the funnel of the SPA item.

  The list of available funnels can be obtained using the [`crm.category.list`](category/crm-category-list.md) with the corresponding `entityTypeId` ||
  || **stageId**
  [`crm_status`][1] | String identifier for the item's stage. 
  
  For example, `'DT1220_30:NEW' = 'Start'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "DYNAMIC_{entityTypeId}_STAGE_{categoryId}" }`, where
  - `entityTypeId` — identifier of the SPA type
  - `categoryId` — identifier of the funnel (direction) of the SPA item

  [Learn more about funnels (directions)](category/index.md).

  Available only if the `isStagesEnabled` setting is enabled for the corresponding SPA  ||
  || **sourceId**
  [`crm_status`][1] | String identifier for the source. (for example, `'CALL' = 'Call'`).
  
  The list of available sources can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }`.

  Available only if the `isSourceEnabled` setting is enabled for the corresponding SPA  ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source.

  Available only if the `isSourceEnabled` setting is enabled for the corresponding SPA ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the item's currency.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA  ||
  || **isManualOpportunity**
  [`boolean`][1] | Opportunity calculation mode. Possible values:

  - `Y` — manual
  - `N` — automatic

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA ||
  || **opportunity**
  [`double`][1] | Amount.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA ||
  || **taxValue**
  [`double`][1] | Tax amount.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA ||
  || **mycompanyId**
  [`crm_company`][1] | Identifier of my company.

  Available only if the `isMycompanyEnabled` setting is enabled for the corresponding SPA ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See the section [{#T}](./user-defined-fields/index.md).

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An item of another type of CRM object linked to this item.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#

  {% note info "SPA Settings" %}

  You can read more about managing SPA settings in [{#T}](./user-defined-object-types/index.md)

  {% endnote %}

{% endlist %}

## How to Update a File Type Custom Field

1. Upload a new file instead of the old one (non-multiple field)

    To replace a file in a non-multiple field, simply upload a new file. The old one will be automatically deleted.

    ```json
    {
        "fields": {
            "ufCrm1617027453943": [
                "myfile.pdf",
                "...base64_encoded_file_content..."
            ]
        }
    }
    ```

2. Remove the value of the file type custom field

    To do this, simply pass an empty string (`''`) instead of the value.

3. Leave the value of the non-multiple file type field unchanged

    The simplest option is not to add a key with this field in `fields`.
    
    But if you need to pass it and not change it, then the value should be passed as a list where the key `id` will be the file identifier.

    ```json
    {
        "fields": {
            "ufCrm1617027453943": {
                "id": 433
            }
        }
    }
    ```

    {% note warning %}
    
    If a value different from the current one is passed in `id`, the field value will be reset and the file will be deleted.
    
    {% endnote %}

4. Working with a multiple file type field

    The value of a multiple field is an array. Each element of the array is subject to the same rules as for non-multiple values.

    **How to partially overwrite values of a multiple file type field**

    For example, currently, the multiple file type field contains values `[12, 255, 44]`.

    You need to keep files `12` and `44`, and upload a new one instead of `255`.

    The request should look as follows:

    ```json
    {
        "fields": {
            "ufCrm1617027453943": [
                {
                    "id": 12
                },
                {
                    "id": 44
                },
                [
                    "myNewFile.pdf",
                    "...base64_encoded_file_content..."
                ]
            ]
        }
    }
    ```

## Code Examples

Update a deal with `id = 351`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"id":351,"fields":{"title":"REST Deal #1","stageId":"C9:UC_NYL06U","assignedById":6,"observers":[1,2,3],"opened":"N","typeId":"SERVICE","opportunity":10000,"currencyId":"USD","additionalInfo":"Updating deal via REST","isManualOpportunity":"N","utmSource":"google","ufCrm_1721244707107":200.05,"parentId1220":[2,1]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"id":351,"fields":{"title":"REST Deal #1","stageId":"C9:UC_NYL06U","assignedById":6,"observers":[1,2,3],"opened":"N","typeId":"SERVICE","opportunity":10000,"currencyId":"USD","additionalInfo":"Updating deal via REST","isManualOpportunity":"N","utmSource":"google","ufCrm_1721244707107":200.05,"parentId1220":[2,1]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.update
    ```

- JS

    ```js
        BX24.callMethod(
            'crm.item.update',
            {
                entityTypeId: 2,
                id: 351,
                fields: {
                    title: "REST Deal #1",
                    stageId: "C9:UC_NYL06U",
                    assignedById: 6,
                    observers: [1, 2, 3],
                    opened: "N",
                    typeId: "SERVICE",
                    opportunity: 10000,
                    currencyId: "USD",
                    additionalInfo: "Updating deal via REST",
                    isManualOpportunity: "N",
                    utmSource: "google",
                    ufCrm_1721244707107: 200.05,
                    parentId1220: [2, 1],
                },
            },
            (result) => {
                if (result.error())
                {
                    console.error(result.error());

                    return;
                }

                console.info(result.data());
            },
        );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.update',
        [
            'entityTypeId' => 2,
            'id' => 351,
            'fields' => [
                'title' => "REST Deal #1",
                'stageId' => "C9:UC_NYL06U",
                'assignedById' => 6,
                'observers' => [1, 2, 3],
                'opened' => "N",
                'typeId' => "SERVICE",
                'opportunity' => 10000,
                'currencyId' => "USD",
                'additionalInfo' => "Updating deal via REST",
                'isManualOpportunity' => "N",
                'utmSource' => "google",
                'ufCrm_1721244707107' => 200.05,
                'parentId1220' => [2, 1],
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 351,
            "createdTime": "2024-07-23T19:10:26+02:00",
            "dateCreateShort": null,
            "updatedTime": "2024-07-23T18:19:21+02:00",
            "dateModifyShort": null,
            "createdBy": 1,
            "updatedBy": 1,
            "assignedById": 6,
            "opened": "N",
            "leadId": null,
            "companyId": 0,
            "contactId": 0,
            "quoteId": null,
            "title": "REST Deal #1",
            "productId": null,
            "categoryId": 9,
            "stageId": "C9:UC_NYL06U",
            "stageSemanticId": "P",
            "isNew": "N",
            "isRecurring": "N",
            "isReturnCustomer": "N",
            "isRepeatedApproach": "N",
            "closed": "N",
            "typeId": "SERVICE",
            "opportunity": 10000,
            "isManualOpportunity": "N",
            "taxValue": 0,
            "currencyId": "USD",
            "probability": null,
            "comments": "",
            "begindate": "2024-07-23T02:00:00+02:00",
            "begindateShort": null,
            "closedate": "2024-07-31T02:00:00+02:00",
            "closedateShort": null,
            "eventDate": null,
            "eventDateShort": null,
            "eventId": null,
            "eventDescription": null,
            "locationId": null,
            "webformId": 0,
            "sourceId": "",
            "sourceDescription": "",
            "originatorId": null,
            "originId": null,
            "additionalInfo": "Updating deal via REST",
            "searchContent": "351 Deal #351 10200.00 US dollar No Did Not Create Sale Title2134234233 07/23/2024 07/31/2024",
            "orderStage": null,
            "movedBy": 1,
            "movedTime": "2024-07-23T18:19:21+02:00",
            "lastActivityBy": 1,
            "lastActivityTime": "2024-07-23T18:10:26+02:00",
            "isWork": null,
            "isWon": null,
            "isLose": null,
            "receivedAmount": null,
            "lostAmount": null,
            "hasProducts": null,
            "ufCrm_1721244707107": 200.05,
            "parentId1220": [
                "2",
                "1"
            ],
            "utmSource": "google",
            "utmMedium": null,
            "utmCampaign": null,
            "utmContent": null,
            "utmTerm": null,
            "observers": [
                1,
                2,
                3
            ],
            "contactIds": [],
            "entityTypeId": 2
        }
    },
    "time": {
        "start": 1721751560.824475,
        "finish": 1721751564.481578,
        "duration": 3.6571030616760254,
        "processing": 3.1893951892852783,
        "date_start": "2024-07-23T18:19:20+02:00",
        "date_finish": "2024-07-23T18:19:24+02:00",
        "operating": 3.1893470287323
    }
}
```

### Returned Values

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | Root element of the response, contains a single key `item` ||
|| **item**
[`item`](./crm-item-add.md#item) | Information about the updated item ||
|| **time**
[`time`][1] | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "NOT_FOUND",
    "error_description": "SPA not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code**                           | **Description**                                                       | **Value**                                                                                    ||
|| `403`      | `allowed_only_intranet_user`      | Action allowed only for intranet users                               | User is not an intranet user                                                               ||
|| `400`      | `NOT_FOUND`                       | SPA not found                                                        | Occurs when an invalid `entityTypeId` is passed                                          ||
|| `400`      | `ACCESS_DENIED`                   | Access denied                                                        | User does not have permission to modify items of type `entityTypeId`                      ||
|| `400`      | `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid value for field "`field`"                                   | An incorrect value for the field `field` was passed                                       ||
|| `400`      | `100`                             | Expected iterable value for multiple field, but got `type` instead | One of the multiple fields received a value of type `type`, while an iterable type was expected ||
|| `400`      | `-`                               | Insufficient rights to change the stage                              | If the user tries to change the stage of the item while lacking sufficient rights          ||
|| `400`      | `UPDATE_DYNAMIC_ITEM_RESTRICTED`  | You cannot change the item due to your plan's restrictions          | Plan restrictions do not allow changing SPA items                                          ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](crm-item-add.md)
- [{#T}](crm-item-get.md)
- [{#T}](crm-item-list.md)
- [{#T}](crm-item-delete.md)
- [{#T}](crm-item-fields.md)

[1]: ../../data-types.md
[2]: ../data-types.md
[3]: ../status/crm-status-list.md