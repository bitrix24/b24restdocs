### Parameter fields

{% include [Footnote about parameters](../../../_includes/required.md) %}

{% list tabs %}

- Lead

  CRM object identifier **entityTypeId:** `1`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Name of the entity.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
    - `entityTypeName` — name of the entity
    - `id` — identifier of the element

  For example, for a lead with `id = 13` — 'Lead #13'
  ||
  || **honorific**
  [`crm_status`][1] | String identifier for the lead's salutation (e.g., `'HNR_RU_1' = 'Mr.'`).

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
  [`crm_status`][1] | String identifier for the stage of the element.

  For example, `'NEW' = 'Unprocessed'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "STATUS" }`

  By default, it takes the value of the first available stage  ||
  || **statusDescription**
  [`text`][1] | Additional information about the stage.

  By default — `null` ||
  || **post**
  [`string`][1] | Position.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`][1] | Identifier for the currency of the element.

  By default, it takes the default currency  ||
  || **isManualOpportunity**
  [`boolean`][1] | Calculation mode for the amount. Possible values:

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

  By default — `Y`. The default value can be changed in the CRM settings  ||
  || **comments**
  [`text`][1] | Comment.

  By default — `null` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the element.

  By default, this is the identifier of the user who calls the method  ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 3`.

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
  [`user[]`][1] | Array of user identifiers who will be observers of the element.

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
  [`string`][1] | Search term for the campaign. For example, keywords for contextual advertising.

  By default equals `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field.

  For information on user-defined fields, see the section [{#T}](../universal/user-defined-fields/index.md)

  Values of multiple fields are passed as an array.

  To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.
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
    - `entityTypeName` — name of the entity
    - `id` — identifier of the element
      For example, for a deal with `id = 13` => 'Deal #13' ||
  || **typeId**
  [`crm_status`][1] | String identifier for the type of entity.

  For example, for a deal: `'SALE' = 'Sale'`

  The list of available entity types can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "DEAL_TYPE" }`

  By default — the first available entity type ||
  || **categoryId**
  [`integer`][1] | Identifier of the [direction](../universal/category/index.md) (funnel) of the deal.

  By default — `0` (general) ||
  || **stageId**
  [`crm_status`][1] | String identifier for the stage of the element.

  For example, `'NEW' = 'Unprocessed'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter:
  - If the deal is in the general funnel (direction) — `{ ENTITY_ID: "DEAL_STAGE" }`
  - If the deal is not in the general funnel (direction) — `{ ENTITY_ID: "DEAL_STAGE_{categoryId}" }`, where
    `categoryId` is the identifier of the funnel ([direction](../universal/category/index.md)) of the deal

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
  [`crm_currency`][1] | Identifier for the currency of the element.

  By default — the default currency ||
  || **isManualOpportunity**
  [`boolean`][1] | Calculation mode for the amount. Possible values:

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

  The list of companies can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 3`.

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

  By default — `Y`. The default value can be changed in the CRM settings ||
  || **comments**
  [`text`][1] | Comment.

  By default — `null` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
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
  [`user[]`][1] | Array of user identifiers who will be observers of the element.

  By default — `null` ||
  || **locationId**
  [`location`][1] | Identifier of the location. Service field.

  By default — `null` ||
  || **utmSource**
  [`string`][1] | Advertising system. Yandex-Direct, Google-Adwords, etc.

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
  [`string`][1] | Search term for the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field. See the section [{#T}](../universal/user-defined-fields/index.md)

  - Values of multiple fields are passed as an array
  - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

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
  [`crm_status`][1] | String identifier for the contact's salutation.

  For example, `'HNR_RU_1' = 'Mr.'`.

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
  [`file`][1] | Photo.

  By default — `null` ||
  || **birthdate**
  [`date`][1] | Date of birth.

  By default — `null` ||
  || **typeId**
  [`crm_status`][1] | String identifier for the type of entity.

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

  By default — `Y`. The default value can be changed in the CRM settings  ||
  || **export**
  [`boolean`][1] | Is the contact included in the export?

  By default — `Y` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 4`.

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
  [`user[]`][1] | Array of user identifiers who will be observers of the element.

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
  [`string`][1] | Search term for the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field. See the section [{#T}](../universal/user-defined-fields/index.md)

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

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

    - `entityTypeName` — name of the entity
    - `id` — identifier of the element

  For example, for a company with `id = 13` => 'Company #13' ||
  || **typeId**
  [`crm_status`][1] | String identifier for the type of entity.

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
  [`crm_status`][1] | String identifier for the type of industry.

  For example, `'IT' = 'Information Technology'`.

  The list of available industry types can be obtained using the [`crm.status.list`][2] method with the filter `{ ENTITY_ID: "INDUSTRY"}`.

  By default — the first available industry type ||
  || **employees**
  [`crm_status`][1] | String identifier for the number of employees.

  The value is taken from the available list, for example, `'EMPLOYEES_1' = 'less than 50'`.

  The list of available employee counts can be obtained using the [`crm.status.list`][2] method with the filter `{ ENTITY_ID: "EMPLOYEES" }`.

  By default — the first available employee count type ||
  || **currencyId**
  [`crm_currency`][1] | Identifier for the currency of the element.

  By default — the default currency ||
  || **revenue**
  [`double`][1] | Annual revenue.

  By default — `0` ||
  || **opened**
  [`boolean`][1] | Is the element available to everyone? Possible values:

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
  [`user`][1] | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 3`.

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
  [`user[]`][1] | Array of user identifiers who will be observers of the element.

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
  [`string`][1] | Search term for the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field. See the section [{#T}](../universal/user-defined-fields/index.md)

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

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
    - `entityTypeName` — name of the entity
    - `id` — identifier of the element

  For example, for an estimate with `id = 13` => 'Estimate #13' ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **opened**
  [`boolean`][1] | Is the element available to everyone? Possible values:

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

  The list of companies can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 3`

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **locationId**
  [`location`][1] | Identifier of the location. Service field.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`][1] | Identifier for the currency of the element.

  By default — the default currency ||
  || **isManualOpportunity**
  [`boolean`][1] | Calculation mode for the amount.

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
  [`crm_status`][1] | String identifier for the stage of the element.

  For example, `'DRAFT' = 'New'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "QUOTE_STATUS" }`.

  By default — the first available stage ||
  || **begindate**
  [`date`][1] | Start date of the element.

  By default — creation date of the element ||
  || **closedate**
  [`date`][1] | End date of the element.

  By default — creation date + 7 days ||
  || **actualDate**
  [`date`][1] | Valid until.

  By default — creation date + 7 days ||
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
  [`string`][1] | Search term for the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field. See the section [{#T}](../universal/user-defined-fields/index.md).

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

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

    - `entityTypeName` — name of the entity
    - `id` — identifier of the element

  For example, for an invoice with `id = 13` => 'Invoice #13'
  ||
  || **xmlId**
  [`string`][1] | External code.

  By default — `null` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **opened**
  [`boolean`][1] | Is the element available to everyone? Possible values:

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM Form.

  By default — `null` ||
  || **begindate**
  [`date`][1] | Start date of the element.

  By default — creation date ||
  || **closedate**
  [`date`][1] | End date of the element.

  By default — creation date + 7 days ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be observers of the element.

  By default — `null` ||
  || **stageId**
  [`crm_status`][1] | String identifier for the stage of the element.

  For example, `'DT31_13:N' = 'New'`.

  The list of available stages can be obtained using [`crm.status.list`][2], with the filter: `{ ENTITY_ID: "SMART_INVOICE_STAGE_{categoryId}" }`, where
  `categoryId` — identifier of the default invoice funnel. It can be obtained using [`crm.category.list`](../universal/category/crm-category-list.md) with `entityTypeId = 31`.

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
  [`crm_currency`][1] | Identifier for the currency of the element.

  By default — the default currency ||
  || **isManualOpportunity**
  [`boolean`][1] | Calculation mode for the amount. Possible values:

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
  [`location`][1] | Identifier of the location. Service field.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`][1] | User-defined field. See the section [{#T}](../universal/user-defined-fields/index.md).

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#


- SPA

  CRM object identifier **entityTypeId:** can be obtained using the [`crm.type.list`](../universal/user-defined-object-types/crm-type-list.md) method or created using the [`crm.type.add`](../universal/user-defined-object-types/crm-type-add.md) method.

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`][1] | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
    - `entityTypeName` — name of the SPA
    - `id` — identifier of the element

  For example, for the SPA element "HR" with `id = 13` => 'HR #13'  ||
  || **xmlId**
  [`string`][1] | External code.

  By default — `null` ||
  || **assignedById**
  [`user`][1] | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method  ||
  || **opened**
  [`boolean`][1] | Is the element available to everyone?

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings  ||
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

  By default — creation date + 7 days  ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 4`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 3`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`][1] | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../universal/crm-item-list.md) method with `entityTypeId = 3`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **observers**
  [`user[]`][1] | Array of user identifiers who will be observers of the element.

  Available only if the `isObserversEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **categoryId**
  [`crm_category`][1] | Identifier of the funnel of the SPA element.

  The list of available funnels can be obtained using [`crm.category.list`](../universal/category/crm-category-list.md) with the corresponding `entityTypeId` ||
  || **stageId**
  [`crm_status`][1] | String identifier for the stage of the element.

  For example, `'DT1220_30:NEW' = 'Start'`.

  The list of available stages can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "DYNAMIC_{entityTypeId}_STAGE_{categoryId}" }`, where
    - `entityTypeId` — identifier of the SPA type
    - `categoryId` — identifier of the funnel (direction) of the SPA element

  [Learn more about funnels (directions)](../universal/category/index.md).

  Available only if the `isStagesEnabled` setting is enabled for the corresponding SPA.

  By default — the first available stage relative to the funnel  ||
  || **sourceId**
  [`crm_status`][1] | String identifier for the source. (e.g., `'CALL' = 'Call'`).

  The list of available sources can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }`.

  Available only if the `isSourceEnabled` setting is enabled for the corresponding SPA.

  By default — the first available source  ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source.

  Available only if the `isSourceEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`][1] | Identifier for the currency of the element.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

  By default — the default currency  ||
  || **isManualOpportunity**
  [`boolean`][1] | Calculation mode for the amount. Possible values:

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
  [`crm_userfield`][1] | User-defined field. See the section [{#T}](../universal/user-defined-fields/index.md).

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#

  {% note info "SPA Settings" %}

  For more information on managing SPA settings, you can read in [{#T}](../universal/user-defined-object-types/index.md)

  {% endnote %}

{% endlist %}

[1]: ../data-types.md
[2]: ../status/crm-status-list.md