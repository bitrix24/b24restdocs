# Create a New CRM Entity crm.item.add

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with the "add" access permission for the CRM entity

This method is a universal way to create objects in CRM. With it, you can create various types of objects, such as deals, contacts, companies, and more.

To create an object, you need to pass the appropriate parameters, including the object type and its information: name, description, contact details, and other specifics.

Upon successful execution of the request, a new object is created.

This method provides a flexible way to automate the object creation process and integrate CRM with other systems.

When creating an entity, a standard series of checks, modifications, and automatic actions are performed:

- access permissions are checked
- required fields are validated for completion
- stage-dependent required fields are validated for completion
- correctness of field entries is verified
- default values are assigned to fields
- automation rules are triggered after saving

Next, let's take a closer look at how to use this method and what parameters need to be passed.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type`          | **Description**                                                                                                                        ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](./index.md) or [user-defined type](user-defined-object-types/index.md) whose element we want to create ||
|| **fields***
[`object`][1]  | Object format.

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

Each CRM entity type has its own set of fields. This means that the set of fields for creating a Lead may not match the set of fields for creating a Contact or SPA.

The list of available fields for each entity type is described [below](#parametr-fields).

An incorrect field in `fields` will be ignored
||
|#

### Fields Parameter

{% include [Footnote on parameters](../../../_includes/required.md) %}

{% list tabs %}

- Lead

  CRM object identifier **entityTypeId:** `1`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
  - `entityTypeName` — entity name
  - `id` — element identifier
  
  For example, for a lead with `id = 13` — 'Lead #13' 
  ||
  || **honorific**
  [`crm_status`][1] | String identifier for the lead's honorific (e.g., `'HNR_US_1' = 'Mr.'`).

  The list of available honorifics can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "HONORIFIC" }`.

  By default — `null` ||
  || **name**
  [`string`][1] | First name.

  By default — `null` ||
  || **secondName**
  [`string`][1] | Middle name.

  By default — `null` ||
  || **lastName**
  [`string`][1] | Last name.

  By default — `null` ||
  || **birthdate**
  [`date`][1] | Date of birth.

  By default — `null` ||
  || **companyTitle**
  [`string`][1] | Company name.

  By default — `null` ||
  || **sourceId**
  [`crm_status`][1] | String identifier for the source.
  
  For example, `'CALL' = 'Call'`.
  
  The list of available sources can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }`.

  By default, it takes the value of the first available source  ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source.

  By default — `null` ||
  || **stageId**
  [`crm_status`][1] | String identifier for the element's stage.
  
  For example, `'NEW' = 'Not Processed'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "STATUS" }`

  By default, it takes the value of the first available stage  ||
  || **statusDescription**
  [`text`][1] | Additional information about the stage.

  By default — `null` ||
  || **post**
  [`string`][1] | Position.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the element's currency.

  By default, it takes the default currency  ||
  || **isManualOpportunity**
  [`boolean`][1] | Mode of calculating the amount. Possible values:

  - `Y` — manual
  - `N` — automatic

  By default — `N` ||
  || **opportunity**
  [`double`][1] | Amount.

  By default — `null` ||
  || **opened**
  [`boolean`][1] | Is the element available to everyone? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in CRM settings  ||
  || **comments**
  [`text`][1] | Comment.

  By default — `null` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the element.

  By default, this is the identifier of the user calling the method  ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **originatorId**
  [`string`][1] | External source.

  By default — `null` ||
  || **originId**
  [`string`][1] | Identifier of the element in the external source.

  By default — `null` ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM Form.

  By default — `null` ||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be Observers in the element.

  By default — `null` ||
  || **utmSource**
  [`string`][1] | Advertising system. For example: Yandex-Direct, Google-Adwords, etc.

  By default — `null` ||
  || **utmMedium**
  [`string`][1] | Type of traffic. Possible values:
  
  - CPC — ads
  - CPM — banners

  By default — `null` ||
  || **utmCampaign**
  [`string`][1] | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`][1] | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising.

  By default equals `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. 
  
  Read about custom fields in the section [{#T}](./user-defined-fields/index.md) 
  
  Values of multiple fields are passed as an array.
  
  To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.
  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}` ||
  |#


- Deal

  CRM object identifier **entityTypeId:** `2`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
  - `entityTypeName` — entity name
  - `id` — element identifier
  For example, for a deal with `id = 13` => 'Deal #13' ||
  || **typeId**
  [`crm_status`][1] | String identifier for the entity type.

  For example, for a deal: `'SALE' = 'Sale'`

  The list of available entity types can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "DEAL_TYPE" }`

  By default — the first available entity type ||
  || **categoryId**
  [`integer`][1] | Identifier of the [direction](./category/index.md) (funnel) of the deal.

  By default — `0` (general) ||
  || **stageId**
  [`crm_status`][1] | String identifier for the element's stage. 
  
  For example, `'NEW' = 'Not Processed'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter:
    - If the deal is in the general funnel (direction)  — `{ ENTITY_ID: "DEAL_STAGE" }`
    - If the deal is not in the general funnel (direction) — `{ ENTITY_ID: "DEAL_STAGE_{categoryId}" }`, where
      `categoryId` is the identifier of the funnel ([direction](./category/index.md)) of the deal

  By default — the first available stage relative to the funnel ||
  || **isRecurring**
  [`boolean`][1] | Is the deal recurring? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `N`||
  || **probability**
  [`integer`][1] | Probability %.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the element's currency.

  By default — default currency ||
  || **isManualOpportunity**
  [`boolean`][1] | Mode of calculating the amount. Possible values:

  - `Y` — manual
  - `N` — automatic

  By default — `N` ||
  || **opportunity**
  [`double`][1] | Amount.

  By default — `null` ||
  || **taxValue**
  [`double`][1] | Tax amount.

  By default — `null` ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **quoteId**
  [`crm_quote`][1] | Identifier of the estimate that will be linked to the deal ||
  || **begindate**
  [`date`][1] | Start date of the element.

  By default — creation date ||
  || **closedate**
  [`date`][1] | End date of the element.

  By default — creation date + 7 days ||
  || **opened**
  [`boolean`][1] | Is the element available to everyone? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in CRM settings ||
  || **comments**
  [`text`][1] | Comment.

  By default — `null` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the element.

  By default — the identifier of the user calling the method ||
  || **sourceId**
  [`crm_status`][1] | String identifier for the source. 
  
  For example, `'CALL' = 'Call'`.
  
  The list of available sources can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }`.

  By default — the first available source ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source.

  By default — `null`||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the element is created.

  By default — `null`||
  || **additionalInfo**
  [`string`][1] | Additional information.

  By default — `null` ||
  || **originatorId**
  [`string`][1] | External source.

  By default — `null`||
  || **originId**
  [`string`][1] | Identifier of the element in the external source.

  By default — `null`||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be Observers in the element.

  By default — `null` ||
  || **locationId**
  [`location`][1] | Identifier of the location. System field.

  By default — `null` ||
  || **utmSource**
  [`string`][1] | Advertising system. Yandex-Direct, Google-Adwords, etc.

  By default — `null` ||
  || **utmMedium**
  [`string`][1] | Type of traffic. Possible values:
  
  - CPC — ads
  - CPM — banners

  By default — `null` ||
  || **utmCampaign**
  [`string`][1] | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`][1] | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See the section [{#T}](./user-defined-fields/index.md)

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.
  
  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object that is linked to this element.

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

  The list of available honorifics can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "HONORIFIC" }`.

  By default — `null` ||
  || **name**
  [`string`][1] | First name.

  By default — `null` ||
  || **secondName**
  [`string`][1] | Middle name.

  By default — `null` ||
  || **lastName**
  [`string`][1] | Last name.

  By default — `null` ||
  || **photo**
  [`file`][1] | Photo.

  By default — `null` ||
  || **birthdate**
  [`date`][1] | Date of birth.

  By default — `null` ||
  || **typeId**
  [`crm_status`][1] | String identifier for the entity type.
  
  For example, for a deal: `'SALE' = 'Sale'`.
  
  The list of available entity types can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "CONTACT_TYPE" }`.

  By default — the first available entity type  ||
  || **sourceId**
  [`crm_status`][1] | String identifier for the source.
  
  For example, `'CALL' = 'Call'`.
  
  The list of available sources can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }`.

  By default — the first available source  ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source.

  By default — `null` ||
  || **post**
  [`string`][1] | Position.

  By default — `null` ||
  || **comments**
  [`text`][1] | Comment.

  By default — `null` ||
  || **opened**
  [`boolean`][1] | Is the element available to everyone? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in CRM settings  ||
  || **export**
  [`boolean`][1] | Is the contact included in the export?

  By default — `Y` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the element.

  By default — the identifier of the user calling the method ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **companyIds**
  [`crm_company`][1]     | Array of identifiers of companies that will be linked to the element ||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the element is created.

  By default — `null` ||
  || **originatorId**
  [`string`][1] | External source.

  By default — `null` ||
  || **originId**
  [`string`][1] | Identifier of the element in the external source.

  By default — `null` ||
  || **originVersion**
  [`string`][1]          | Version of the original.

  By default — `null` ||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be Observers in the element.

  By default — `null` ||
  || **utmSource**
  [`string`][1] | Advertising system. Yandex-Direct, Google-Adwords, etc.

  By default — `null` ||
  || **utmMedium**
  [`string`][1] | Type of traffic. Possible values:
  
  - CPC — ads
  - CPM — banners

  By default — `null` ||
  || **utmCampaign**
  [`string`][1] | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`][1] | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See the section [{#T}](./user-defined-fields/index.md)

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}` 
  ||
  |#


- Company

  CRM object identifier **entityTypeId:** `4`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
  
  - `entityTypeName` — entity name
  - `id` — element identifier
  
  For example, for a company with `id = 13` => 'Company #13' ||
  || **typeId**
  [`crm_status`][1] | String identifier for the entity type.
  
  For example, for a deal: `'SALE' = 'Sale'`.
  
  The list of available entity types can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "COMPANY_TYPE" }`.

  By default — the first available entity type ||
  || **logo**
  [`file`][1] | Logo.

  By default — `null` ||
  || **bankingDetails**
  [`string`][1] | Banking details.

  By default — `null` ||
  || **industry**
  [`crm_status`][1] | String identifier for the industry type. 
  
  For example, `'IT' = 'Information Technology'`.
  
  The list of available industry types can be obtained using the [`crm.status.list`][2] method with the filter `{ ENTITY_ID: "INDUSTRY"}`.

  By default — the first available industry type ||
  || **employees**
  [`crm_status`][1] | String identifier for the number of employees.
  
  The value is taken from the list of available types, for example, `'EMPLOYEES_1' = 'less than 50'`.

  The list of available employee counts can be obtained using the [`crm.status.list`][2] method with the filter `{ ENTITY_ID: "EMPLOYEES" }`.

  By default — the first available employee count type ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the element's currency.

  By default — default currency ||
  || **revenue**
  [`double`][1] | Annual revenue.

  By default — `0` ||
  || **opened**
  [`boolean`][1] | Is the element available to everyone? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in CRM settings ||
  || **comments**
  [`text`][1] | Comment.

  By default — `null` ||
  || **isMyCompany**
  [`boolean`][1] | Is the company my company?

  By default — `N` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the element.

  By default — the identifier of the user calling the method ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null`||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the element is created.

  By default — `null`||
  || **originatorId**
  [`string`][1] | External source.

  By default — `null` ||
  || **originId**
  [`string`][1] | Identifier of the element in the external source.

  By default — `null` ||
  || **originVersion**
  [`string`][1] | Version of the original.

  By default — `null` ||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be Observers in the element.

  By default — `null` ||
  || **utmSource**
  [`string`][1] | Advertising system. Yandex-Direct, Google-Adwords, etc.

  By default — `null` ||
  || **utmMedium**
  [`string`][1] | Type of traffic. Possible values:
  - CPC — ads
  - CPM — banners

  By default — `null` ||
  || **utmCampaign**
  [`string`][1] | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`][1] | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See the section [{#T}](./user-defined-fields/index.md)

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}` 
  ||
  |#


- Estimate

  CRM object identifier **entityTypeId:** `7`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
  - `entityTypeName` — entity name
  - `id` — element identifier
  
  For example, for an estimate with `id = 13` => 'Estimate #13' ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the element.

  By default — the identifier of the user calling the method ||
  || **opened**
  [`boolean`][1] | Is the element available to everyone? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in CRM settings ||
  || **content**
  [`text`][1] | Content.

  By default — `null` ||
  || **terms**
  [`text`][1] | Terms.

  By default — `null` ||
  || **comments**
  [`text`][1] | Comment.

  By default — `null` ||
  || **dealId**
  [`crm_deal`][1]        | Identifier of the linked deal.

  By default — `null` ||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the element is created.

  By default — `null` ||
  || **storageTypeId**
  [`integer`][1] | Identifier of the storage type. Possible values:
  - `1` — file
  - `2` — WebDAV
  - `3` — disk

  By default:
  1. If the `disk` module is enabled -> Disk
  2. If the `webdav` module is enabled -> WebDAV
  3. File 
  ||
  || **storageElementIds**
  [`integer`][1] | Array of files.

  By default — `null` ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM Form.

  By default — `null` ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **locationId**
  [`location`][1] | Identifier of the location. System field.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the element's currency.

  By default — default currency ||
  || **isManualOpportunity**
  [`boolean`][1] | Mode of calculating the amount.

  - `Y` — manual
  - `N` — automatic

  By default — `N` ||
  || **opportunity**
  [`double`][1] | Amount.

  By default — `null` ||
  || **taxValue**
  [`double`][1] | Tax amount.

  By default — `null` ||
  || **stageId**
  [`crm_status`][1] | String identifier for the element's stage. 
  
  For example, `'DRAFT' = 'New'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "QUOTE_STATUS" }`.

  By default — the first available stage ||
  || **begindate**
  [`date`][1] | Start date of the element.

  By default — creation date of the element ||
  || **closedate**
  [`date`][1] | End date of the element.

  By default — creation date of the element + 7 days ||
  || **actualDate**
  [`date`][1] | Valid until.

  By default — creation date of the element + 7 days ||
  || **mycompanyId**
  [`crm_company`][1] | Identifier of my company.

  By default — identifier of the first available "my" company ||
  || **utmSource**
  [`string`][1] | Advertising system. Yandex-Direct, Google-Adwords, etc.

  By default — `null` ||
  || **utmMedium**
  [`string`][1] | Type of traffic.
  
  - CPC — ads
  - CPM — banners

  By default — `null` ||
  || **utmCampaign**
  [`string`][1] | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`][1] | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See the section [{#T}](./user-defined-fields/index.md).

  - Values of multiple fields are passed as an array
  - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#


- Invoice

  CRM object identifier **entityTypeId:** `31`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
  
  - `entityTypeName` — entity name
  - `id` — element identifier
  
  For example, for an invoice with `id = 13` => 'Invoice #13'
  ||
  || **xmlId**
  [`string`][1] | External code.

  By default — `null` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the element.

  By default — the identifier of the user calling the method ||
  || **opened**
  [`boolean`][1] | Is the element available to everyone? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in CRM settings ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM Form.

  By default — `null` ||
  || **begindate**
  [`date`][1] | Start date of the element.

  By default — creation date of the element ||
  || **closedate**
  [`date`][1] | End date of the element.

  By default — creation date of the element + 7 days ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be Observers in the element.

  By default — `null` ||
  || **stageId**
  [`crm_status`][1] | String identifier for the element's stage. 
  
  For example, `'DT31_13:N' = 'New'`.

  The list of available stages can be obtained using [`crm.status.list`][2], with the filter: `{ ENTITY_ID: "SMART_INVOICE_STAGE_{categoryId}" }`, where
  `categoryId` — identifier of the default invoice funnel. It can be obtained using [`crm.category.list`](category/crm-category-list.md) with `entityTypeId = 31`.

  By default — the first available stage ||
  || **sourceId**
  [`crm_status`][1] | String identifier for the source.
  
  For example, `'CALL' = 'Call'`.
  
  The list of available sources can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }`.

  By default — the first available source ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the element's currency.

  By default — default currency ||
  || **isManualOpportunity**
  [`boolean`][1] | Mode of calculating the amount. Possible values:

  - `Y` — manual
  - `N` — automatic

  By default — `N` ||
  || **opportunity**
  [`double`][1] | Amount.

  By default — `null` ||
  || **taxValue**
  [`double`][1] | Tax amount.

  By default — `null` ||
  || **mycompanyId**
  [`crm_company`][1] | Identifier of my company.

  By default — identifier of the first available "my" company ||
  || **comments**
  [`text`][1] | Comment.

  By default — `null` ||
  || **locationId**
  [`location`][1] | Identifier of the location. System field.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See the section [{#T}](./user-defined-fields/index.md).

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}` 
  ||
  |#


- SPA

  CRM object identifier **entityTypeId:** can be obtained using the [`crm.type.list`](user-defined-object-types/crm-type-list.md) method or created using the [`crm.type.add`](user-defined-object-types/crm-type-add.md) method.

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
  - `entityTypeName` — name of the SPA
  - `id` — element identifier
  
  For example, for the SPA element "HR" with `id = 13` => 'HR #13'  ||
  || **xmlId**
  [`string`][1] | External code.

  By default — `null` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the element.

  By default — the identifier of the user calling the method  ||
  || **opened**
  [`boolean`][1] | Is the element available to everyone?

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in CRM settings  ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM Form.

  By default — `null` ||
  || **begindate**
  [`date`][1] | Start date of the element.

  Available only if the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA.

  By default — creation date of the element  ||
  || **closedate**
  [`date`][1] | End date of the element.

  Available only if the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA.

  By default — creation date of the element + 7 days  ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be Observers in the element.

  Available only if the `isObserversEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **categoryId**
  [`crm_category`][1] | Identifier of the funnel of the SPA element.

  The list of available funnels can be obtained using the [`crm.category.list`](category/crm-category-list.md) method with the corresponding `entityTypeId` ||
  || **stageId**
  [`crm_status`][1] | String identifier for the element's stage. 
  
  For example, `'DT1220_30:NEW' = 'Start'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "DYNAMIC_{entityTypeId}_STAGE_{categoryId}" }`, where
  - `entityTypeId` — identifier of the SPA type
  - `categoryId` — identifier of the funnel (direction) of the SPA element

  [Learn more about funnels (directions)](category/index.md).

  Available only if the `isStagesEnabled` setting is enabled for the corresponding SPA.

  By default — the first available stage relative to the funnel  ||
  || **sourceId**
  [`crm_status`][1] | String identifier for the source. (for example, `'CALL' = 'Call'`).
  
  The list of available sources can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }`.

  Available only if the `isSourceEnabled` setting is enabled for the corresponding SPA.

  By default — the first available source  ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source.

  Available only if the `isSourceEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the element's currency.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

  By default — default currency  ||
  || **isManualOpportunity**
  [`boolean`][1] | Mode of calculating the amount. Possible values:

  - `Y` — manual
  - `N` — automatic

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

  By default — `N` ||
  || **opportunity**
  [`double`][1] | Amount.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **taxValue**
  [`double`][1] | Tax amount.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **mycompanyId**
  [`crm_company`][1] | Identifier of my company.

  Available only if the `isMycompanyEnabled` setting is enabled for the corresponding SPA.

  By default — Identifier of the first available "my" company ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See the section [{#T}](./user-defined-fields/index.md).

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}` 
  ||
  |#

  {% note info "SPA Settings" %}

  You can read more about managing SPA settings in [{#T}](./user-defined-object-types/index.md)

  {% endnote %}

{% endlist %}

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

1. Example of creating a deal

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"fields":{"title":"New deal (specifically for the REST methods example)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":[1,2]}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.add
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"fields":{"title":"New deal (specifically for the REST methods example)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":[1,2]},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.add
        ```

    - JS

        ```js
        const formatDate = (date) => {
            return date.toISOString().slice(0, 10);
        };

        const day = 60 * 60 * 24 * 1000;

        const now = new Date();
        const twelveDaysInAdvance = new Date(now.getTime() + 12 * day);
        const monthAgo = new Date(now.getTime() - 30 * day);

        const commentsExample = `
        Example comment within the deal

        [B]Bold text[/B]
        [I]Italic[/I]
        [U]Underlined[/U]
        [S]Strikethrough[/S]
        [B][I][U][S]Mix[/S][/U][/I][/B]

        [LIST]
        [*]List item #1
        [*]List item #2
        [*]List item #3
        [/LIST]

        [LIST=1]
        [*]Numbered list item #1
        [*]Numbered list item #2
        [*]Numbered list item #3
        [/LIST]
        `;

        BX24.callMethod(
            'crm.item.add', 
            {
                entityTypeId: 2,
                fields: 
                {
                    title: "New deal (specifically for the REST methods example)",
                    typeId: "SERVICE",
                    categoryId: 9,
                    stageId: "C9:UC_KN8KFI",
                    isReccurring: "Y",
                    probability: 50,
                    currencyId: "USD",
                    isManualOpportunity: "Y",
                    opportunity: 999.99,
                    taxValue: 99.9,
                    companyId: 5,
                    contactId: 4,
                    contactIds: [4, 5],
                    quoteId: 7,
                    begindate: formatDate(monthAgo),
                    closedate: formatDate(twelveDaysInAdvance),
                    opened: "N",
                    comments: commentsExample,
                    assignedById: 6,
                    sourceId: "WEB",
                    sourceDescription: "There should be additional description about the source",
                    leadId: 102,
                    additionalInfo: "There should be additional information",
                    observers: [2, 3],
                    utmSource: "google",
                    utmMedium: "CPC",
                    ufCrm_1721244707107: 1111.1,
                    parentId1220: [
                        1,
                        2,
                    ],
                },
            },
            (result) => 
            {
                result.error() 
                    ? console.error(result.error()) 
                    : console.info(result.data())
                ;
            }
        );
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.add',
            [
                'entityTypeId' => 2,
                'fields' => [
                    'title' => "New deal (specifically for the REST methods example)",
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
                    'sourceDescription' => "There should be additional description about the source",
                    'leadId' => 102,
                    'additionalInfo' => "There should be additional information",
                    'observers' => [2, 3],
                    'utmSource' => "google",
                    'utmMedium' => "CPC",
                    'ufCrm_1721244707107' => 1111.1,
                    'parentId1220' => [
                        1,
                        2,
                    ],
                ],
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

 - B24-PHP-SDK

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
            "ru": "Custom field (Yes/No)",
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
                "ufCrm44_1721812760630": "String for custom field of type String",
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
                "ufCrm44_1721812760630": "String for custom field of type String",
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

    - JS

        ```js
        const greenPixelInBase64 = "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==";

        BX24.callMethod(
            'crm.item.add', 
            {
                entityTypeId: 1302,
                fields: {
                    ufCrm44_1721812760630: "String for custom field of type String",
                    ufCrm44_1721812814433: 81,
                    ufCrm44_1721812853419: (new Date()).toISOString().slice(0, 10),
                    ufCrm44_1721812885588: [
                        "example.com",
                        "second-example.com",
                    ],
                    ufCrm44_1721812898903: [
                        "green_pixel.png",
                        greenPixelInBase64,
                    ],
                    ufCrm44_1721812915476: "300|USD",
                    ufCrm44_1721812935209: "Y",
                    ufCrm44_1721812948498: 9999.9,
                },
            },
            (result) => 
            {
                result.error() 
                    ? console.error(result.error()) 
                    : console.info(result.data())
                ;
            }
        );
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.add',
            [
                'entityTypeId' => 1302,
                'fields' => [
                    'ufCrm44_1721812760630' => "String for custom field of type String",
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
            "title": "New deal (specifically for the REST methods example)",
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
            "comments": "\nExample comment within the deal\n\n[B]Bold text[/B]\n[I]Italic[/I]\n[U]Underlined[/U]\n[S]Strikethrough[/S]\n[B][I][U][S]Mix[/S][/U][/I][/B]\n\n[LIST]\n[*]List item #1\n[*]List item #2\n[*]List item #3\n[/LIST]\n\n[LIST=1]\n[*]Numbered list item #1\n[*]Numbered list item #2\n[*]Numbered list item #3\n[/LIST]\n",
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
            "sourceDescription": "There should be additional description about the source",
            "originatorId": null,
            "originId": null,
            "additionalInfo": "There should be additional information",
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
            "parentId1220": [
                "1",
                "2"
            ],
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

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | The root element of the response, contains a single key `item` ||
|| **item**
[`item`](#item) | Information about the created element ||
|| **time**
[`time`][1] | Information about the request execution time ||
|#

#### Object item {#item}

{% note info %}

Disabled fields always return `null`

{% endnote %}

{% list tabs %}

- Lead

  #|
  || **Name**
  `type` | **Description** ||
  || **id**                  
  [`integer`][1]        | Identifier of the element                                                                              ||
  || **createdTime**         
  [`datetime`][1]       | Creation time of the element                                                                             ||
  || **dateCreateShort**     
  [`datetime`][1]       | Creation time of the element (short format).
 
  Field is disabled                                            ||
  || **updatedTime**         
  [`datetime`][1]       | Last modification time of the element                                                                 ||
  || **dateModifyShort**     
  [`datetime`][1]       | Last modification time of the element (short format).
 
  Field is disabled                                ||
  || **createdBy**           
  [`user`][1]           | Identifier of the user who created the element                                                  ||
  || **updatedBy**           
  [`user`][1]           | Identifier of the user who modified the element                                                 ||
  || **assignedById**        
  [`user`][1]           | Identifier of the user responsible for the element                                                ||
  || **opened**              
  [`boolean`][1]        | Is the element open?                                                                        ||
  || **companyId**           
  [`crm_company`][1]    | Identifier of the company associated with the element                                                      ||
  || **contactId**           
  [`crm_contact`][1]    | Identifier of the contact associated with the element                                                     ||
  || **stageId**             
  [`crm_status`][1]     | String identifier of the element's stage                                                             ||
  || **isConvert**           
  [`boolean`][1]        | Has the lead been converted?
  
  Field is disabled                                                               ||
  || **statusDescription**   
  [`text`][1]           | Additional information about the stage                                                                              ||
  || **stageSemanticId**     
  [`string`][1]         | Stage group. Possible values:
  
  - `P` — in progress
  - `S` — successful
  - `F` — unsuccessful
  ||
  || **productId**           
  [`string`][1]         | Identifier of the product.
  
  Deprecated.
  
  Field is disabled                                                      ||
  || **opportunity**         
  [`double`][1]         | Amount                                                                                               ||
  || **currencyId**          
  [`crm_currency`][1]   | Identifier of the element's currency                                                                       ||
  || **sourceId**            
  [`crm_status`][1]     | String identifier of the source type                                                              ||
  || **sourceDescription**   
  [`text`][1]           | Additional information about the source                                                                          ||
  || **title**               
  [`string`][1]         | Name of the element                                                                                   ||
  || **name**                
  [`string`][1]         | First name                                                                                                 ||
  || **lastName**            
  [`string`][1]         | Last name                                                                                             ||
  || **secondName**          
  [`string`][1]         | Middle name                                                                                            ||
  || **shortName**           
  [`string`][1]         | Last name First name.
  
  Short format: for example 'Ivanov Ivan' -> 'Ivanov I.'.
  
  Field is disabled                 ||
  || **companyTitle**        
  [`string`][1]         | Company name                                                                                   ||
  || **post**                
  [`string`][1]         | Position                                                                                           ||
  || **address**             
  [`text`][1]           | Address.
  
  Deprecated.
  
  Field is disabled                                                                     ||
  || **comments**            
  [`text`][1]           | Comment                                                                                         ||
  || **webformId**           
  [`integer`][1]        | Identifier of the CRM form                                                                             ||
  || **originatorId**        
  [`string`][1]         | External source                                                                                    ||
  || **originId**            
  [`string`][1]         | Identifier of the element in the external source                                                         ||
  || **dateClosed**          
  [`datetime`][1]       | Closing time of the element                                                                             ||
  || **birthdate**           
  [`date`][1]           | Date of birth                                                                                       ||
  || **honorific**           
  [`crm_status`][1]     | String identifier of the salutation type                                                              ||
  || **hasPhone**            
  [`boolean`][1]        | Does the element have a phone number?                                                                       ||
  || **hasEmail**            
  [`boolean`][1]        | Does the element have an email?                                                                         ||
  || **hasImol**             
  [`boolean`][1]        | Does the element have open lines?                                                                ||
  || **login**               
  [`string`][1]         | Login.
  
  Deprecated.
  
  Field is disabled                                                                    ||
  || **isReturnCustomer**    
  [`boolean`][1]        | Is the element a repeat customer?                                                                       ||
  || **searchContent**       
  [`text`][1]           | Information for full-text search.
  
  Service field                                               ||
  || **isManualOpportunity** 
  [`boolean`][1]        | Is manual mode for calculating the amount set?                                                            ||
  || **movedBy**             
  [`user`][1]           | Identifier of the user who last changed the stage                                         ||
  || **movedTime**           
  [`datetime`][1]       | Time of the last stage change                                                                        ||
  || **lastActivityBy**      
  [`user`][1]           | Identifier of the user who last showed activity in the timeline                       ||
  || **lastActivityTime**    
  [`datetime`][1]       | Time of the last activity in the timeline                                                 ||
  || **phoneMobile**         
  [`string`][1]         | Mobile phone                                                                                   ||
  || **phoneWork**           
  [`string`][1]         | Work phone                                                                                     ||
  || **phoneMailing**        
  [`string`][1]         | Mailing phone                                                                                ||
  || **emailHome**           
  [`string`][1]         | Personal e-mail                                                                                       ||
  || **emailWork**           
  [`string`][1]         | Work e-mail                                                                                      ||
  || **emailMailing**        
  [`string`][1]         | Mailing e-mail                                                                                  ||
  || **skype**               
  [`string`][1]         | Skype                                                                                               ||
  || **icq**                 
  [`string`][1]         | ICQ                                                                                                 ||
  || **imol**                
  [`string`][1]         | IMOL                                                                                                ||
  || **email**               
  [`string`][1]         | E-mail                                                                                              ||
  || **phone**               
  [`string`][1]         | Phone                                                                                             ||
  || **utmSource**           
  [`string`][1]         | Advertising system. Yandex-Direct, Google-Adwords, and others                                           ||
  || **utmMedium**           
  [`string`][1]         | Type of traffic. Possible values:
  - CPC — ads
  - CPM — banners                                                       ||
  || **utmCampaign**         
  [`string`][1]         | Identifier of the advertising campaign                                                                     ||
  || **utmContent**          
  [`string`][1]         | Content of the campaign.
  
  For example, for contextual ads                                          ||
  || **utmTerm**             
  [`string`][1]         | Search condition of the campaign.
  
  For example, keywords for contextual advertising                              ||
  || **observers**           
  [`user[]`][1]         | List of user identifiers who are Observers                                ||
  || **contactIds**          
  [`crm_contact[]`][1]  | List of contact identifiers associated with the element                                            ||
  || **entityTypeId**        
  [`integer`][1]        | Identifier of the entity type                                                                         ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field. See section [{#T}](./user-defined-fields/index.md).

    - Values of multiple fields are returned as an array
    - Value of the `file` type field is returned as an object:
      - `id` — identifier
      - `url` — link to the file on the account
      - `urlMachine` — link to the file for the application

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#

- Deal

  #|
  || **Name**
  `type` | **Description** ||
  || **id**                  
  [`integer`][1]        | Identifier of the element                                                                              ||
  || **createdTime**         
  [`datetime`][1]       | Creation time of the element                                                                             ||
  || **dateCreateShort**     
  [`datetime`][1]       | Creation time of the element (short format).
  
  Field is disabled                                            ||
  || **updatedTime**         
  [`datetime`][1]       | Last modification time of the element                                                                 ||
  || **dateModifyShort**     
  [`datetime`][1]       | Last modification time of the element (short format).
  
  Field is disabled                                ||
  || **createdBy**           
  [`user`][1]           | Identifier of the user who created the element                                                  ||
  || **updatedBy**           
  [`user`][1]           | Identifier of the user who modified the element                                                 ||
  || **assignedById**        
  [`user`][1]           | Identifier of the user responsible for the element                                                ||
  || **opened**              
  [`boolean`][1]        | Is the element open?                                                                        ||
  || **leadId**              
  [`crm_lead`][1]       | Identifier of the lead on which the element is based                                           ||
  || **companyId**           
  [`crm_company`][1]    | Identifier of the company associated with the element                                                      ||
  || **contactId**           
  [`crm_contact`][1]    | Identifier of the contact associated with the element                                                     ||
  || **quoteId**             
  [`crm_quote`][1]      | Identifier of the estimate associated with the element                                                  ||
  || **title**               
  [`string`][1]         | Name of the element                                                                                   ||
  || **productId**           
  [`string`][1]         | Identifier of the product. 
  
  Deprecated. Field is disabled                                                      ||
  || **categoryId**          
  [`crm_category`][1]   | Identifier of the Sales Funnel (direction) of the element                                                        ||
  || **stageId**             
  [`crm_status`][1]     | String identifier of the element's stage                                                             ||
  || **stageSemanticId**     
  [`string`][1]         | Stage group

  - `P` — in progress
  - `S` — successful
  - `F` — unsuccessful
  ||
  || **isNew**               
  [`boolean`][1]        | Is the deal new?                                                                            ||
  || **isRecurring**         
  [`boolean`][1]        | Is the deal recurring?                                                                        ||
  || **isReturnCustomer**    
  [`boolean`][1]        | Is the element a repeat customer?                                                                       ||
  || **isRepeatedApproach**  
  [`boolean`][1]        | Is the deal a repeated approach?                                                             ||
  || **closed**              
  [`boolean`][1]        | Is the deal closed?                                                                         ||
  || **typeId**              
  [`crm_status`][1]     | String identifier of the deal type                                                                 ||
  || **opportunity**         
  [`double`][1]         | Amount                                                                                               ||
  || **isManualOpportunity** 
  [`boolean`][1]        | Is manual mode for calculating the amount set?                                                            ||
  || **taxValue**            
  [`double`][1]         | Tax amount                                                                                        ||
  || **currencyId**          
  [`crm_currency`][1]   | Identifier of the element's currency                                                                       ||
  || **probability**         
  [`integer`][1]        | Probability, %                                                                                      ||
  || **comments**            
  [`text`][1]           | Comment                                                                                         ||
  || **begindate**           
  [`date`][1]           | Start date of the element                                                                                ||
  || **begindateShort**      
  [`datetime`][1]       | Start time of the element (short format).
  
  Field is disabled                                              ||
  || **closedate**           
  [`date`][1]           | Completion date of the element                                                                            ||
  || **closedateShort**      
  [`datetime`][1]       | End time of the element (short format).
  
  Field is disabled                                           ||
  || **eventDate**           
  [`datetime`][1]       | Event date                                                                                        ||
  || **eventDateShort**      
  [`datetime`][1]       | Event date (short format).
  
  Field is disabled                                                       ||
  || **eventId**             
  [`crm_status`][1]     | String identifier of the event type                                                                ||
  || **eventDescription**    
  [`text`][1]           | Event description                                                                                    ||
  || **locationId**          
  [`location`][1]       | Identifier of the location. 
  
  Service field                                                        ||
  || **webformId**           
  [`integer`][1]        | Identifier of the CRM form                                                                             ||
  || **sourceId**            
  [`crm_status`][1]     | String identifier of the source type                                                              ||
  || **sourceDescription**   
  [`text`][1]           | Additional information about the source                                                                          ||
  || **originatorId**        
  [`string`][1]         | External source                                                                                    ||
  || **originId**            
  [`string`][1]         | Identifier of the element in the external source                                                         ||
  || **additionalInfo**      
  [`string`][1]         | Additional information                                                                           ||
  || **searchContent**       
  [`text`][1]           | Information for full-text search.
  
  Service field                                               ||
  || **orderStage**          
  [`string`][1]         | Payment status of the deal                                                                                ||
  || **movedBy**             
  [`user`][1]           | Identifier of the user who last changed the stage                                         ||
  || **movedTime**           
  [`datetime`][1]       | Time of the last stage change                                                                        ||
  || **lastActivityBy**      
  [`user`][1]           | Identifier of the user who last showed activity in the timeline                       ||
  || **lastActivityTime**    
  [`datetime`][1]       | Time of the last activity in the timeline                                                 ||
  || **isWork**               
  [`boolean`][1]        | Is the deal in progress?
  
  Field is disabled                                                         ||
  || **isWon**                
  [`boolean`][1]        | Is the deal won?
  
  Field is disabled                                                       ||
  || **isLose**               
  [`boolean`][1]        | Is the deal lost?
  
  Field is disabled                                                      ||
  || **receivedAmount**      
  [`string`][1]         | Received amount.
  
  Field is disabled                                                                    ||
  || **lostAmount**          
  [`string`][1]         | Lost amount.
  
  Field is disabled                                                                    ||
  || **hasProducts**         
  [`boolean`][1]        | Does the element contain products?
  
  Field is disabled                                                          ||
  || **utmSource**           
  [`string`][1]         | Advertising system. Yandex-Direct, Google-Adwords, and others                                           ||
  || **utmMedium**           
  [`string`][1]         | Type of traffic. Possible values:
  - CPC — ads
  - CPM — banners                                                       ||
  || **utmCampaign**         
  [`string`][1]         | Identifier of the advertising campaign                                                                     ||
  || **utmContent**          
  [`string`][1]         | Content of the campaign. For example, for contextual ads                                          ||
  || **utmTerm**             
  [`string`][1]         | Search condition of the campaign. For example, keywords for contextual advertising                              ||
  || **observers**           
  [`user[]`][1]         | List of user identifiers who are Observers                                ||
  || **contactIds**          
  [`crm_contact[]`][1]  | List of contact identifiers associated with the element                                            ||
  || **entityTypeId**        
  [`integer`][1]        | Identifier of the entity type                                                                         ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field. See section [{#T}](./user-defined-fields/index.md).

    - Values of multiple fields are returned as an array
    - Value of the `file` type field is returned as an object:
        - `id` — identifier
        - `url` — link to the file on the account
        - `urlMachine` — link to the file for the application

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field (An element of another type of CRM object that is linked to this element).

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#

- Contact

  #|
  || **Name**
  `type` | **Description** ||
  || **id**                  
  [`integer`][1]        | Identifier of the element                                                                              ||
  || **createdTime**         
  [`datetime`][1]       | Creation time of the element                                                                             ||
  || **updatedTime**         
  [`datetime`][1]       | Last modification time of the element                                                                 ||
  || **createdBy**           
  [`user`][1]           | Identifier of the user who created the element                                                  ||
  || **updatedBy**           
  [`user`][1]           | Identifier of the user who modified the element                                                 ||
  || **assignedById**        
  [`user`][1]           | Identifier of the user responsible for the element                                                ||
  || **opened**              
  [`boolean`][1]        | Is the element open?                                                                        ||
  || **companyId**           
  [`crm_company`][1]    | Identifier of the company associated with the element                                                      ||
  || **sourceId**            
  [`crm_status`][1]     | String identifier of the source type                                                              ||
  || **sourceDescription**   
  [`text`][1]           | Additional information about the source                                                                          ||
  || **name**                
  [`string`][1]         | First name                                                                                                 ||
  || **lastName**            
  [`string`][1]         | Last name                                                                                             ||
  || **secondName**          
  [`string`][1]         | Middle name                                                                                            ||
  || **shortName**           
  [`string`][1]         | Last name First name.
  
  Short format: for example 'Ivanov Ivan' -> 'Ivanov I.'.
  
  Field is disabled                 ||
  || **photo**               
  [`file`][1]           | Photo                                                                                          ||
  || **post**                
  [`string`][1]         | Position                                                                                           ||
  || **address**             
  [`text`][1]           | Address. 
  
  Deprecated. Field is disabled                                                                     ||
  || **comments**            
  [`text`][1]           | Comment                                                                                         ||
  || **leadId**              
  [`crm_lead`][1]       | Identifier of the lead on which the element is based                                           ||
  || **export**              
  [`boolean`][1]        | Is exporting the contact allowed?                                                                 ||
  || **typeId**              
  [`crm_status`][1]     | String identifier of the deal type                                                                 ||
  || **webformId**           
  [`integer`][1]        | Identifier of the CRM form                                                                             ||
  || **originatorId**        
  [`string`][1]         | External source                                                                                    ||
  || **originId**            
  [`string`][1]         | Identifier of the element in the external source                                                         ||
  || **originVersion**       
  [`string`][1]         | Version of the original                                                                                    ||
  || **birthdate**           
  [`date`][1]           | Date of birth                                                                                       ||
  || **honorific**           
  [`crm_status`][1]     | String identifier of the salutation type                                                              ||
  || **hasPhone**            
  [`boolean`][1]        | Does the element have a phone number?                                                                       ||
  || **hasEmail**            
  [`boolean`][1]        | Does the element have an email?                                                                         ||
  || **hasImol**             
  [`boolean`][1]        | Does the element have open lines?                                                                ||
  || **searchContent**       
  [`text`][1]           | Information for full-text search. Service field                                               ||
  || **categoryId**          
  [`crm_category`][1]   | Identifier of the Sales Funnel (direction) of the element                                                        ||
  || **lastActivityBy**      
  [`user`][1]           | Identifier of the user who last showed activity in the timeline                       ||
  || **lastActivityTime**    
  [`datetime`][1]       | Time of the last activity in the timeline                                                 ||
  || **login**               
  [`string`][1]         | Login.
  
  Deprecated. Field is disabled                                                                    ||
  || **emailHome**           
  [`string`][1]         | Personal e-mail                                                                                       ||
  || **emailWork**           
  [`string`][1]         | Work e-mail                                                                                      ||
  || **emailMailing**        
  [`string`][1]         | Mailing e-mail                                                                                  ||
  || **phoneMobile**         
  [`string`][1]         | Mobile phone                                                                                   ||
  || **phoneWork**           
  [`string`][1]         | Work phone                                                                                     ||
  || **phoneMailing**        
  [`string`][1]         | Mailing phone                                                                                ||
  || **imol**                
  [`string`][1]         | IMOL                                                                                                ||
  || **email**               
  [`string`][1]         | E-mail                                                                                              ||
  || **phone**               
  [`string`][1]         | Phone                                                                                             ||
  || **utmSource**           
  [`string`][1]         | Advertising system. Yandex-Direct, Google-Adwords, and others                                           ||
  || **utmMedium**           
  [`string`][1]         | Type of traffic. Possible values:
  - CPC — ads
  - CPM — banners                                                       ||
  || **utmCampaign**         
  [`string`][1]         | Identifier of the advertising campaign                                                                     ||
  || **utmContent**          
  [`string`][1]         | Content of the campaign. For example, for contextual ads                                          ||
  || **utmTerm**             
  [`string`][1]         | Search condition of the campaign. For example, keywords for contextual advertising                              ||
  || **observers**           
  [`user[]`][1]         | List of user identifiers who are Observers                                ||
  || **companyIds**          
  [`crm_company[]`][1]  | List of company identifiers associated with the element                                             ||
  || **entityTypeId**        
  [`integer`][1]        | Identifier of the entity type                                                                         ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field. See section [{#T}](./user-defined-fields/index.md).

    - Values of multiple fields are returned as an array
    *- Value of the `file` type field is returned as an object:
        - `id` — identifier
        - `url` — link to the file on the account
        - `urlMachine` — link to the file for the application

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#

- Company

  #|
  || **Name**
  `type` | **Description** ||
  || **id**                  
  [`integer`][1]        | Identifier of the element                                                                              ||
  || **createdTime**         
  [`datetime`][1]       | Creation time of the element                                                                             ||
  || **updatedTime**         
  [`datetime`][1]       | Last modification time of the element                                                                 ||
  || **createdBy**           
  [`user`][1]           | Identifier of the user who created the element                                                  ||
  || **updatedBy**           
  [`user`][1]           | Identifier of the user who modified the element                                                 ||
  || **assignedById**        
  [`user`][1]           | Identifier of the user responsible for the element                                                ||
  || **opened**              
  [`boolean`][1]        | Is the element open?                                                                        ||
  || **title**               
  [`string`][1]         | Name of the element                                                                                   ||
  || **logo**                
  [`file`][1]           | Logo                                                                                             ||
  || **address**             
  [`text`][1]           | Address. 
  
  Deprecated. Field is disabled                                                                     ||
  || **addressLegal**        
  [`text`][1]           | Legal address.
  
  Deprecated                                                                        ||
  || **bankingDetails**      
  [`string`][1]         | Banking details                                                                                ||
  || **comments**            
  [`text`][1]           | Comment                                                                                         ||
  || **typeId**              
  [`crm_status`][1]     | String identifier of the deal type                                                                 ||
  || **industry**            
  [`crm_status`][1]     | String identifier of the industry type                                                              ||
  || **revenue**             
  [`double`][1]         | Annual revenue                                                                                      ||
  || **currencyId**          
  [`crm_currency`][1]   | Identifier of the element's currency                                                                       ||
  || **employees**           
  [`crm_status`][1]     | String identifier of the number of employees                                                     ||
  || **leadId**              
  [`crm_lead`][1]       | Identifier of the lead on which the element is based                                           ||
  || **webformId**           
  [`integer`][1]        | Identifier of the CRM form                                                                             ||
  || **originatorId**        
  [`string`][1]         | External source                                                                                    ||
  || **originId**            
  [`string`][1]         | Identifier of the element in the external source                                                         ||
  || **originVersion**       
  [`string`][1]         | Version of the original                                                                                    ||
  || **hasPhone**            
  [`boolean`][1]        | Does the element have a phone number?                                                                       ||
  || **hasEmail**            
  [`boolean`][1]        | Does the element have an email?                                                                         ||
  || **hasImol**             
  [`boolean`][1]        | Does the element have open lines?                                                                ||
  || **isMyCompany**         
  [`boolean`][1]        | Is the company my company?                                                                 ||
  || **searchContent**       
  [`text`][1]           | Information for full-text search.
  
  Service field                                               ||
  || **categoryId**          
  [`crm_category`][1]   | Identifier of the Sales Funnel (direction) of the element                                                        ||
  || **lastActivityBy**      
  [`user`][1]           | Identifier of the user who last showed activity in the timeline                       ||
  || **lastActivityTime**    
  [`datetime`][1]       | Time of the last activity in the timeline                                                 ||
  || **emailHome**           
  [`string`][1]         | Personal e-mail                                                                                       ||
  || **emailWork**           
  [`string`][1]         | Work e-mail                                                                                      ||
  || **emailMailing**        
  [`string`][1]         | Mailing e-mail                                                                                  ||
  || **phoneMobile**         
  [`string`][1]         | Mobile phone                                                                                   ||
  || **phoneWork**           
  [`string`][1]         | Work phone                                                                                     ||
  || **phoneMailing**        
  [`string`][1]         | Mailing phone                                                                                ||
  || **imol**                
  [`string`][1]         | IMOL                                                                                                ||
  || **email**               
  [`string`][1]         | E-mail                                                                                              ||
  || **phone**               
  [`string`][1]         | Phone                                                                                             ||
  || **ufLogo**              
  [`file`][1]           | Logo (document generator)                                                                     ||
  || **ufStamp**             
  [`file`][1]           | Company seal (document generator)                                                           ||
  || **ufDirectorSign**      
  [`file`][1]           | Director's signature (document generator)                                                            ||
  || **ufAccountantSign**    
  [`file`][1]           | Chief accountant's signature (document generator)                                                       ||
  || **utmSource**           
  [`string`][1]         | Advertising system. Yandex-Direct, Google-Adwords, and others                                           ||
  || **utmMedium**           
  [`string`][1]         | Type of traffic. Possible values:
  - CPC — ads
  - CPM — banners                                                       ||
  || **utmCampaign**         
  [`string`][1]         | Identifier of the advertising campaign                                                                     ||
  || **utmContent**          
  [`string`][1]         | Content of the campaign. For example, for contextual ads                                          ||
  || **utmTerm**             
  [`string`][1]         | Search condition of the campaign. For example, keywords for contextual advertising                              ||
  || **observers**           
  [`user[]`][1]         | List of user identifiers who are Observers                                ||
  || **contactIds**          
  [`crm_contact[]`][1]  | List of contact identifiers associated with the element                                            ||
  || **entityTypeId**        
  [`integer`][1]        | Identifier of the entity type                                                                         ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field. See section [{#T}](./user-defined-fields/index.md)

    - Values of multiple fields are returned as an array
    - Value of the `file` type field is returned as an object:
        - `id` — identifier
        - `url` — link to the file on the account
        - `urlMachine` — link to the file for the application

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field (An element of another type of CRM object that is linked to this element).

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#

- Estimate

  #|
  || **Name**
  `type` | **Description** ||
  || **id**                  
  [`integer`][1]        | Identifier of the element                                                                              ||
  || **createdTime**         
  [`datetime`][1]       | Creation time of the element                                                                             ||
  || **dateCreateShort**     
  [`datetime`][1]       | Creation time of the element (short format).
  
  Field is disabled                                            ||
  || **updatedTime**         
  [`datetime`][1]       | Last modification time of the element                                                                 ||
  || **dateModifyShort**     
  [`datetime`][1]       | Last modification time of the element (short format).
  
  Field is disabled                                ||
  || **createdBy**           
  [`user`][1]           | Identifier of the user who created the element                                                  ||
  || **updatedBy**           
  [`user`][1]           | Identifier of the user who modified the element                                                 ||
  || **assignedById**        
  [`user`][1]           | Identifier of the user responsible for the element                                                ||
  || **opened**              
  [`boolean`][1]        | Is the element open?                                                                        ||
  || **leadId**              
  [`crm_lead`][1]       | Identifier of the lead on which the element is based                                           ||
  || **dealId**              
  [`crm_deal`][1]       | Identifier of the deal associated with the element                                                        ||
  || **companyId**           
  [`crm_company`][1]    | Identifier of the company associated with the element                                                      ||
  || **contactId**           
  [`crm_contact`][1]    | Identifier of the contact associated with the element                                                     ||
  || **personTypeId**        
  [`integer`][1]        | Identifier of the payer type                                                                      ||
  || **mycompanyId**         
  [`crm_company`][1]    | Identifier of "my" company                                                                       ||
  || **title**               
  [`string`][1]         | Name of the element                                                                                   ||
  || **stageId**             
  [`crm_status`][1]     | String identifier of the element's stage                                                             ||
  || **closed**              
  [`boolean`][1]        | Is the deal closed?                                                                         ||
  || **opportunity**         
  [`double`][1]         | Amount                                                                                               ||
  || **isManualOpportunity** 
  [`boolean`][1]        | Is manual mode for calculating the amount set?                                                            ||
  || **taxValue**            
  [`double`][1]         | Tax amount                                                                                        ||
  || **currencyId**          
  [`crm_currency`][1]   | Identifier of the element's currency                                                                       ||
  || **comments**            
  [`text`][1]           | Comment                                                                                         ||
  || **commentsType**        
  [`integer`][1]        | Identifier of the comment type.

  Possible values:
  - `0` — unknown
  - `1` — text
  - `2` — bb-code
  - `3` — HTML
  ||
  || **begindate**           
  [`date`][1]           | Start date of the element                                                                                ||
  || **begindateShort**      
  [`datetime`][1]       | Start time of the element (short format).
  
  Field is disabled                                              ||
  || **closedate**           
  [`date`][1]           | Completion date of the element                                                                            ||
  || **closedateShort**      
  [`datetime`][1]       | End time of the element (short format).
  
  Field is disabled                                           ||
  || **quoteNumber**         
  [`string`][1]         | Estimate number                                                                                   ||
  || **content**             
  [`text`][1]           | Content                                                                                          ||
  || **contentType**         
  [`integer`][1]        | Identifier of the content type.

  Possible values:
  - `0` — unknown
  - `1` — text
  - `2` — bb-code
  - `3` — HTML
  ||
  || **terms**                
  [`text`][1]           | Terms                                                                                             ||
  || **termsType**           
  [`integer`][1]        | Identifier of the terms type.

  Possible values:
  - `0` — unknown
  - `1` — text
  - `2` — bb-code
  - `3` — HTML
  ||
  || **storageTypeId**       
  [`integer`][1]        | Identifier of the storage type                                                                         ||
  || **storageElementIds**   
  [`integer[]`][1]      | Array of files                                                                                       ||
  || **locationId**          
  [`location`][1]       | Identifier of the location. Service field                                                        ||
  || **webformId**           
  [`integer`][1]        | Identifier of the CRM form                                                                             ||
  || **clientTitle**         
  [`string`][1]         | Client name                                                                                    ||
  || **clientAddr**          
  [`string`][1]         | Client address                                                                                       ||
  || **clientContact**       
  [`string`][1]         | Client contacts                                                                                    ||
  || **clientEmail**         
  [`string`][1]         | Client e-mail                                                                                      ||
  || **clientPhone**         
  [`string`][1]         | Client phone                                                                                     ||
  || **clientTpId**          
  [`string`][1]         | Client TIN                                                                                         ||
  || **clientTpaId**         
  [`string`][1]         | Client TPP                                                                                         ||
  || **searchContent**       
  [`text`][1]           | Information for full-text search. Service field                                               ||
  || **lastActivityBy**      
  [`user`][1]           | Identifier of the user who last showed activity in the timeline                       ||
  || **lastActivityTime**    
  [`datetime`][1]       | Time of the last activity in the timeline                                                 ||
  || **hasProducts**         
  [`boolean`][1]        | Does the element contain products.
  
  Field is disabled                                                          ||
  || **actualDate**          
  [`date`][1]           | Valid until                                                                                        ||
  || **utmSource**           
  [`string`][1]         | Advertising system. Yandex-Direct, Google-Adwords, and others                                           ||
  || **utmMedium**           
  [`string`][1]         | Type of traffic. Possible values:
  - CPC — ads
  - CPM — banners                                                       ||
  || **utmCampaign**         
  [`string`][1]         | Identifier of the advertising campaign                                                                     ||
  || **utmContent**          
  [`string`][1]         | Content of the campaign. For example, for contextual ads                                          ||
  || **utmTerm**             
  [`string`][1]         | Search condition of the campaign. For example, keywords for contextual advertising                             ||
  || **contactIds**          
  [`crm_contact[]`][1]  | List of contact identifiers associated with the element                                            ||
  || **entityTypeId**        
  [`integer`][1]        | Identifier of the entity type                                                                         ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field. See section [{#T}](./user-defined-fields/index.md).

    - Values of multiple fields are returned as an array
    - Value of the `file` type field is returned as an object:
        - `id` — identifier;
        - `url` — link to the file on the account;
        - `urlMachine` — link to the file for the application.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field (An element of another type of CRM object that is linked to this element).

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#

- Invoice

  #|
  || **Name**
  `type` | **Description** ||
  || **id**                  
  [`integer`][1]        | Identifier of the element                                                                              ||
  || **xmlId**               
  [`string`][1]         | External code                                                                                         ||
  || **title**               
  [`string`][1]         | Name of the element                                                                                   ||
  || **createdBy**           
  [`user`][1]           | Identifier of the user who created the element                                                  ||
  || **updatedBy**           
  [`user`][1]           | Identifier of the user who modified the element                                                 ||
  || **movedBy**             
  [`user`][1]           | Identifier of the user who last changed the stage                                         ||
  || **createdTime**         
  [`datetime`][1]       | Creation time of the element                                                                             ||
  || **updatedTime**         
  [`datetime`][1]       | Last modification time of the element                                                                 ||
  || **movedTime**           
  [`datetime`][1]       | Time of the last stage change                                                                        ||
  || **categoryId**          
  [`crm_category`][1]   | Identifier of the Sales Funnel (direction) of the element                                                        ||
  || **opened**              
  [`boolean`][1]        | Is the element open?                                                                        ||
  || **stageId**             
  [`crm_status`][1]     | String identifier of the element's stage                                                             ||
  || **previousStageId**     
  [`crm_status`][1]     | Identifier of the previous stage type                                                                ||
  || **begindate**           
  [`date`][1]           | Start date of the element                                                                                ||
  || **closedate**           
  [`date`][1]           | Completion date of the element                                                                            ||
  || **companyId**           
  [`crm_company`][1]    | Identifier of the company associated with the element                                                      ||
  || **contactId**           
  [`crm_contact`][1]    | Identifier of the contact associated with the element                                                     ||
  || **opportunity**         
  [`double`][1]         | Amount                                                                                               ||
  || **isManualOpportunity** 
  [`boolean`][1]        | Is manual mode for calculating the amount set?                                                            ||
  || **taxValue**            
  [`double`][1]         | Tax amount                                                                                        ||
  || **currencyId**          
  [`crm_currency`][1]   | Identifier of the element's currency                                                                       ||
  || **mycompanyId**         
  [`crm_company`][1]    | Identifier of "my" company                                                                       ||
  || **sourceId**            
  [`crm_status`][1]     | String identifier of the source type                                                              ||
  || **sourceDescription**   
  [`text`][1]           | Additional information about the source                                                                          ||
  || **webformId**           
  [`integer`][1]        | Identifier of the CRM form                                                                             ||
  || **assignedById**        
  [`user`][1]           | Identifier of the user responsible for the element                                                ||
  || **comments**            
  [`text`][1]           | Comment                                                                                         ||
  || **accountNumber**       
  [`string`][1]         | Invoice number                                                                                         ||
  || **locationId**          
  [`location`][1]       | Identifier of the location.
  
  Service field                                                        ||
  || **lastActivityBy**      
  [`user`][1]           | Identifier of the user who last showed activity in the timeline                       ||
  || **lastActivityTime**    
  [`datetime`][1]       | Time of the last activity in the timeline                                                 ||
  || **utmSource**           
  [`string`][1]         | Advertising system. Yandex-Direct, Google-Adwords, and others.
  
  Field is disabled                          ||
  || **utmMedium**           
  [`string`][1]         | Type of traffic. Possible values:
  - CPC — ads
  - CPM — banners
  
  Field is disabled                                      ||
  || **utmCampaign**         
  [`string`][1]         | Identifier of the advertising campaign.
  
  Field is disabled                                                    ||
  || **utmContent**          
  [`string`][1]         | Content of the campaign. For example, for contextual ads.
  
  Field is disabled                          ||
  || **utmTerm**             
  [`string`][1]         | Search condition of the campaign. For example, keywords for contextual advertising.
  
  Field is disabled              ||
  || **observers**           
  [`user[]`][1]         | List of user identifiers who are Observers                                ||
  || **contactIds**          
  [`crm_contact[]`][1]  | List of contact identifiers associated with the element                                            ||
  || **entityTypeId**        
  [`integer`][1]        | Identifier of the entity type                                                                         ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field. See section [{#T}](./user-defined-fields/index.md).

    - Values of multiple fields are returned as an array
    - Value of the `file` type field is returned as an object:
        - `id` — identifier;
        - `url` — link to the file on the account;
        - `urlMachine` — link to the file for the application.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field (An element of another type of CRM object that is linked to this element).

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#

- SPA

  #|
  || **Name**
  `type` | **Description** ||
  || **id**                  
  [`integer`][1]        | Identifier of the element                                                                              ||
  || **xmlId**               
  [`string`][1]         | External code                                                                                         ||
  || **title**               
  [`string`][1]         | Name of the element                                                                                   ||
  || **createdBy**           
  [`user`][1]           | Identifier of the user who created the element                                                  ||
  || **updatedBy**           
  [`user`][1]           | Identifier of the user who modified the element                                                 ||
  || **movedBy**             
  [`user`][1]           | Identifier of the user who last changed the stage.

  Available only when the `isStagesEnabled` setting is enabled for the corresponding SPA
  ||
  || **createdTime**         
  [`datetime`][1]       | Creation time of the element                                                                             ||
  || **updatedTime**         
  [`datetime`][1]       | Last modification time of the element                                                                 ||
  || **movedTime**           
  [`datetime`][1]       |  Time of the last stage change.

  Available only when the `isStagesEnabled` setting is enabled for the corresponding SPA
  ||
  || **categoryId**          
  [`crm_category`][1]   | Identifier of the Sales Funnel (direction) of the element                                                        ||
  || **opened**              
  [`boolean`][1]        | Is the element open?                                                                        ||
  || **stageId**             
  [`crm_status`][1]     |  String identifier of the element's stage.

  Available only when the `isStagesEnabled` setting is enabled for the corresponding SPA
  ||
  || **previousStageId**     
  [`crm_status`][1]     |  Identifier of the previous stage type.

  Available only when the `isStagesEnabled` setting is enabled for the corresponding SPA
  ||
  || **begindate**           
  [`date`][1]           |  Start date of the element.

  Available only when the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA
  ||
  || **closedate**           
  [`date`][1]           |  Completion date of the element.

  Available only when the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA
  ||
  || **companyId**           
  [`crm_company`][1]    |  Identifier of the company associated with the element.

  Available only when the `isClientEnabled` setting is enabled for the corresponding SPA
  ||
  || **contactId**           
  [`crm_contact`][1]    | Identifier of the contact associated with the element.

  Available only when the `isClientEnabled` setting is enabled for the corresponding SPA
  ||
  || **opportunity**         
  [`double`][1]         | Amount.

  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **isManualOpportunity** 
  [`boolean`][1]        | Is manual mode for calculating the amount set.

  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **taxValue**            
  [`double`][1]         | Tax amount.

  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **currencyId**          
  [`crm_currency`][1]   | Identifier of the element's currency.

  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **opportunityAccount**  
  [`double`][1]         | Amount in accounting currency. 
  
  Deprecated. Field is disabled.

  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **taxValueAccount**     
  [`double`][1]         | Tax amount in accounting currency. 
  
  Deprecated. Field is disabled.

  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **accountCurrencyId**   
  [`crm_currency`][1]   | Accounting currency.
  
  Field is disabled.

  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **mycompanyId**         
  [`crm_company`][1]    | Identifier of "my" company.

  Available only when the `isMycompanyEnabled` setting is enabled for the corresponding SPA
  ||
  || **sourceId**            
  [`crm_status`][1]     | String identifier of the source type.

  Available only when the `isSourceEnabled` setting is enabled for the corresponding SPA
  ||
  || **sourceDescription**   
  [`text`][1]           | Additional information about the source.

  Available only when the `isSourceEnabled` setting is enabled for the corresponding SPA
  ||
  || **webformId**           
  [`integer`][1]        | Identifier of the CRM form                                                                             ||
  || **assignedById**        
  [`user`][1]           | Identifier of the user responsible for the element                                                ||
  || **lastActivityBy**      
  [`user`][1]           | Identifier of the user who last showed activity in the timeline                       ||
  || **lastActivityTime**    
  [`datetime`][1]       | Time of the last activity in the timeline                                                 ||
  || **utmSource**           
  [`string`][1]         | Advertising system. Yandex-Direct, Google-Adwords, and others.
  
  Field is disabled                          ||
  || **utmMedium**           
  [`string`][1]         | Type of traffic. Possible values:
  - CPC — ads
  - CPM — banners
  
  Field is disabled                                       ||
  || **utmCampaign**         
  [`string`][1]         | Identifier of the advertising campaign.
  
  Field is disabled                                                    ||
  || **utmContent**          
  [`string`][1]         | Content of the campaign. For example, for contextual ads.
  
  Field is disabled                          ||
  || **mycompanyId**         
  [`crm_company`][1]    | Identifier of "my" company.

  Available only when the `isMycompanyEnabled` setting is enabled for the corresponding SPA
  ||
  || **contactIds**          
  [`crm_contact[]`][1]  | List of contact identifiers associated with the element.

  Available only when the `isClientEnabled` setting is enabled for the corresponding SPA
  ||
  || **entityTypeId**        
  [`integer`][1]        | Identifier of the entity type                                                                         ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field. See section [{#T}](./user-defined-fields/index.md).

    - Values of multiple fields are returned as an array
    - Value of the `file` type field is returned as an object:
        - `id` — identifier;
        - `url` — link to the file on the account;
        - `urlMachine` — link to the file for the application.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#

{% endlist %}

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "NOT_FOUND",
    "error_description": "SPA not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code**                           | **Description**                                                    | **Value**                                                                                     ||
|| `403`      | `allowed_only_intranet_user`      | Action is allowed only for intranet users                        | User is not an intranet user                                                                   ||
|| `400`      | `NOT_FOUND`                       | SPA not found                                                   | Occurs when an invalid `entityTypeId` is provided                                             ||
|| `400`      | `ACCESS_DENIED`                   | Access denied                                                   | User does not have permission to add elements of type `entityTypeId`                          ||
|| `400`      | `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid value for field "`field`"                               | Incorrect value provided for the `field`                                                      ||
|| `400`      | `100`                             | Expected iterable value for multiple field, but got `type` instead | A value of type `type` was provided for one of the multiple fields, while an iterable type was expected ||
|| `400`      | `CREATE_DYNAMIC_ITEM_RESTRICTED`  | You cannot create a new item due to your plan's restrictions    | Plan restrictions do not allow the creation of SPA items                                      ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}


## Continue Learning

- [{#T}](crm-item-update.md)
- [{#T}](crm-item-get.md)
- [{#T}](crm-item-list.md)
- [{#T}](crm-item-delete.md)
- [{#T}](crm-item-fields.md)

[1]: ../data-types.md
[2]: ../status/crm-status-list.md