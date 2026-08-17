# Import One Record crm.item.import

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with "import" access permission for the CRM object

A universal method for importing objects into the CRM.

You can read about the differences between the import logic and the standard addition of items in the article [{#T}](./index.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type`          | **Description** ||
|| **entityTypeId***
[`integer`](../../data-types.md) | Identifier of the [system](../../data-types.md#object_type) or [custom type](../user-defined-object-types/index.md) for which the entity needs to be created.

Numerical values for system types (Lead — 1, Deal — 2, Contact — 3, Company — 4, Invoice — 31, etc.) are provided in the [CRM object types reference](../../data-types.md#object_type). The identifier for the SPA can be obtained using the [crm.type.list](../user-defined-object-types/crm-type-list.md) method. ||
|| **fields***
[`object`](../../data-types.md)  | An object in the following format:

```js
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — the name of the field
- `value_n` — the value of the field

For multi-fields, such as `PHONE`, `EMAIL`, provide data in the [crm_multifield](../../data-types.md#crm_multifield) structure:

```js
{
    field_name: [
        {
            VALUE: "value_1",
            VALUE_TYPE: "type_1"
        },
        {
            VALUE: "value_2",
            VALUE_TYPE: "type_2"
        },
        ...
    ]
}
```

- `field_name` — the name of the field, for example, `PHONE`
- `VALUE` — the value of the field, for example, a phone number
- `VALUE_TYPE` — the type of value, for example, `WORK`

Each CRM object has its own set of fields. This means that the set of fields for creating a Lead does not have to match the set of fields for creating a Contact or SPA.

The list of available fields for each type of object is described [below](#parametr-fields).

An incorrect field in `fields` will be ignored.

You can also find out the set of fields using the universal method [crm.item.fields](../crm-item-fields.md) or methods for specific CRM objects:
- [crm.lead.fields](../../leads/crm-lead-fields.md)
- [crm.deal.fields](../../deals/crm-deal-fields.md)
- [crm.contact.fields](../../contacts/crm-contact-fields.md)
- [crm.company.fields](../../companies/crm-company-fields.md)
- [crm.quote.fields](../../quote/crm-quote-fields.md)
||
|| **useOriginalUfNames**
[`boolean`](../../data-types.md) | Parameter to control the format of custom field names in the request and response.   
Possible values:

- `Y` — original names of custom fields, e.g., `UF_CRM_2_1639669411830`
- `N` — custom field names in camelCase, e.g., `ufCrm2_1639669411830`

Default is `N`. ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

{% list tabs %}

- Lead

  CRM object identifier **entityTypeId:** `1`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../data-types.md) | Name of the entity.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
    - `entityTypeName` — name of the entity
    - `id` — identifier of the element

  For example, for a lead with `id = 13` — 'Lead #13'
  ||
  || **honorific**
  [`crm_status`](../../data-types.md) | String identifier for the lead's salutation (e.g., `'HNR_RU_1' = 'Mr.'`).

  The list of available salutations can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "HONOFIRIC" }`.

  By default — `null` ||
  || **name**
  [`string`](../../data-types.md) | First name.

  By default — `null` ||
  || **secondName**
  [`string`](../../data-types.md) | Middle name.

  By default — `null` ||
  || **lastName**
  [`string`](../../data-types.md) | Last name.

  By default — `null` ||
  || **birthdate**
  [`date`](../../data-types.md) | Date of birth.

  By default — `null` ||
  || **companyTitle**
  [`string`](../../data-types.md) | Company name.

  By default — `null` ||
  || **sourceId**
  [`crm_status`](../../data-types.md) | String identifier for the source.

  For example, `'CALL' = 'Call'`.

  The list of available sources can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }`.

  By default, it takes the value of the first available source  ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source.

  By default — `null` ||
  || **stageId**
  [`crm_status`](../../data-types.md) | String identifier for the stage of the element.

  For example, `'NEW' = 'Unprocessed'`.

  The list of available stages can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "STATUS" }`

  By default, it takes the value of the first available stage  ||
  || **statusDescription**
  [`text`](../../data-types.md) | Additional information about the stage.

  By default — `null` ||
  || **post**
  [`string`](../../data-types.md) | Position.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`](../../data-types.md) | Identifier for the currency of the element.

  By default, it takes the default currency  ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Calculation mode for the amount. Possible values:

    - `Y` — manual
    - `N` — automatic

  By default — `N` ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount.

  By default — `null` ||
  || **opened**
  [`boolean`](../../data-types.md) | Is the element available to everyone? Possible values:

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings  ||
  || **comments**
  [`text`](../../data-types.md) | Comment.

  By default — `null` ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the element.

  By default, this is the identifier of the user who calls the method  ||
  || **companyId**
  [`crm_company`](../../data-types.md) | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`](../../data-types.md) | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`](../../data-types.md) | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **originatorId**
  [`string`](../../data-types.md) | External source.

  By default — `null` ||
  || **originId**
  [`string`](../../data-types.md) | Identifier of the element in the external source.

  By default — `null` ||
  || **webformId**
  [`integer`](../../data-types.md) | Identifier of the CRM Form.

  By default — `null` ||
  || **observers**
  [`user[]`](../../data-types.md) | Array of user identifiers who will be observers of the element.

  By default — `null` ||
  || **utmSource**
  [`string`](../../data-types.md) | Advertising system. For example: Google-Adwords

  By default — `null` ||
  || **utmMedium**
  [`string`](../../data-types.md) | Type of traffic. Possible values:

    - CPC — ads
    - CPM — banners

  By default — `null` ||
  || **utmCampaign**
  [`string`](../../data-types.md) | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`](../../data-types.md) | Search term for the campaign. For example, keywords for contextual advertising.

  By default equals `null` ||
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

  CRM object identifier **entityTypeId:** `2`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../data-types.md) | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
    - `entityTypeName` — name of the entity
    - `id` — identifier of the element
      For example, for a deal with `id = 13` => 'Deal #13' ||
  || **typeId**
  [`crm_status`](../../data-types.md) | String identifier for the type of entity.

  For example, for a deal: `'SALE' = 'Sale'`

  The list of available entity types can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "DEAL_TYPE" }`

  By default — the first available entity type ||
  || **categoryId**
  [`integer`](../../data-types.md) | Identifier of the [direction](../category/index.md) (funnel) of the deal.

  By default — `0` (general) ||
  || **stageId**
  [`crm_status`](../../data-types.md) | String identifier for the stage of the element.

  For example, `'NEW' = 'Unprocessed'`.

  The list of available stages can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter:
  - If the deal is in the general funnel (direction) — `{ ENTITY_ID: "DEAL_STAGE" }`
  - If the deal is not in the general funnel (direction) — `{ ENTITY_ID: "DEAL_STAGE_{categoryId}" }`, where
    `categoryId` is the identifier of the funnel ([direction](../category/index.md)) of the deal

  By default — the first available stage relative to the funnel ||
  || **isRecurring**
  [`boolean`](../../data-types.md) | Is the deal recurring? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `N`||
  || **probability**
  [`integer`](../../data-types.md) | Probability %.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`](../../data-types.md) | Identifier for the currency of the element.

  By default — the default currency ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Calculation mode for the amount. Possible values:

  - `Y` — manual
  - `N` — automatic

  By default — `N` ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount.

  By default — `null` ||
  || **taxValue**
  [`double`](../../data-types.md) | Tax amount.

  By default — `null` ||
  || **companyId**
  [`crm_company`](../../data-types.md) | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`](../../data-types.md) | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`](../../data-types.md) | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **quoteId**
  [`crm_quote`](../../data-types.md) | Identifier of the estimate that will be linked to the deal ||
  || **begindate**
  [`date`](../../data-types.md) | Start date of the element.

  By default — creation date ||
  || **closedate**
  [`date`](../../data-types.md) | End date of the element.

  By default — creation date + 7 days ||
  || **opened**
  [`boolean`](../../data-types.md) | Is the element available to everyone? Possible values:

  - `Y` — yes
  - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings ||
  || **comments**
  [`text`](../../data-types.md) | Comment.

  By default — `null` ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **sourceId**
  [`crm_status`](../../data-types.md) | String identifier for the source.

  For example, `'CALL' = 'Call'`.

  The list of available sources can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }`.

  By default — the first available source ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source.

  By default — `null`||
  || **leadId**
  [`crm_lead`](../../data-types.md) | Identifier of the lead based on which the element is created.

  By default — `null`||
  || **additionalInfo**
  [`string`](../../data-types.md) | Additional information.

  By default — `null` ||
  || **originatorId**
  [`string`](../../data-types.md) | External source.

  By default — `null`||
  || **originId**
  [`string`](../../data-types.md) | Identifier of the element in the external source.

  By default — `null`||
  || **observers**
  [`user[]`](../../data-types.md) | Array of user identifiers who will be observers of the element.

  By default — `null` ||
  || **locationId**
  [`location`](../../data-types.md) | Identifier of the location. Service field.

  By default — `null` ||
  || **utmSource**
  [`string`](../../data-types.md) | Advertising system. For example: Google-Adwords

  By default — `null` ||
  || **utmMedium**
  [`string`](../../data-types.md) | Type of traffic. Possible values:

  - CPC — ads
  - CPM — banners

  By default — `null` ||
  || **utmCampaign** [`string`](../../data-types.md) | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`](../../data-types.md) | Search term for the campaign. For example, keywords for contextual advertising.

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

  CRM object identifier **entityTypeId:** `3`

  #|
  || **Name**
  `type` | **Description** ||
  || **honorific**
  [`crm_status`](../../data-types.md) | String identifier for the contact's salutation.

  For example, `'HNR_RU_1' = 'Mr.'`.

  The list of available salutations can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "HONOFIRIC" }`.

  By default — `null` ||
  || **name**
  [`string`](../../data-types.md) | First name.

  By default — `null` ||
  || **secondName**
  [`string`](../../data-types.md) | Middle name.

  By default — `null` ||
  || **lastName**
  [`string`](../../data-types.md) | Last name.

  By default — `null` ||
  || **photo**
  [`file`](../../data-types.md) | Photo.

  By default — `null` ||
  || **birthdate**
  [`date`](../../data-types.md) | Date of birth.

  By default — `null` ||
  || **typeId**
  [`crm_status`](../../data-types.md) | String identifier for the type of entity.

  For example, for a deal: `'SALE' = 'Sale'`.

  The list of available entity types can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "CONTACT_TYPE" }`.

  By default — the first available entity type  ||
  || **sourceId**
  [`crm_status`](../../data-types.md) | String identifier for the source.

  For example, `'CALL' = 'Call'`.

  The list of available sources can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }`.

  By default — the first available source  ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source.

  By default — `null` ||
  || **post**
  [`string`](../../data-types.md) | Position.

  By default — `null` ||
  || **comments**
  [`text`](../../data-types.md) | Comment.

  By default — `null` ||
  || **opened**
  [`boolean`](../../data-types.md) | Is the element available to everyone? Possible values:

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings  ||
  || **export**
  [`boolean`](../../data-types.md) | Is the contact included in the export?

  By default — `Y` ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **companyId**
  [`crm_company`](../../data-types.md) | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **companyIds**
  [`crm_company`](../../data-types.md)     | Array of identifiers of companies that will be linked to the element ||
  || **leadId**
  [`crm_lead`](../../data-types.md) | Identifier of the lead based on which the element is created.

  By default — `null` ||
  || **originatorId**
  [`string`](../../data-types.md) | External source.

  By default — `null` ||
  || **originId**
  [`string`](../../data-types.md) | Identifier of the element in the external source.

  By default — `null` ||
  || **originVersion**
  [`string`](../../data-types.md)          | Version of the original.

  By default — `null` ||
  || **observers**
  [`user[]`](../../data-types.md) | Array of user identifiers who will be observers of the element.

  By default — `null` ||
  || **utmSource**
  [`string`](../../data-types.md) | Advertising system. For example: Google-Adwords

  By default — `null` ||
  || **utmMedium**
  [`string`](../../data-types.md) | Type of traffic. Possible values:

    - CPC — ads
    - CPM — banners

  By default — `null` ||
  || **utmCampaign**
  [`string`](../../data-types.md) | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`](../../data-types.md) | Search term for the campaign. For example, keywords for contextual advertising.

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

  CRM object identifier **entityTypeId:** `4`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../data-types.md) | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where

    - `entityTypeName` — name of the entity
    - `id` — identifier of the element

  For example, for a company with `id = 13` => 'Company #13' ||
  || **typeId**
  [`crm_status`](../../data-types.md) | String identifier for the type of entity.

  For example, for a deal: `'SALE' = 'Sale'`.

  The list of available entity types can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "COMPANY_TYPE" }`.

  By default — the first available entity type ||
  || **logo**
  [`file`](../../data-types.md) | Logo.

  By default — `null` ||
  || **bankingDetails**
  [`string`](../../data-types.md) | Banking details.

  By default — `null` ||
  || **industry**
  [`crm_status`](../../data-types.md) | String identifier for the type of industry.

  For example, `'IT' = 'Information Technology'`.

  The list of available industry types can be obtained using the [`crm.status.list`](../../status/crm-status-list.md) method with the filter `{ ENTITY_ID: "INDUSTRY"}`.

  By default — the first available industry type ||
  || **employees**
  [`crm_status`](../../data-types.md) | String identifier for the number of employees.

  The value is taken from the available list, for example, `'EMPLOYEES_1' = 'less than 50'`.

  The list of available employee counts can be obtained using the [`crm.status.list`](../../status/crm-status-list.md) method with the filter `{ ENTITY_ID: "EMPLOYEES" }`.

  By default — the first available employee count type ||
  || **currencyId**
  [`crm_currency`](../../data-types.md) | Identifier for the currency of the element.

  By default — the default currency ||
  || **revenue**
  [`double`](../../data-types.md) | Annual revenue.

  By default — `0` ||
  || **opened**
  [`boolean`](../../data-types.md) | Is the element available to everyone? Possible values:

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings ||
  || **comments**
  [`text`](../../data-types.md) | Comment.

  By default — `null` ||
  || **isMyCompany**
  [`boolean`](../../data-types.md) | Is the company my company?

  By default — `N` ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **contactIds**
  [`crm_contact[]`](../../data-types.md) | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null`||
  || **leadId**
  [`crm_lead`](../../data-types.md) | Identifier of the lead based on which the element is created.

  By default — `null`||
  || **originatorId**
  [`string`](../../data-types.md) | External source.

  By default — `null` ||
  || **originId**
  [`string`](../../data-types.md) | Identifier of the element in the external source.

  By default — `null` ||
  || **originVersion**
  [`string`](../../data-types.md) | Version of the original.

  By default — `null` ||
  || **observers**
  [`user[]`](../../data-types.md) | Array of user identifiers who will be observers of the element.

  By default — `null` ||
  || **utmSource**
  [`string`](../../data-types.md) | Advertising system. For example: Google-Adwords

  By default — `null` ||
  || **utmMedium**
  [`string`](../../data-types.md) | Type of traffic. Possible values:
    - CPC — ads
    - CPM — banners

  By default — `null` ||
  || **utmCampaign**
  [`string`](../../data-types.md) | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`](../../data-types.md) | Search term for the campaign. For example, keywords for contextual advertising.

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
  [`string`](../../data-types.md) | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
    - `entityTypeName` — name of the entity
    - `id` — identifier of the element

  For example, for an estimate with `id = 13` => 'Estimate #13' ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **opened**
  [`boolean`](../../data-types.md) | Is the element available to everyone? Possible values:

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings ||
  || **content**
  [`text`](../../data-types.md) | Content.

  By default — `null` ||
  || **terms**
  [`text`](../../data-types.md) | Terms.

  By default — `null` ||
  || **comments**
  [`text`](../../data-types.md) | Comment.

  By default — `null` ||
  || **dealId**
  [`crm_deal`](../../data-types.md)        | Identifier of the linked deal.

  By default — `null` ||
  || **leadId**
  [`crm_lead`](../../data-types.md) | Identifier of the lead based on which the element is created.

  By default — `null` ||
  || **storageTypeId**
  [`integer`](../../data-types.md) | Identifier of the storage type. Possible values:
    - `1` — file
    - `2` — WebDAV
    - `3` — disk

  By default:
    1. If the `disk` module is enabled -> Disk
    2. If the `webdav` module is enabled -> WebDAV
    3. File
  ||
  || **storageElementIds**
  [`integer`](../../data-types.md) | Array of files.

  By default — `null` ||
  || **webformId**
  [`integer`](../../data-types.md) | Identifier of the CRM Form.

  By default — `null` ||
  || **companyId**
  [`crm_company`](../../data-types.md) | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`](../../data-types.md) | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 3`

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`](../../data-types.md) | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **locationId**
  [`location`](../../data-types.md) | Identifier of the location. Service field.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`](../../data-types.md) | Identifier for the currency of the element.

  By default — the default currency ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Calculation mode for the amount.

    - `Y` — manual
    - `N` — automatic

  By default — `N` ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount.

  By default — `null` ||
  || **taxValue**
  [`double`](../../data-types.md) | Tax amount.

  By default — `null` ||
  || **stageId**
  [`crm_status`](../../data-types.md) | String identifier for the stage of the element.

  For example, `'DRAFT' = 'New'`.

  The list of available stages can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "QUOTE_STATUS" }`.

  By default — the first available stage ||
  || **begindate**
  [`date`](../../data-types.md) | Start date of the element.

  By default — creation date of the element ||
  || **closedate**
  [`date`](../../data-types.md) | End date of the element.

  By default — creation date + 7 days ||
  || **actualDate**
  [`date`](../../data-types.md) | Valid until.

  By default — creation date + 7 days ||
  || **mycompanyId**
  [`crm_company`](../../data-types.md) | Identifier of my company.

  By default — identifier of the first available "my" company ||
  || **utmSource**
  [`string`](../../data-types.md) | Advertising system. For example: Google-Adwords

  By default — `null` ||
  || **utmMedium**
  [`string`](../../data-types.md) | Type of traffic.

    - CPC — ads
    - CPM — banners

  By default — `null` ||
  || **utmCampaign**
  [`string`](../../data-types.md) | Identifier of the advertising campaign.

  By default — `null` ||
  || **utmContent**
  [`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads.

  By default — `null` ||
  || **utmTerm**
  [`string`](../../data-types.md) | Search term for the campaign. For example, keywords for contextual advertising.

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

  CRM object identifier **entityTypeId:** `31`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../data-types.md) | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where

    - `entityTypeName` — name of the entity
    - `id` — identifier of the element

  For example, for an invoice with `id = 13` => 'Invoice #13'
  ||
  || **xmlId**
  [`string`](../../data-types.md) | External code.

  By default — `null` ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method ||
  || **opened**
  [`boolean`](../../data-types.md) | Is the element available to everyone? Possible values:

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings ||
  || **webformId**
  [`integer`](../../data-types.md) | Identifier of the CRM Form.

  By default — `null` ||
  || **begindate**
  [`date`](../../data-types.md) | Start date of the element.

  By default — creation date ||
  || **closedate**
  [`date`](../../data-types.md) | End date of the element.

  By default — creation date + 7 days ||
  || **companyId**
  [`crm_company`](../../data-types.md) | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 4`.

  By default — `null` ||
  || **contactId**
  [`crm_contact`](../../data-types.md) | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`](../../data-types.md) | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 3`.

  By default — `null` ||
  || **observers**
  [`user[]`](../../data-types.md) | Array of user identifiers who will be observers of the element.

  By default — `null` ||
  || **stageId**
  [`crm_status`](../../data-types.md) | String identifier for the stage of the element.

  For example, `'DT31_13:N' = 'New'`.

  The list of available stages can be obtained using [`crm.status.list`](../../status/crm-status-list.md), with the filter: `{ ENTITY_ID: "SMART_INVOICE_STAGE_{categoryId}" }`, where
  `categoryId` — identifier of the default invoice funnel. It can be obtained using [`crm.category.list`](../category/crm-category-list.md) with `entityTypeId = 31`.

  By default — the first available stage ||
  || **sourceId**
  [`crm_status`](../../data-types.md) | String identifier for the source.

  For example, `'CALL' = 'Call'`.

  The list of available sources can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }`.

  By default — the first available source ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`](../../data-types.md) | Identifier for the currency of the element.

  By default — the default currency ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Calculation mode for the amount. Possible values:

    - `Y` — manual
    - `N` — automatic

  By default — `N` ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount.

  By default — `null` ||
  || **taxValue**
  [`double`](../../data-types.md) | Tax amount.

  By default — `null` ||
  || **mycompanyId**
  [`crm_company`](../../data-types.md) | Identifier of my company.

  By default — identifier of the first available "my" company ||
  || **comments**
  [`text`](../../data-types.md) | Comment.

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

  CRM object identifier **entityTypeId:** can be obtained using the [`crm.type.list`](../user-defined-object-types/crm-type-list.md) method or created using the [`crm.type.add`](../user-defined-object-types/crm-type-add.md) method.

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../data-types.md) | Name of the element.

  By default, it is generated using the template `{entityTypeName} #{id}`, where
    - `entityTypeName` — name of the SPA
    - `id` — identifier of the element

  For example, for the SPA element "HR" with `id = 13` => 'HR #13'  ||
  || **xmlId**
  [`string`](../../data-types.md) | External code.

  By default — `null` ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the element.

  By default — the identifier of the user who calls the method  ||
  || **opened**
  [`boolean`](../../data-types.md) | Is the element available to everyone?

    - `Y` — yes
    - `N` — no

  By default — `Y`. The default value can be changed in the CRM settings  ||
  || **webformId**
  [`integer`](../../data-types.md) | Identifier of the CRM Form.

  By default — `null` ||
  || **begindate**
  [`date`](../../data-types.md) | Start date of the element.

  Available only if the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA.

  By default — creation date of the element  ||
  || **closedate**
  [`date`](../../data-types.md) | End date of the element.

  Available only if the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA.

  By default — creation date + 7 days  ||
  || **companyId**
  [`crm_company`](../../data-types.md) | Identifier of the company linked to the element.

  The list of companies can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 4`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **contactId**
  [`crm_contact`](../../data-types.md) | Identifier of the contact linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 3`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **contactIds**
  [`crm_contact[]`](../../data-types.md) | List of identifiers of contacts linked to the element.

  The list of contacts can be obtained using the [`crm.item.list`](../crm-item-list.md) method with `entityTypeId = 3`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **observers**
  [`user[]`](../../data-types.md) | Array of user identifiers who will be observers of the element.

  Available only if the `isObserversEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **categoryId**
  [`crm_category`](../../data-types.md) | Identifier of the funnel of the SPA element.

  The list of available funnels can be obtained using [`crm.category.list`](../category/crm-category-list.md) with the corresponding `entityTypeId` ||
  || **stageId**
  [`crm_status`](../../data-types.md) | String identifier for the stage of the element.

  For example, `'DT1220_30:NEW' = 'Start'`.

  The list of available stages can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "DYNAMIC_{entityTypeId}_STAGE_{categoryId}" }`, where
    - `entityTypeId` — identifier of the SPA type
    - `categoryId` — identifier of the funnel (direction) of the SPA element

  [Learn more about funnels (directions)](../category/index.md).

  Available only if the `isStagesEnabled` setting is enabled for the corresponding SPA.

  By default — the first available stage relative to the funnel  ||
  || **sourceId**
  [`crm_status`](../../data-types.md) | String identifier for the source. (e.g., `'CALL' = 'Call'`).

  The list of available sources can be obtained using [`crm.status.list`](../../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }`.

  Available only if the `isSourceEnabled` setting is enabled for the corresponding SPA.

  By default — the first available source  ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source.

  Available only if the `isSourceEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **currencyId**
  [`crm_currency`](../../data-types.md) | Identifier for the currency of the element.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

  By default — the default currency  ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Calculation mode for the amount. Possible values:

    - `Y` — manual
    - `N` — automatic

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

  By default — `N` ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **taxValue**
  [`double`](../../data-types.md) | Tax amount.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA.

  By default — `null` ||
  || **mycompanyId**
  [`crm_company`](../../data-types.md) | Identifier of my company.

  Available only if the `isMycompanyEnabled` setting is enabled for the corresponding SPA.

  By default — Identifier of the first available "my" company ||
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

  {% note info "SPA Settings" %}

  For more information on managing SPA settings, you can read in [{#T}](../user-defined-object-types/index.md)

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
        -d '{"entityTypeId":2,"fields":{"title":"New deal (specifically for REST method examples)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be an additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":2}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.import
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"fields":{"title":"New deal (specifically for REST method examples)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be an additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":2},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.import
        ```

    - Python

        Example

        ```python
        from b24pysdk.client import BaseClient
        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        client: BaseClient

        try:
            bitrix_response = client.crm.item.import_(
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
                    "sourceDescription": "There should be an additional description about the source",
                    "leadId": 102,
                    "additionalInfo": "There should be additional information",
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
            'crm.item.import', 
            {
                entityTypeId: 2,
                fields: 
                {
                    title: "New deal (specifically for REST method examples)",
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
                    sourceDescription: "There should be an additional description about the source",
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
            'crm.item.import',
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
                    'sourceDescription' => "There should be an additional description about the source",
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
                "ufCrm44_1721812760630": "String for a string-type custom field",
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
                "ufCrm44_1721812760630": "String for a string-type custom field",
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
        https://**put_your_bitrix24_address**/rest/crm.item.import
        ```

    - BX24.js

        ```js
        const greenPixelInBase64 = "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==";

        BX24.callMethod(
            'crm.item.import', 
            {
                entityTypeId: 1302,
                fields: {
                    ufCrm44_1721812760630: "String for a string-type custom field",
                    ufCrm44_1721812814433: 81,
                    ufCrm44_1721812853419: (new Date()).toISOString().slice(0, 10),
                    ufCrm44_1721812885588: [
                        "example.com",
                        "second-example.com",
                    ],
                    ufCrm44_1721812898903: [
                        "green_pixel.png",
                        greenpixelBase64,
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
            'crm.item.import',
            [
                'entityTypeId' => 1302,
                'fields' => [
                    'ufCrm44_1721812760630' => "String for a string-type custom field",
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

The method will return an `item` array with the identifier of the created item in case of success, or an error message.

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
[`object`](../../data-types.md) | The root element of the response. 

Contains a single key — `item` ||
|| **item**
[`object`](../../data-types.md) | Information about the created element. 

Contains a single key — `id` ||
|| **id**
[`int`](../../data-types.md) | Identifier of the created entity ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

{% note info " " %}

By default, custom field names are passed and returned in camelCase, for example `ufCrm2_1639669411830`.
When passing the parameter `useOriginalUfNames` with the value `Y`, custom fields will be returned with their original names, for example `UF_CRM_2_1639669411830`.

{% endnote %}

## Error Handling

HTTP status: **401**, **400**, **403**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Smart process not found"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code**                           | **Description**                                                       | **Value**                                                                                    ||
|| `400`      | `NOT_FOUND`                       | SPA not found                                            | Occurs when an invalid `entityTypeId` is passed                                              ||
|| `400`      | `ACCESS_DENIED`                   | Access denied                                                    | User does not have permission to add items of type `entityTypeId`                             ||
|| `400`      | `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid value for field "`field`"                                   | An incorrect value for the field `field` was provided.

For system fields of type `createdTime`, if the request is not made by an administrator ||
|| `400`      | `100`                             | Expected iterable value for multiple field, but got `type` instead | One of the multiple fields received a value of type `type`, while an iterable type was expected. This can also occur with an incorrect request (invalid JSON or request headers). ||
|| `400`      | `CREATE_DYNAMIC_ITEM_RESTRICTED`  | You cannot create a new item due to your plan restrictions | Plan restrictions do not allow creating SPA items                              ||
|| `401`      | `INVALID_CREDENTIALS`             | Invalid authorization data for the request                            | Incorrect `ID` and/or code in the request path.                                       ||
|| `403`      | `allowed_only_intranet_user`      | This action is allowed only for intranet users                   | User is not an intranet user                                                 ||
|#

{% include [System errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-batch-import.md)
