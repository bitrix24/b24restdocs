# Create a new CRM entity crm.item.add

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with the "add" access permission for the CRM object

This method is a universal way to create objects in CRM. With it, you can create various types of objects, such as deals, contacts, companies, and others.

To create an object, you need to pass the appropriate parameters, including the object type and its information: title, description, contact details, and other specifics.

After a successful request, a new object is created.

This method provides a flexible way to automate the process of creating objects and integrate CRM with other systems.

When creating an entity, a standard series of checks, modifications, and automatic actions are performed:

- access permissions are checked
- required fields are validated
- required fields dependent on stages are validated
- field values are checked for correctness
- default values are assigned to fields
- automation rules are triggered after saving

Next, we will look in detail at how to use this method and what parameters need to be passed.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

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

Each CRM entity type has its own set of fields. This means that the set of fields for creating a Lead does not have to match the set of fields for creating a Contact or SPA.

The list of available fields for each entity type is described [below](#parametr-fields).

An incorrect field in `fields` will be ignored
||
|| **useOriginalUfNames**
[`boolean`][1] | Parameter to control the format of custom field names in the request and response.   
Possible values:

- `Y` — original names of custom fields, for example `UF_CRM_2_1639669411830`
- `N` — custom field names in camelCase, for example `ufCrm2_1639669411830`

Default is `N` ||
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
  [`string`][1] | Title of the entity.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
  - `entityTypeName` — name of the entity
  - `id` — identifier of the element
  
  For example, for a lead with `id = 13` — 'Lead #13' 
  ||
  || **honorific**
  [`crm_status`][1] | String identifier of the lead's honorific (for example, `'HNR_RU_1' = 'Mr.'`).

  The list of available honorifics can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "HONOFIRIC" }`.

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
  [`crm_status`][1] | String identifier of the source.
  
  For example, `'CALL' = 'Call'`.
  
  The list of available sources can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }`.

  Default is the first available source  ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source.

  Default is `null` ||
  || **stageId**
  [`crm_status`][1] | String identifier of the entity's stage.
  
  For example, `'NEW' = 'Unprocessed'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "STATUS" }`

  Default is the first available stage  ||
  || **statusDescription**
  [`text`][1] | Additional information about the stage.

  Default is `null` ||
  || **post**
  [`string`][1] | Position.

  Default is `null` ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the entity's currency.

  Default is the default currency  ||
  || **isManualOpportunity**
  [`boolean`][1] | Mode of calculating the amount. Possible values:

  - `Y` — manual
  - `N` — automatic

  Default is `N` ||
  || **opportunity**
  [`double`][1] | Amount.

  Default is `null` ||
  || **opened**
  [`boolean`][1] | Is the entity available to everyone? Possible values:

  - `Y` — yes
  - `N` — no

  Default is `Y`. The default value can be changed in CRM settings  ||
  || **comments**
  [`text`][1] | Comment.

  Default is `null` ||
  || **assignedById**
  [`user`][1] | Identifier of the user responsible for the entity.

  Default is the identifier of the user calling the method  ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the entity.

  The list of companies can be obtained using the method [`crm.item.list`](crm-item-list.md) with `entityTypeId = 4`.

  Default is `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the entity.

  The list of contacts can be obtained using the method [`crm.item.list`](crm-item-list.md) with `entityTypeId = 3`.

  Default is `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the entity.

  The list of contacts can be obtained using the method [`crm.item.list`](crm-item-list.md) with `entityTypeId = 3`.

  Default is `null` ||
  || **originatorId**
  [`string`][1] | External source.

  Default is `null` ||
  || **originId**
  [`string`][1] | Identifier of the entity in the external source.

  Default is `null` ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM Form.

  Default is `null` ||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be Observers in the entity.

  Default is `null` ||
  || **utmSource**
  [`string`][1] | Advertising system. For example: Google Ads, Facebook Ads, and others.

  Default is `null` ||
  || **utmMedium**
  [`string`][1] | Type of traffic. Possible values:
  
  - CPC — ads
  - CPM — banners

  Default is `null` ||
  || **utmCampaign**
  [`string`][1] | Identifier of the advertising campaign.

  Default is `null` ||
  || **utmContent**
  [`string`][1] | Content of the campaign. For example, for contextual ads.

  Default is `null` ||
  || **utmTerm**
  [`string`][1] | Search term of the campaign. For example, keywords for contextual advertising.

  Default is `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. 
  
  Read about custom fields in the section [{#T}](./user-defined-fields/index.md) 
  
  Values of multiple fields are passed as an array.
  
  To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the content of the file encoded in [base64](../../files/how-to-upload-files.md).
  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  || **fm**
  [`multifield[]`][1] | Array of multifields.

  More about multifields can be read in the section [{#T}](../data-types.md#crm_multifield)

  Structure of a multifield:

    - `typeId` — Type of multifield
    - `valueType` — Type of value
    - `value` — Value

  Example:

    ```bash
    fm: [
      {
        "valueType": "WORK",
        "value": "+19999999999",
        "typeId": "PHONE"
      },
      {
        "valueType": "WORK",
        "value": "bitrix@bitrix.com",
        "typeId": "EMAIL"
      }
    ]
    ```
  Default is `null`
  ||
  |#

- Deal

  CRM object identifier **entityTypeId:** `2`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Title of the entity

  By default, it is generated using the template `{entityTypeName} #{id}`, where
  - `entityTypeName` — name of the entity
  - `id` — identifier of the entity
  For example, for a deal with `id = 13` => 'Deal #13' ||
  || **typeId**
  [`crm_status`][1] | String identifier of the entity type.

  For example, for a deal: `'SALE' = 'Sale'`

  The list of available entity types can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "DEAL_TYPE" }`

  By default — the first available entity type ||
  || **categoryId**
  [`integer`][1] | Identifier of the [direction](./category/index.md) (funnel) of the deal.

  By default — `0` (general) ||
  || **stageId**
  [`crm_status`][1] | String identifier of the entity stage. 
  
  For example, `'NEW' = 'Not Processed'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter:
    - If the deal is in the general funnel (direction) — `{ ENTITY_ID: "DEAL_STAGE" }`
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
  [`crm_currency`][1] | Identifier of the currency of the entity.

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
  [`crm_company`][1] | Identifier of the company linked to the entity.

  The list of companies can be obtained using the method [`crm.item.list`](crm-item-list.md) with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the entity.

  The list of contacts can be obtained using the method [`crm.item.list`](crm-item-list.md) with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the entity.

  The list of contacts can be obtained using the method [`crm.item.list`](crm-item-list.md) with `entityTypeId = 3`.

  By default — `null` ||
  || **quoteId**
  [`crm_quote`][1] | Identifier of the estimate that will be linked to the deal ||
  || **begindate**
  [`date`][1] | Start date of the entity.

  By default — creation date ||
  || **closedate**
  [`date`][1] | End date of the entity.

  By default — creation date + 7 days ||
  || **opened**
  [`boolean`][1] | Is the entity available to everyone? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings ||
  || **comments**
  [`text`][1] | Comment.

  By default — `null` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the entity.

  By default — identifier of the user calling the method ||
  || **sourceId**
  [`crm_status`][1] | String identifier of the source. 
  
  For example, `'CALL' = 'Call'`.
  
  The list of available sources can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }`.

  By default — the first available source ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source.

  By default — `null`||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the entity is created.

  By default — `null`||
  || **additionalInfo**
  [`string`][1] | Additional information.

  By default — `null` ||
  || **originatorId**
  [`string`][1] | External source.

  By default — `null`||
  || **originId**
  [`string`][1] | Identifier of the entity in the external source.

  By default — `null`||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be Observers in the entity.

  By default — `null` ||
  || **locationId**
  [`location`][1] | Identifier of the location. System field.

  By default — `null` ||
  || **utmSource**
  [`string`][1] | Advertising system. Google Ads, Google AdWords, and others.

  By default — `null` ||
  || **utmMedium**
  [`string`][1] | Type of traffic. Possible values:
  
  - CPC — ads
  - CPM — banners

  By default — `null` ||
  || **utmCampaign** [`string`][1] | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`][1] | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field. See section [{#T}](./user-defined-fields/index.md)

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the content of the file encoded in [base64](../../files/how-to-upload-files.md).
  
  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}` 
  ||
  |#

- Contact

  CRM object identifier **entityTypeId:** `3`

  #|
  || **Name**
  `type` | **Description** ||
  || **honorific**
  [`crm_status`][1] | String identifier of the contact's salutation. 
  
  For example, `'HNR_US_1' = 'Mr.'`.

  The list of available salutations can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "HONOFIRIC" }`.

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
  [`file`][1] | Photograph.

  By default — `null` ||
  || **birthdate**
  [`date`][1] | Date of birth.

  By default — `null` ||
  || **typeId**
  [`crm_status`][1] | String identifier of the entity type.
  
  For example, for a deal: `'SALE' = 'Sale'`.
  
  The list of available entity types can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "CONTACT_TYPE" }`.

  By default — the first available entity type  ||
  || **sourceId**
  [`crm_status`][1] | String identifier of the source.
  
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
  [`boolean`][1] | Is the entity available to everyone? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings  ||
  || **export**
  [`boolean`][1] | Is the contact included in the export?

  By default — `Y` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the entity.

  By default — identifier of the user calling the method ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the entity.

  The list of companies can be obtained using the method [`crm.item.list`](crm-item-list.md) with `entityTypeId = 4`.

  By default — `null` ||
  || **companyIds**
  [`crm_company`][1]     | Array of identifiers of companies that will be linked to the entity ||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the entity is created.

  By default — `null` ||
  || **originatorId**
  [`string`][1] | External source.

  By default — `null` ||
  || **originId**
  [`string`][1] | Identifier of the entity in the external source.

  By default — `null` ||
  || **originVersion**
  [`string`][1]          | Version of the original.

  By default — `null` ||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be Observers in the entity.

  By default — `null` ||
  || **utmSource**
  [`string`][1] | Advertising system. Google Ads, Google AdWords, and others.

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
  [`crm_userfield`][1] | User-defined field. See section [{#T}](./user-defined-fields/index.md)

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the content of the file encoded in [base64](../../files/how-to-upload-files.md).

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  || **fm**
  [`multifield[]`][1] | Array of multi-fields.

  More about multi-fields can be read in section [{#T}](../data-types.md#crm_multifield)

  Structure of a multi-field:

    - `typeId` — Type of the multi-field
    - `valueType` — Type of value
    - `value` — Value

  Example:

    ```bash
    fm: [
      {
        "valueType": "WORK",
        "value": "+19999999999",
        "typeId": "PHONE"
      },
      {
        "valueType": "WORK",
        "value": "bitrix@bitrix.com",
        "typeId": "EMAIL"
      }
    ]
    ```
  By default — `null`||
  |#

- Company

  CRM object identifier **entityTypeId:** `4`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Name of the entity.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
  
  - `entityTypeName` — name of the entity
  - `id` — identifier of the entity
  
  For example, for a company with `id = 13` => 'Company #13' ||
  || **typeId**
  [`crm_status`][1] | String identifier of the entity type.
  
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
  [`crm_status`][1] | String identifier of the industry type. 
  
  For example, `'IT' = 'Information Technology'`.
  
  The list of available industry types can be obtained using the method [`crm.status.list`][2] with the filter `{ ENTITY_ID: "INDUSTRY"}`.

  By default — the first available industry type ||
  || **employees**
  [`crm_status`][1] | String identifier of the number of employees type.
  
  The value is taken from the available list, for example, `'EMPLOYEES_1' = 'less than 50'`.

  The list of available employee counts can be obtained using the method [`crm.status.list`][2] with the filter `{ ENTITY_ID: "EMPLOYEES" }`.

  By default — the first available employee count type ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the entity's currency.

  By default — default currency ||
  || **revenue**
  [`double`][1] | Annual revenue.

  By default — `0` ||
  || **opened**
  [`boolean`][1] | Is the entity available to everyone? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings ||
  || **comments**
  [`text`][1] | Comment.

  By default — `null` ||
  || **isMyCompany**
  [`boolean`][1] | Is the company my company?

  By default — `N` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the entity.

  By default — identifier of the user calling the method ||
  || **contactIds**
  [`crm_contact[]`][1] | List of contact identifiers linked to the entity.

  The list of contacts can be obtained using the method [`crm.item.list`](crm-item-list.md) with `entityTypeId = 3`.

  By default — `null`||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the entity is created.

  By default — `null`||
  || **originatorId**
  [`string`][1] | External source.

  By default — `null` ||
  || **originId**
  [`string`][1] | Identifier of the entity in the external source.

  By default — `null` ||
  || **originVersion**
  [`string`][1] | Version of the original.

  By default — `null` ||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be Observers in the entity.

  By default — `null` ||
  || **utmSource**
  [`string`][1] | Advertising system. Google Ads, Facebook Ads, and others.

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
  [`crm_userfield`][1] | User-defined field. See section [{#T}](./user-defined-fields/index.md)

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the content of the file encoded in [base64](../../files/how-to-upload-files.md)

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  || **fm**
  [`multifield[]`][1] | Array of multifields.

  More about multifields can be read in section [{#T}](../data-types.md#crm_multifield)

  Structure of a multifield:

    - `typeId` — Type of multifield
    - `valueType` — Type of value
    - `value` — Value

  Example:

    ```bash
    fm: [
      {
        "valueType": "WORK",
        "value": "+19999999999",
        "typeId": "PHONE"
      },
      {
        "valueType": "WORK",
        "value": "bitrix@bitrix.com",
        "typeId": "EMAIL"
      }
    ]

    ```
  By default — `null`||
  |#

- Estimate

  CRM object identifier **entityTypeId:** `7`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Name of the entity.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
  - `entityTypeName` — name of the entity
  - `id` — identifier of the entity
  
  For example, for an estimate with `id = 13` => 'Estimate #13' ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the entity.

  By default — identifier of the user calling the method ||
  || **opened**
  [`boolean`][1] | Is the entity available to everyone? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings ||
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
  [`crm_lead`][1] | Identifier of the lead based on which the entity is created.

  By default — `null` ||
  || **storageTypeId**
  [`integer`][1] | Identifier of the storage type. Possible values:
  - `1` — file
  - `2` — WebDAV
  - `3` — Drive

  By default:
  1. If the `disk` module is installed -> Drive
  2. If the `webdav` module is installed -> WebDAV
  3. File 
  ||
  || **storageElementIds**
  [`integer`][1] | Array of files.

  By default — `null` ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM Form.

  By default — `null` ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the entity.

  The list of companies can be obtained using the method [`crm.item.list`](crm-item-list.md) with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the entity.

  The list of contacts can be obtained using the method [`crm.item.list`](crm-item-list.md) with `entityTypeId = 3`

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of contact identifiers linked to the entity.

  The list of contacts can be obtained using the method [`crm.item.list`](crm-item-list.md) with `entityTypeId = 3`.

  By default — `null` ||
  || **locationId**
  [`location`][1] | Identifier of the location. System field.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the entity's currency.

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
  [`crm_status`][1] | String identifier of the entity's stage. 
  
  For example, `'DRAFT' = 'New'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "QUOTE_STATUS" }`.

  By default — the first available stage ||
  || **begindate**
  [`date`][1] | Start date of the entity.

  By default — creation date of the entity ||
  || **closedate**
  [`date`][1] | End date of the entity.

  By default — creation date of the entity + 7 days ||
  || **actualDate**
  [`date`][1] | Valid until.

  By default — creation date of the entity + 7 days ||
  || **mycompanyId**
  [`crm_company`][1] | Identifier of my company.

  By default — identifier of the first available "my" company ||
  || **utmSource**
  [`string`][1] | Advertising system. Google Ads, Facebook Ads, and others.

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
  [`crm_userfield`][1] | User-defined field. See section [{#T}](./user-defined-fields/index.md).

  - Values of multiple fields are passed as an array
  - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the content of the file encoded in [base64](../../files/how-to-upload-files.md).

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#

- Invoice

  CRM object identifier **entityTypeId:** `31`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Name of the entity.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
  
  - `entityTypeName` — name of the entity
  - `id` — identifier of the entity
  
  For example, for an invoice with `id = 13` => 'Invoice #13'
  ||
  || **xmlId**
  [`string`][1] | External code.

  By default — `null` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the entity.

  By default — identifier of the user calling the method ||
  || **opened**
  [`boolean`][1] | Indicates whether the entity is accessible to everyone. Possible values:

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM Form.

  By default — `null` ||
  || **begindate**
  [`date`][1] | Start date of the entity.

  By default — creation date of the entity ||
  || **closedate**
  [`date`][1] | End date of the entity.

  By default — creation date of the entity + 7 days ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the entity.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the entity.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the entity.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be Observers in the entity.

  By default — `null` ||
  || **stageId**
  [`crm_status`][1] | String identifier of the stage of the entity. 
  
  For example, `'DT31_13:N' = 'New'`.

  The list of available stages can be found using [`crm.status.list`][2], applying the filter: `{ ENTITY_ID: "SMART_INVOICE_STAGE_{categoryId}" }`, where
  `categoryId` — identifier of the default invoice funnel. It can be found using [`crm.category.list`](category/crm-category-list.md) with `entityTypeId = 31`.

  By default — the first available stage ||
  || **sourceId**
  [`crm_status`][1] | String identifier of the source.
  
  For example, `'CALL' = 'Call'`.
  
  The list of available sources can be found using [`crm.status.list`][2] applying the filter `{ ENTITY_ID: "SOURCE" }`.

  By default — the first available source ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the currency of the entity.

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
  [`crm_userfield`][1] | Custom field. See section [{#T}](./user-defined-fields/index.md).

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the content of the file encoded in [base64](../../files/how-to-upload-files.md).

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#

- Smart Process

  CRM object identifier **entityTypeId:** can be obtained using the [`crm.type.list`](user-defined-object-types/crm-type-list.md) method or created using the [`crm.type.add`](user-defined-object-types/crm-type-add.md) method.

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Name of the entity.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
  - `entityTypeName` — name of the smart process
  - `id` — identifier of the entity
  
  For example, for the smart process element "HR" with `id = 13` => 'HR #13'  ||
  || **xmlId**
  [`string`][1] | External code.

  By default — `null` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the entity.

  By default — identifier of the user calling the method  ||
  || **opened**
  [`boolean`][1] | Indicates whether the entity is accessible to everyone.

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings  ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM Form.

  By default — `null` ||
  || **begindate**
  [`date`][1] | Start date of the entity.

  Available only when the `isBeginCloseDatesEnabled` setting is enabled for the corresponding smart process.

  By default — creation date of the entity  ||
  || **closedate**
  [`date`][1] | End date of the entity.

  Available only when the `isBeginCloseDatesEnabled` setting is enabled for the corresponding smart process.

  By default — creation date of the entity + 7 days  ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the entity.

  The list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 4`.

  Available only when the `isClientEnabled` setting is enabled for the corresponding smart process.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the entity.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  Available only when the `isClientEnabled` setting is enabled for the corresponding smart process.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the entity.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method with `entityTypeId = 3`.

  Available only when the `isClientEnabled` setting is enabled for the corresponding smart process.

  By default — `null` ||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be Observers in the entity.

  Available only when the `isObserversEnabled` setting is enabled for the corresponding smart process.

  By default — `null` ||
  || **categoryId**
  [`crm_category`][1] | Identifier of the funnel of the smart process entity.

  The list of available funnels can be found using the [`crm.category.list`](category/crm-category-list.md) applying the corresponding `entityTypeId` ||
  || **stageId**
  [`crm_status`][1] | String identifier of the stage of the entity. 
  
  For example, `'DT1220_30:NEW' = 'Start'`.

  The list of available stages can be found using [`crm.status.list`][2] applying the filter `{ ENTITY_ID: "DYNAMIC_{entityTypeId}_STAGE_{categoryId}" }`, where
  - `entityTypeId` — identifier of the smart process type
  - `categoryId` — identifier of the funnel (direction) of the smart process element

  [More about funnels (directions)](category/index.md).

  Available only when the `isStagesEnabled` setting is enabled for the corresponding smart process.

  By default — the first available stage relative to the funnel  ||
  || **sourceId**
  [`crm_status`][1] | String identifier of the source. (for example, `'CALL' = 'Call'`).
  
  The list of available sources can be found using [`crm.status.list`][2] applying the filter `{ ENTITY_ID: "SOURCE" }`.

  Available only when the `isSourceEnabled` setting is enabled for the corresponding smart process.

  By default — the first available source  ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source.

  Available only when the `isSourceEnabled` setting is enabled for the corresponding smart process.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the currency of the entity.

  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding smart process.

  By default — default currency  ||
  || **isManualOpportunity**
  [`boolean`][1] | Mode of calculating the amount. Possible values:

  - `Y` — manual
  - `N` — automatic

  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding smart process.

  By default — `N` ||
  || **opportunity**
  [`double`][1] | Amount.

  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding smart process.

  By default — `null` ||
  || **taxValue**
  [`double`][1] | Tax amount.

  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding smart process.

  By default — `null` ||
  || **mycompanyId**
  [`crm_company`][1] | Identifier of my company.

  Available only when the `isMycompanyEnabled` setting is enabled for the corresponding smart process.

  By default — Identifier of the first available "my" company ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See section [{#T}](./user-defined-fields/index.md).

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the content of the file encoded in [base64](../../files/how-to-upload-files.md).

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#

  {% note info "Smart Process Settings" %}

  You can read more about managing smart process settings in [{#T}](./user-defined-object-types/index.md)

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
        -d '{"entityTypeId":2,"fields":{"title":"New deal (specifically for the REST methods example)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":2}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.add
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"fields":{"title":"New deal (specifically for the REST methods example)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":2},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.add
        ```

    - BX24.js

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
                    parentId1220: 2,
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

    - PHP CRest

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
                    'parentId1220' => 2,
                ],
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

   - PHP

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

    - BX24.js

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

    - PHP CRest

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

## Response Processing

HTTP Status: **200**

{% note info %}

Disabled fields always return `null`

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
            "title": "New deal (specifically for the example of REST methods)",
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


{% note info " " %}

By default, names of user-defined fields are passed and returned in camelCase, for example `ufCrm2_1639669411830`.
When passing the parameter `useOriginalUfNames` with the value `Y`, user-defined fields will be returned with their original names, for example `UF_CRM_2_1639669411830`.

{% endnote %}

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | The root element of the response, contains a single key `item` ||
|| **item**
[`item`](./object-fields.md) | Information about the created item, [field descriptions](./object-fields.md) ||
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
|| `403`      | `allowed_only_intranet_user`      | Action allowed only for intranet users                   | User is not an intranet user                                                 ||
|| `400`      | `NOT_FOUND`                       | SPA not found                                            | Occurs when an invalid `entityTypeId` is passed                                              ||
|| `400`      | `ACCESS_DENIED`                   | Access denied                                                    | User does not have permission to add items of type `entityTypeId`                             ||
|| `400`      | `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid value for field "`field`"                                   | Incorrect value for field `field`                                                     ||
|| `400`      | `100`                             | Expected iterable value for multiple field, but got `type` instead | One of the multiple fields received a value of type `type`, while an iterable type was expected ||
|| `400`      | `CREATE_DYNAMIC_ITEM_RESTRICTED`  | You cannot create a new item due to your plan restrictions | Plan restrictions do not allow creating SPA items                              ||
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
  
  