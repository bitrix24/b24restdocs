# Import One Record crm.item.import

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to import CRM items

The `crm.item.import` method imports one item into the CRM.

Import uses separate permissions and does not trigger item creation automation. Learn more about import specifics in the [method overview](./index.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type`          | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the [system](../../data-types.md#object_type) or [custom CRM type](../user-defined-object-types/index.md) into which the item should be imported.

Numerical values for system types, such as lead — `1`, deal — `2`, contact — `3`, company — `4`, and invoice — `31`, are provided in the [CRM object types reference](../../data-types.md#object_type). You can obtain a SPA identifier using the [crm.type.list](../user-defined-object-types/crm-type-list.md) method. ||
|| **fields***
[`object`](../../../data-types.md) | Field values of the item being imported [(detailed description)](#fields) ||
|| **useOriginalUfNames**
[`boolean`](../../../data-types.md) | Parameter to control the format of custom field names in the request.
Possible values:

- `Y` — original names of custom fields, e.g., `UF_CRM_2_1639669411830`
- `N` — custom field names in camelCase, e.g., `ufCrm2_1639669411830`

Default is `N`. ||
|#

### Parameter fields {#fields}

The field set depends on the CRM object type. Pass field names as object keys and field values as their values:

```js
{
    field_1: value_1,
    field_2: value_2
}
```

An invalid field in `fields` will be ignored. You can get the current field set using the universal method [crm.item.fields](../crm-item-fields.md) or the methods [crm.lead.fields](../../leads/crm-lead-fields.md), [crm.deal.fields](../../deals/crm-deal-fields.md), [crm.contact.fields](../../contacts/crm-contact-fields.md), [crm.company.fields](../../companies/crm-company-fields.md), and [crm.quote.fields](../../quote/crm-quote-fields.md).

Pass multiple fields, such as `PHONE` and `EMAIL`, in the [crm_multifield](../../data-types.md#crm_multifield) format.

{% list tabs %}

- Lead

  CRM type identifier `entityTypeId`: `1`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../../data-types.md) | Name of the entity.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
    - `entityTypeName` — object type name
    - `id` — identifier of the element

  For example, for a lead with `id = 13` — `Lead #13`
  ||
  || **honorific**
  [`crm_status`](../../data-types.md) | String identifier of the lead salutation, for example, `HNR_RU_1` — "Mr."

  You can get the list of available salutations using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "HONORIFIC" }`.

  By default — `null` ||
  || **name**
  [`string`](../../../data-types.md) | First name.

  By default — `null` ||
  || **secondName**
  [`string`](../../../data-types.md) | Middle name.

  By default — `null` ||
  || **lastName**
  [`string`](../../../data-types.md) | Last name.

  By default — `null` ||
  || **birthdate**
  [`date`](../../../data-types.md) | Date of birth.

  By default — `null` ||
  || **companyTitle**
  [`string`](../../../data-types.md) | Company name.

  By default — `null` ||
  || **sourceId**
  [`crm_status`](../../data-types.md) | String identifier for the source.

  For example, `CALL` — "Call".

  You can get the list of available sources using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }`.

  By default — the first available source ||
  || **sourceDescription**
  [`text`](../../../data-types.md) | Additional information about the source.

  By default — `null` ||
  || **stageId**
  [`crm_status`](../../data-types.md) | String identifier for the stage of the element.

  For example, `NEW` — "Unprocessed".

  You can get the list of available stages using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "STATUS" }`

  By default — the first available stage ||
  || **statusDescription**
  [`text`](../../../data-types.md) | Additional information about the stage.

  By default — `null` ||
  || **post**
  [`string`](../../../data-types.md) | Position.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`](../../data-types.md) | Identifier for the currency of the element.

  By default — the default currency ||
  || **isManualOpportunity**
  [`boolean`](../../../data-types.md) | Calculation mode for the amount. Possible values:

    - `Y` — manual
    - `N` — automatic

  By default — `N` ||
  || **opportunity**
  [`double`](../../../data-types.md) | Amount.

  By default — `null` ||
  || **opened**
  [`boolean`](../../../data-types.md) | Is the element available to everyone? Possible values:

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings  ||
  || **comments**
  [`text`](../../../data-types.md) | Comment.

  By default — `null` ||
  || **assignedById**
  [`user`](../../../data-types.md) | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **companyId**
  [`crm_company`](../../data-types.md) | Identifier of the company linked to the element.

  You can retrieve the list of companies using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`](../../data-types.md) | Identifier of the contact linked to the element.

  You can retrieve the list of contacts using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`](../../data-types.md) | Array of identifiers of contacts linked to the item.

  You can retrieve the list of contacts using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 3`.

  By default — `null` ||
  || **originatorId**
  [`string`](../../../data-types.md) | External source.

  By default — `null` ||
  || **originId**
  [`string`](../../../data-types.md) | Identifier of the element in the external source.

  By default — `null` ||
  || **webformId**
  [`integer`](../../../data-types.md) | Identifier of the CRM Form.

  By default — `null` ||
  || **observers**
  [`user[]`](../../../data-types.md) | Array of identifiers of users who will observe the item.

  By default — `null` ||
  || **utmSource**
  [`string`](../../../data-types.md) | Advertising system. For example: Google-Adwords

  By default — `null` ||
  || **utmMedium**
  [`string`](../../../data-types.md) | Type of traffic. Possible values:

    - CPC — ads
    - CPM — banners

  By default — `null` ||
  || **utmCampaign**
  [`string`](../../../data-types.md) | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`](../../../data-types.md) | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`](../../../data-types.md) | Search term for the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`](../../data-types.md) | User-defined field.

  For information on user-defined fields, see the section [{#T}](../user-defined-fields/index.md)

  Values of multiple fields are passed as an array.

  To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.
  ||
  || **parentId...**
  [`crm_entity`](../../data-types.md) | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}` ||
  |#


- Deal

  CRM type identifier `entityTypeId`: `2`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../../data-types.md) | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
    - `entityTypeName` — object type name
    - `id` — identifier of the element
      For example, for a deal with `id = 13` — `Deal #13` ||
  || **typeId**
  [`crm_status`](../../data-types.md) | String identifier of the object type.

  For example, for a deal: `SALE` — "Sale"

  You can get the list of available object types using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "DEAL_TYPE" }`

  By default — the first available object type ||
  || **categoryId**
  [`integer`](../../../data-types.md) | Identifier of the deal [funnel](../category/index.md).

  By default — `0` (general funnel) ||
  || **stageId**
  [`crm_status`](../../data-types.md) | String identifier for the stage of the element.

  For example, `NEW` — "Unprocessed".

  You can get the list of available stages using [`crm.status.list`](../../status/crm-status-list.md) with the filter:

  - if the deal is in the general funnel — `{ ENTITY_ID: "DEAL_STAGE" }`
  - if the deal is in another funnel — `{ ENTITY_ID: "DEAL_STAGE_{categoryId}" }`, where `categoryId` is the identifier of the deal [funnel](../category/index.md)

  By default — the first available stage in the funnel ||
  || **isRecurring**
  [`boolean`](../../../data-types.md) | Is the deal recurring? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `N` ||
  || **probability**
  [`integer`](../../../data-types.md) | Probability of successfully closing the deal, as a percentage.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`](../../data-types.md) | Identifier for the currency of the element.

  By default — the default currency ||
  || **isManualOpportunity**
  [`boolean`](../../../data-types.md) | Calculation mode for the amount. Possible values:

  - `Y` — manual
  - `N` — automatic

  By default — `N` ||
  || **opportunity**
  [`double`](../../../data-types.md) | Amount.

  By default — `null` ||
  || **taxValue**
  [`double`](../../../data-types.md) | Tax amount.

  By default — `null` ||
  || **companyId**
  [`crm_company`](../../data-types.md) | Identifier of the company linked to the element.

  You can retrieve the list of companies using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`](../../data-types.md) | Identifier of the contact linked to the element.

  You can retrieve the list of contacts using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`](../../data-types.md) | Array of identifiers of contacts linked to the item.

  You can retrieve the list of contacts using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 3`.

  By default — `null` ||
  || **quoteId**
  [`crm_quote`](../../data-types.md) | Identifier of the estimate that will be linked to the deal ||
  || **begindate**
  [`date`](../../../data-types.md) | Start date of the element.

  By default — creation date ||
  || **closedate**
  [`date`](../../../data-types.md) | End date of the element.

  By default — seven days after item creation ||
  || **opened**
  [`boolean`](../../../data-types.md) | Is the element available to everyone? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings ||
  || **comments**
  [`text`](../../../data-types.md) | Comment.

  By default — `null` ||
  || **assignedById**
  [`user`](../../../data-types.md) | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **sourceId**
  [`crm_status`](../../data-types.md) | String identifier for the source.

  For example, `CALL` — "Call".

  You can retrieve the list of available sources using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }`.

  By default — the first available source ||
  || **sourceDescription**
  [`text`](../../../data-types.md) | Additional information about the source.

  By default — `null`||
  || **leadId**
  [`crm_lead`](../../data-types.md) | Identifier of the lead based on which the element is created.

  By default — `null`||
  || **additionalInfo**
  [`string`](../../../data-types.md) | Additional information.

  By default — `null` ||
  || **originatorId**
  [`string`](../../../data-types.md) | External source.

  By default — `null`||
  || **originId**
  [`string`](../../../data-types.md) | Identifier of the element in the external source.

  By default — `null`||
  || **observers**
  [`user[]`](../../../data-types.md) | Array of identifiers of users who will observe the item.

  By default — `null` ||
  || **locationId**
  [`location`](../../data-types.md) | Identifier of the location. Service field.

  By default — `null` ||
  || **utmSource**
  [`string`](../../../data-types.md) | Advertising system. For example: Google-Adwords

  By default — `null` ||
  || **utmMedium**
  [`string`](../../../data-types.md) | Type of traffic. Possible values:

  - CPC — ads
  - CPM — banners

  By default — `null` ||
  || **utmCampaign** [`string`](../../../data-types.md) | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`](../../../data-types.md) | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`](../../../data-types.md) | Search term for the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`](../../data-types.md) | User-defined field. See the section [{#T}](../user-defined-fields/index.md)

  - Values of multiple fields are passed as an array
  - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`](../../data-types.md) | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#


- Contact

  CRM type identifier `entityTypeId`: `3`

  #|
  || **Name**
  `type` | **Description** ||
  || **honorific**
  [`crm_status`](../../data-types.md) | String identifier for the contact's salutation.

  For example, `HNR_RU_1` — "Mr."

  You can get the list of available salutations using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "HONORIFIC" }`.

  By default — `null` ||
  || **name**
  [`string`](../../../data-types.md) | First name.

  By default — `null` ||
  || **secondName**
  [`string`](../../../data-types.md) | Middle name.

  By default — `null` ||
  || **lastName**
  [`string`](../../../data-types.md) | Last name.

  By default — `null` ||
  || **photo**
  [`file`](../../../data-types.md) | Photo.

  By default — `null` ||
  || **birthdate**
  [`date`](../../../data-types.md) | Date of birth.

  By default — `null` ||
  || **typeId**
  [`crm_status`](../../data-types.md) | String identifier of the object type.

  For example, for a deal: `SALE` — "Sale".

  You can retrieve the list of available object types using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "CONTACT_TYPE" }`.

  By default — the first available object type ||
  || **sourceId**
  [`crm_status`](../../data-types.md) | String identifier for the source.

  For example, `CALL` — "Call".

  You can retrieve the list of available sources using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }`.

  By default — the first available source ||
  || **sourceDescription**
  [`text`](../../../data-types.md) | Additional information about the source.

  By default — `null` ||
  || **post**
  [`string`](../../../data-types.md) | Position.

  By default — `null` ||
  || **comments**
  [`text`](../../../data-types.md) | Comment.

  By default — `null` ||
  || **opened**
  [`boolean`](../../../data-types.md) | Is the element available to everyone? Possible values:

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings  ||
  || **export**
  [`boolean`](../../../data-types.md) | Is the contact included in the export?

  By default — `Y` ||
  || **assignedById**
  [`user`](../../../data-types.md) | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **companyId**
  [`crm_company`](../../data-types.md) | Identifier of the company linked to the element.

  You can retrieve the list of companies using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 4`.

  By default — `null` ||
  || **companyIds**
  [`crm_company`](../../data-types.md)     | Array of identifiers of companies that will be linked to the element ||
  || **leadId**
  [`crm_lead`](../../data-types.md) | Identifier of the lead based on which the element is created.

  By default — `null` ||
  || **originatorId**
  [`string`](../../../data-types.md) | External source.

  By default — `null` ||
  || **originId**
  [`string`](../../../data-types.md) | Identifier of the element in the external source.

  By default — `null` ||
  || **originVersion**
  [`string`](../../../data-types.md)          | Version of the original.

  By default — `null` ||
  || **observers**
  [`user[]`](../../../data-types.md) | Array of identifiers of users who will observe the item.

  By default — `null` ||
  || **utmSource**
  [`string`](../../../data-types.md) | Advertising system. For example: Google-Adwords

  By default — `null` ||
  || **utmMedium**
  [`string`](../../../data-types.md) | Type of traffic. Possible values:

    - CPC — ads
    - CPM — banners

  By default — `null` ||
  || **utmCampaign**
  [`string`](../../../data-types.md) | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`](../../../data-types.md) | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`](../../../data-types.md) | Search term for the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`](../../data-types.md) | User-defined field. See the section [{#T}](../user-defined-fields/index.md)

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`](../../data-types.md) | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#


- Company

  CRM type identifier `entityTypeId`: `4`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../../data-types.md) | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where

    - `entityTypeName` — object type name
    - `id` — identifier of the element

  For example, for a company with `id = 13` — `Company #13` ||
  || **typeId**
  [`crm_status`](../../data-types.md) | String identifier of the object type.

  For example, for a deal: `SALE` — "Sale".

  You can retrieve the list of available object types using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "COMPANY_TYPE" }`.

  By default — the first available object type ||
  || **logo**
  [`file`](../../../data-types.md) | Logo.

  By default — `null` ||
  || **bankingDetails**
  [`string`](../../../data-types.md) | Banking details.

  By default — `null` ||
  || **industry**
  [`crm_status`](../../data-types.md) | String identifier for the type of industry.

  For example, `IT` — "Information Technology".

  You can retrieve the list of available industries using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "INDUSTRY" }`.

  By default — the first available industry ||
  || **employees**
  [`crm_status`](../../data-types.md) | String identifier for the number of employees.

  The value is selected from the available list, for example, `EMPLOYEES_1` — "less than 50".

  You can retrieve the list of available ranges using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "EMPLOYEES" }`.

  By default — the first available employee count range ||
  || **currencyId**
  [`crm_currency`](../../data-types.md) | Identifier for the currency of the element.

  By default — the default currency ||
  || **revenue**
  [`double`](../../../data-types.md) | Annual revenue.

  By default — `0` ||
  || **opened**
  [`boolean`](../../../data-types.md) | Is the element available to everyone? Possible values:

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings ||
  || **comments**
  [`text`](../../../data-types.md) | Comment.

  By default — `null` ||
  || **isMyCompany**
  [`boolean`](../../../data-types.md) | Is the company my company?

  By default — `N` ||
  || **assignedById**
  [`user`](../../../data-types.md) | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **contactIds**
  [`crm_contact[]`](../../data-types.md) | Array of identifiers of contacts linked to the item.

  You can retrieve the list of contacts using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 3`.

  By default — `null`||
  || **leadId**
  [`crm_lead`](../../data-types.md) | Identifier of the lead based on which the element is created.

  By default — `null`||
  || **originatorId**
  [`string`](../../../data-types.md) | External source.

  By default — `null` ||
  || **originId**
  [`string`](../../../data-types.md) | Identifier of the element in the external source.

  By default — `null` ||
  || **originVersion**
  [`string`](../../../data-types.md) | Version of the original.

  By default — `null` ||
  || **observers**
  [`user[]`](../../../data-types.md) | Array of identifiers of users who will observe the item.

  By default — `null` ||
  || **utmSource**
  [`string`](../../../data-types.md) | Advertising system. For example: Google-Adwords

  By default — `null` ||
  || **utmMedium**
  [`string`](../../../data-types.md) | Type of traffic. Possible values:
    - CPC — ads
    - CPM — banners

  By default — `null` ||
  || **utmCampaign**
  [`string`](../../../data-types.md) | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`](../../../data-types.md) | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`](../../../data-types.md) | Search term for the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`](../../data-types.md) | User-defined field. See the section [{#T}](../user-defined-fields/index.md)

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`](../../data-types.md) | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#


- Estimate

  CRM object identifier **entityTypeId:** `7`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../../data-types.md) | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
    - `entityTypeName` — object type name
    - `id` — identifier of the element

  For example, for an estimate with `id = 13` — `Estimate #13` ||
  || **assignedById**
  [`user`](../../../data-types.md) | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **opened**
  [`boolean`](../../../data-types.md) | Is the element available to everyone? Possible values:

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings ||
  || **content**
  [`text`](../../../data-types.md) | Content.

  By default — `null` ||
  || **terms**
  [`text`](../../../data-types.md) | Terms.

  By default — `null` ||
  || **comments**
  [`text`](../../../data-types.md) | Comment.

  By default — `null` ||
  || **dealId**
  [`crm_deal`](../../data-types.md)        | Identifier of the linked deal.

  By default — `null` ||
  || **leadId**
  [`crm_lead`](../../data-types.md) | Identifier of the lead based on which the element is created.

  By default — `null` ||
  || **storageTypeId**
  [`integer`](../../../data-types.md) | Identifier of the storage type. Possible values:
    - `1` — file
    - `2` — WebDAV
    - `3` — disk

  By default:
    1. If the `disk` module is enabled -> Disk
    2. If the `webdav` module is enabled -> WebDAV
    3. File
  ||
  || **storageElementIds**
  [`integer`](../../../data-types.md) | Array of files.

  By default — `null` ||
  || **webformId**
  [`integer`](../../../data-types.md) | Identifier of the CRM Form.

  By default — `null` ||
  || **companyId**
  [`crm_company`](../../data-types.md) | Identifier of the company linked to the element.

  You can retrieve the list of companies using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`](../../data-types.md) | Identifier of the contact linked to the element.

  You can retrieve the list of contacts using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 3`

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`](../../data-types.md) | Array of identifiers of contacts linked to the item.

  You can retrieve the list of contacts using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 3`.

  By default — `null` ||
  || **locationId**
  [`location`](../../data-types.md) | Identifier of the location. Service field.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`](../../data-types.md) | Identifier for the currency of the element.

  By default — the default currency ||
  || **isManualOpportunity**
  [`boolean`](../../../data-types.md) | Calculation mode for the amount.

    - `Y` — manual
    - `N` — automatic

  By default — `N` ||
  || **opportunity**
  [`double`](../../../data-types.md) | Amount.

  By default — `null` ||
  || **taxValue**
  [`double`](../../../data-types.md) | Tax amount.

  By default — `null` ||
  || **stageId**
  [`crm_status`](../../data-types.md) | String identifier for the stage of the element.

  For example, `DRAFT` — "New".

  You can retrieve the list of available stages using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "QUOTE_STATUS" }`.

  By default — the first available stage ||
  || **begindate**
  [`date`](../../../data-types.md) | Start date of the element.

  By default — creation date of the element ||
  || **closedate**
  [`date`](../../../data-types.md) | End date of the element.

  By default — seven days after item creation ||
  || **actualDate**
  [`date`](../../../data-types.md) | Valid until.

  By default — seven days after item creation ||
  || **mycompanyId**
  [`crm_company`](../../data-types.md) | Identifier of my company.

  By default — identifier of the first available "my" company ||
  || **utmSource**
  [`string`](../../../data-types.md) | Advertising system. For example: Google-Adwords

  By default — `null` ||
  || **utmMedium**
  [`string`](../../../data-types.md) | Type of traffic.

    - CPC — ads
    - CPM — banners

  By default — `null` ||
  || **utmCampaign**
  [`string`](../../../data-types.md) | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`](../../../data-types.md) | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`](../../../data-types.md) | Search term for the campaign. For example, keywords for contextual advertising.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`](../../data-types.md) | User-defined field. See the section [{#T}](../user-defined-fields/index.md).

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`](../../data-types.md) | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#


- Invoice

  CRM type identifier `entityTypeId`: `31`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../../data-types.md) | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where

    - `entityTypeName` — object type name
    - `id` — identifier of the element

  For example, for an invoice with `id = 13` — `Invoice #13`
  ||
  || **xmlId**
  [`string`](../../../data-types.md) | External code.

  By default — `null` ||
  || **assignedById**
  [`user`](../../../data-types.md) | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **opened**
  [`boolean`](../../../data-types.md) | Is the element available to everyone? Possible values:

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings ||
  || **webformId**
  [`integer`](../../../data-types.md) | Identifier of the CRM Form.

  By default — `null` ||
  || **begindate**
  [`date`](../../../data-types.md) | Start date of the element.

  By default — creation date ||
  || **closedate**
  [`date`](../../../data-types.md) | End date of the element.

  By default — seven days after item creation ||
  || **companyId**
  [`crm_company`](../../data-types.md) | Identifier of the company linked to the element.

  You can retrieve the list of companies using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`](../../data-types.md) | Identifier of the contact linked to the element.

  You can retrieve the list of contacts using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`](../../data-types.md) | Array of identifiers of contacts linked to the item.

  You can retrieve the list of contacts using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 3`.

  By default — `null` ||
  || **observers**
  [`user[]`](../../../data-types.md) | Array of identifiers of users who will observe the item.

  By default — `null` ||
  || **stageId**
  [`crm_status`](../../data-types.md) | String identifier for the stage of the element.

  For example, `DT31_13:N` — "New".

  You can retrieve the list of available stages using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SMART_INVOICE_STAGE_{categoryId}" }`, where
  `categoryId` is the identifier of the default invoice funnel. You can retrieve it using [`crm.category.list`](../category/crm-category-list.md) with `entityTypeId = 31`.

  By default — the first available stage ||
  || **sourceId**
  [`crm_status`](../../data-types.md) | String identifier for the source.

  For example, `CALL` — "Call".

  You can retrieve the list of available sources using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }`.

  By default — the first available source ||
  || **sourceDescription**
  [`text`](../../../data-types.md) | Additional information about the source.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`](../../data-types.md) | Identifier for the currency of the element.

  By default — the default currency ||
  || **isManualOpportunity**
  [`boolean`](../../../data-types.md) | Calculation mode for the amount. Possible values:

    - `Y` — manual
    - `N` — automatic

  By default — `N` ||
  || **opportunity**
  [`double`](../../../data-types.md) | Amount.

  By default — `null` ||
  || **taxValue**
  [`double`](../../../data-types.md) | Tax amount.

  By default — `null` ||
  || **mycompanyId**
  [`crm_company`](../../data-types.md) | Identifier of my company.

  By default — identifier of the first available "my" company ||
  || **comments**
  [`text`](../../../data-types.md) | Comment.

  By default — `null` ||
  || **locationId**
  [`location`](../../data-types.md) | Identifier of the location. Service field.

  By default — `null` ||
  || **ufCrm...**
  [`crm_userfield`](../../data-types.md) | User-defined field. See the section [{#T}](../user-defined-fields/index.md).

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`](../../data-types.md) | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#


- SPA

  CRM type identifier `entityTypeId`: can be retrieved using the [`crm.type.list`](../user-defined-object-types/crm-type-list.md) method or created using the [`crm.type.add`](../user-defined-object-types/crm-type-add.md) method.

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../../data-types.md) | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
    - `entityTypeName` — name of the SPA
    - `id` — identifier of the element

  For example, for the HR SPA item with `id = 13` — `HR #13` ||
  || **xmlId**
  [`string`](../../../data-types.md) | External code.

  By default — `null` ||
  || **assignedById**
  [`user`](../../../data-types.md) | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method  ||
  || **opened**
  [`boolean`](../../../data-types.md) | Is the element available to everyone?

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings  ||
  || **webformId**
  [`integer`](../../../data-types.md) | Identifier of the CRM Form.

  By default — `null` ||
  || **begindate**
  [`date`](../../../data-types.md) | Start date of the element.

  Available only if the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA.

  By default — creation date of the element  ||
  || **closedate**
  [`date`](../../../data-types.md) | End date of the element.

  Available only if the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA.

  By default — seven days after item creation ||
  || **companyId**
  [`crm_company`](../../data-types.md) | Identifier of the company linked to the element.

  You can retrieve the list of companies using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 4`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **contactId**
  [`crm_contact`](../../data-types.md) | Identifier of the contact linked to the element.

  You can retrieve the list of contacts using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 3`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`](../../data-types.md) | Array of identifiers of contacts linked to the item.

  You can retrieve the list of contacts using [`crm.item.list`](../crm-item-list.md) with `entityTypeId = 3`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **observers**
  [`user[]`](../../../data-types.md) | Array of identifiers of users who will observe the item.

  Available only if the `isObserversEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **categoryId**
  [`crm_category`](../../data-types.md) | Identifier of the funnel of the SPA element.

  You can retrieve the list of available funnels using [`crm.category.list`](../category/crm-category-list.md) with the corresponding `entityTypeId` ||
  || **stageId**
  [`crm_status`](../../data-types.md) | String identifier for the stage of the element.

  For example, `DT1220_30:NEW` — "Start".

  You can retrieve the list of available stages using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "DYNAMIC_{entityTypeId}_STAGE_{categoryId}" }`, where
    - `entityTypeId` — identifier of the SPA type
    - `categoryId` — identifier of the SPA item funnel

  [Learn more about funnels](../category/index.md).

  Available only if the `isStagesEnabled` setting is enabled for the corresponding SPA.

  By default — the first available stage in the funnel ||
  || **sourceId**
  [`crm_status`](../../data-types.md) | String identifier of the source, for example, `CALL` — "Call".

  You can retrieve the list of available sources using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }`.

  Available only if the `isSourceEnabled` setting is enabled for the corresponding SPA.

  By default — the first available source ||
  || **sourceDescription**
  [`text`](../../../data-types.md) | Additional information about the source.

  Available only if the `isSourceEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`](../../data-types.md) | Identifier for the currency of the element.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

  By default — the default currency ||
  || **isManualOpportunity**
  [`boolean`](../../../data-types.md) | Calculation mode for the amount. Possible values:

    - `Y` — manual
    - `N` — automatic

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

  By default — `N` ||
  || **opportunity**
  [`double`](../../../data-types.md) | Amount.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **taxValue**
  [`double`](../../../data-types.md) | Tax amount.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **mycompanyId**
  [`crm_company`](../../data-types.md) | Identifier of my company.

  Available only if the `isMycompanyEnabled` setting is enabled for the corresponding SPA.

  By default — identifier of the first available "my" company ||
  || **ufCrm...**
  [`crm_userfield`](../../data-types.md) | User-defined field. See the section [{#T}](../user-defined-fields/index.md).

    - Values of multiple fields are passed as an array
    - To upload a file, the value of the user-defined field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

  ||
  || **parentId...**
  [`crm_entity`](../../data-types.md) | Parent field. An element of another type of CRM object that is linked to this element.

  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  |#

  {% note info "" %}

  Learn more about managing SPA settings in [Smart Processes: Overview of Methods and Events](../user-defined-object-types/index.md)

  {% endnote %}

{% endlist %}

To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

1. How to Import a Deal

   {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"fields":{"title":"New deal","typeId":"SERVICE","isRecurring":"Y","opportunity":999.99,"currencyId":"EUR"}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.import
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"fields":{"title":"New deal","typeId":"SERVICE","isRecurring":"Y","opportunity":999.99,"currencyId":"EUR"},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.import
        ```

    - JS (TS)

        ```ts
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        const response = await $b24.actions.v2.call.make({
          method: 'crm.item.import',
          params: {
            entityTypeId: 2,
            fields: {
              title: 'New deal',
              typeId: 'SERVICE',
              isRecurring: 'Y',
              opportunity: 999.99,
              currencyId: 'EUR',
            },
          },
          requestId: Text.getUuidRfc4122()
        })

        if (!response.isSuccess) {
          console.error(response.getErrorMessages().join('; '))
        } else {
          console.info(response.getData()!.result)
        }
        ```

    - JS (UMD)

        ```html
        <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
        <script>
          async function importDeal() {
            const $b24 = await B24Js.initializeB24Frame()
            const response = await $b24.actions.v2.call.make({
              method: 'crm.item.import',
              params: {
                entityTypeId: 2,
                fields: {
                  title: 'New deal',
                  typeId: 'SERVICE',
                  isRecurring: 'Y',
                  opportunity: 999.99,
                  currencyId: 'EUR',
                },
              },
              requestId: B24Js.Text.getUuidRfc4122()
            })

            if (!response.isSuccess) {
              console.error(response.getErrorMessages().join('; '))
              return
            }
            console.info(response.getData().result)
          }

          document.addEventListener('DOMContentLoaded', importDeal)
        </script>
        ```

    - Python

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.crm.item.import_(
                entity_type_id=2,
                fields={
                    "title": "New deal",
                    "typeId": "SERVICE",
                    "isRecurring": "Y",
                    "opportunity": 999.99,
                    "currencyId": "EUR",
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

    - PHP

        ```php
        try {
            $response = $b24Service->core->call(
                'crm.item.import',
                [
                    'entityTypeId' => 2,
                    'fields' => [
                        'title' => 'New deal',
                        'typeId' => 'SERVICE',
                        'isRecurring' => 'Y',
                        'opportunity' => 999.99,
                        'currencyId' => 'EUR',
                    ],
                ]
            );

            $result = $response->getResponseData()->getResult();
            echo 'Success: ' . print_r($result->data(), true);
        } catch (Throwable $e) {
            error_log($e->getMessage());
            echo 'Error importing CRM item: ' . $e->getMessage();
        }
        ```

    - BX24.js

        ```js
        BX24.callMethod(
            'crm.item.import', 
            {
                entityTypeId: 2,
                fields: {
                    title: "New deal",
                    typeId: "SERVICE",
                    isRecurring: "Y",
                    opportunity: 999.99,
                    currencyId: "EUR",
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
            'crm.item.import',
            [
                'entityTypeId' => 2,
                'fields' => [
                    'title' => "New deal",
                    'typeId' => "SERVICE",
                    'isRecurring' => "Y",
                    'opportunity' => 999.99,
                    'currencyId' => "EUR",
                ],
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    - Go

        ```go
        // client and ctx are already created — see the "Go SDK" section
        res, err := client.Core().Call(ctx, "crm.item.import", b24.Params{
            "entityTypeId": 2,
            "fields": b24.Params{
                "title":       "New deal",
                "typeId":      "SERVICE",
                "isRecurring": "Y",
                "opportunity": 999.99,
                "currencyId":  "EUR",
            },
        })
        if err != nil {
            return fmt.Errorf("crm.item.import: %w", err)
        }

        raw, ok := b24.Unwrap(res.Result, "item")
        if !ok {
            return fmt.Errorf("item key is missing from the response")
        }
        fmt.Printf("%s\n", raw)
        ```

   {% endlist %}


2. How to Create an SPA Item with a Set of Custom Fields

    {% cut "Custom fields involved in the example" %}

    {% include [Set of Custom Fields](../../_include/user-fields-for-examples-cut.md) %}

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
                "ufCrm44_1721812760630": "String value for a custom String field",
                "ufCrm44_1721812814433": 81,
                "ufCrm44_1721812853419": "2024-08-21",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "300|EUR",
                "ufCrm44_1721812935209": "Y",
                "ufCrm44_1721812948498": 9999.9
            }
        }' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.import
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{
            "entityTypeId": 1302,
            "fields": {
                "ufCrm44_1721812760630": "String value for a custom String field",
                "ufCrm44_1721812814433": 81,
                "ufCrm44_1721812853419": "2024-08-21",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "300|EUR",
                "ufCrm44_1721812935209": "Y",
                "ufCrm44_1721812948498": 9999.9
            },
            "auth": "**put_access_token_here**"
        }' \
        https://**put_your_bitrix24_address**/rest/crm.item.import
        ```

    - JS (TS)

        ```ts
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'
        declare const $b24: B24Frame

        const response = await $b24.actions.v2.call.make({
          method: 'crm.item.import',
          params: {
            entityTypeId: 1302,
            fields: {
              ufCrm44_1721812760630: 'String value for a custom String field',
              ufCrm44_1721812814433: 81,
              ufCrm44_1721812853419: '2024-08-21',
              ufCrm44_1721812885588: ['example.com', 'second-example.com'],
              ufCrm44_1721812898903: ['green_pixel.png', 'iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=='],
              ufCrm44_1721812915476: '300|EUR',
              ufCrm44_1721812935209: 'Y',
              ufCrm44_1721812948498: 9999.9,
            },
          },
          requestId: Text.getUuidRfc4122()
        })
        console.info(response.getData()?.result)
        ```

    - JS (UMD)

        ```html
        <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
        <script>
          async function importSmartProcessItem() {
            const $b24 = await B24Js.initializeB24Frame()
            const response = await $b24.actions.v2.call.make({
              method: 'crm.item.import',
              params: {
                entityTypeId: 1302,
                fields: {
                  ufCrm44_1721812760630: 'String value for a custom String field',
                  ufCrm44_1721812814433: 81,
                  ufCrm44_1721812853419: '2024-08-21',
                  ufCrm44_1721812885588: ['example.com', 'second-example.com'],
                  ufCrm44_1721812898903: ['green_pixel.png', 'iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=='],
                  ufCrm44_1721812915476: '300|EUR',
                  ufCrm44_1721812935209: 'Y',
                  ufCrm44_1721812948498: 9999.9,
                },
              },
              requestId: B24Js.Text.getUuidRfc4122()
            })
            console.info(response.getData()?.result)
          }
          document.addEventListener('DOMContentLoaded', importSmartProcessItem)
        </script>
        ```

    - Python

        ```python
        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.crm.item.import_(
                entity_type_id=1302,
                fields={
                    "ufCrm44_1721812760630": "String value for a custom String field",
                    "ufCrm44_1721812814433": 81,
                    "ufCrm44_1721812853419": "2024-08-21",
                    "ufCrm44_1721812885588": [
                        "example.com",
                        "second-example.com",
                    ],
                    "ufCrm44_1721812898903": [
                        "green_pixel.png",
                        "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==",
                    ],
                    "ufCrm44_1721812915476": "300|EUR",
                    "ufCrm44_1721812935209": "Y",
                    "ufCrm44_1721812948498": 9999.9,
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

    - PHP

        ```php
        try {
            $response = $b24Service->core->call(
                'crm.item.import',
                [
                    'entityTypeId' => 1302,
                    'fields' => [
                        'ufCrm44_1721812760630' => 'String value for a custom String field',
                        'ufCrm44_1721812814433' => 81,
                        'ufCrm44_1721812853419' => '2024-08-21',
                        'ufCrm44_1721812885588' => ['example.com', 'second-example.com'],
                        'ufCrm44_1721812898903' => ['green_pixel.png', 'iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=='],
                        'ufCrm44_1721812915476' => '300|EUR',
                        'ufCrm44_1721812935209' => 'Y',
                        'ufCrm44_1721812948498' => 9999.9,
                    ],
                ]
            );
            echo 'Success: ' . print_r($response->getResponseData()->getResult()->data(), true);
        } catch (Throwable $e) {
            echo 'Error: ' . $e->getMessage();
        }
        ```

    - BX24.js

        ```js
        const greenPixelInBase64 = "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==";

        BX24.callMethod(
            'crm.item.import',
            {
                entityTypeId: 1302,
                fields: {
                    ufCrm44_1721812760630: "String value for a custom String field",
                    ufCrm44_1721812814433: 81,
                    ufCrm44_1721812853419: "2024-08-21",
                    ufCrm44_1721812885588: ["example.com", "second-example.com"],
                    ufCrm44_1721812898903: ["green_pixel.png", greenPixelInBase64],
                    ufCrm44_1721812915476: "300|EUR",
                    ufCrm44_1721812935209: "Y",
                    ufCrm44_1721812948498: 9999.9,
                },
            },
            result => result.error() ? console.error(result.error()) : console.info(result.data())
        );
        ```

    - PHP CRest

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.import',
            [
                'entityTypeId' => 1302,
                'fields' => [
                    'ufCrm44_1721812760630' => "String value for a custom String field",
                    'ufCrm44_1721812814433' => 81,
                    'ufCrm44_1721812853419' => '2024-08-21',
                    'ufCrm44_1721812885588' => [
                        "example.com",
                        "second-example.com",
                    ],
                    'ufCrm44_1721812898903' => [
                        "green_pixel.png",
                        "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==",
                    ],
                    'ufCrm44_1721812915476' => "300|EUR",
                    'ufCrm44_1721812935209' => "Y",
                    'ufCrm44_1721812948498' => 9999.9,
                ],
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    - Go

        ```go
        res, err := client.Core().Call(ctx, "crm.item.import", b24.Params{
            "entityTypeId": 1302,
            "fields": b24.Params{
                "ufCrm44_1721812760630": "String value for a custom String field",
                "ufCrm44_1721812814433": 81,
                "ufCrm44_1721812853419": "2024-08-21",
                "ufCrm44_1721812885588": []string{"example.com", "second-example.com"},
                "ufCrm44_1721812898903": []string{"green_pixel.png", "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="},
                "ufCrm44_1721812915476": "300|EUR",
                "ufCrm44_1721812935209": "Y",
                "ufCrm44_1721812948498": 9999.9,
            },
        })
        if err != nil {
            return fmt.Errorf("crm.item.import: %w", err)
        }
        fmt.Printf("%v\n", res.Result)
        ```

   {% endlist %}

## Response Handling

The method returns an `item` object containing the identifier of the created item.

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 4
        }
    },
    "time": {
        "start": 1722940215.145257,
        "finish": 1722940217.94124,
        "duration": 2.795983076095581,
        "processing": 2.4315829277038574,
        "date_start": "2024-08-06T10:30:15+00:00",
        "date_finish": "2024-08-06T10:30:17+00:00",
        "operating": 2.4314892292022705
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response. Contains an object with the import result [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **item**
[`object`](../../../data-types.md) | Import result [(detailed description)](#item) ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the created item ||
|#

## Error Handling

HTTP status: **400**, **401**, **403**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Smart process not found"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `NOT_FOUND` | SPA not found | An unknown `entityTypeId` was passed ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have permission to import items of type `entityTypeId` ||
|| `400` | `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid value for field `field` | An invalid value was passed for the `field` field.

For system fields, such as `createdTime`, the error also occurs if the request is made by a non-administrator ||
|| `400` | `100` | Expected iterable value for multiple field, but got `type` instead | A value of type `type` was passed to a multiple field, but an iterable value was expected. The error can also occur due to invalid JSON or request headers ||
|| `400` | `CREATE_DYNAMIC_ITEM_RESTRICTED` | You cannot create a new item due to your plan restrictions | Plan restrictions do not allow creating SPA items ||
|| `401` | `INVALID_CREDENTIALS` | Invalid authorization data for the request | Invalid user identifier or webhook code in the request URL ||
|| `403` | `allowed_only_intranet_user` | This action is allowed only for intranet users | The user is not an intranet user ||
|#

{% include [System errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-batch-import.md)
