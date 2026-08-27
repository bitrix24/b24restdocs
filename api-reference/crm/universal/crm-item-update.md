# Update Item crm.item.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "change" permissions for the CRM object items

Updates an item of a specific CRM object type by assigning it new values from the `fields` parameter.

When updating an item, a standard series of checks, modifications, and automatic actions are performed:
- access permissions are checked
- mandatory field completion is checked if the item stage was changed within the same pipeline
- stage-dependent mandatory field completion is checked if the item stage was changed within the same pipeline
- field data validity is checked
- default values are assigned to fields
- if it is determined before saving that no field values have changed, the save **is not performed**
- automation rules are triggered after saving

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../data-types.md) | Identifier of the [system](../data-types.md#object_type) or [custom type](./user-defined-object-types/index.md) whose item we want to modify.

Numerical values for system types (Lead — 1, Deal — 2, Contact — 3, Company — 4, Invoice — 31, etc.) are listed in the [CRM object types reference](../data-types.md#object_type). The identifier of the smart process can be obtained using the [crm.type.list](./user-defined-object-types/crm-type-list.md) method ||
|| **id^*^**
[`integer`](../../data-types.md) | Identifier of the item we want to modify.

Can be obtained using the [`crm.item.list`](crm-item-list.md) or [`crm.item.add`](crm-item-add.md) methods ||
|| **fields***
[`object`](../../data-types.md) | Object in the format
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

Each CRM entity type has its own set of fields. This means that the set of fields for modifying a Lead does not have to match the set of fields for modifying a Contact or Smart Process.

The list of available fields for each entity type is described [below](#parameter-fields).

An incorrect field in `fields` will be ignored.

{% note info %}

Only those fields that need to be changed should be passed in `fields`.

{% endnote %}

||
|| **useOriginalUfNames**
[`boolean`](../../data-types.md) | Parameter to control the format of custom field names in the request and response.   
Possible values:

- `Y` — original names of custom fields, e.g., `UF_CRM_2_1639669411830`
- `N` — custom field names in camelCase, e.g., `ufCrm2_1639669411830`

Default is `N` ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

{% list tabs %}

- Lead

  CRM object identifier **entityTypeId:** `1`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../data-types.md) | Name of the item
  ||
  || **honorific**
  [`crm_status`](../data-types.md) | String identifier of the lead request (e.g., `'HNR_DE_1' = 'Mr.'`).

  A list of available requests can be obtained using [`crm.status.list`](../status/crm-status-list.md) by applying the `{ ENTITY_ID: "HONOFIRIC" }` filter ||
  || **name**
  [`string`](../../data-types.md) | First Name ||
  || **secondName**
  [`string`](../../data-types.md) | Middle Name ||
  || **lastName**
  [`string`](../../data-types.md) | Last Name ||
  || **birthdate**
  [`date`](../../data-types.md) | Date of birth ||
  || **companyTitle**
  [`string`](../../data-types.md) | Company name ||
  || **sourceId**
  [`crm_status`](../data-types.md) | String identifier of the source.
  
  For example, `'CALL' = 'Call'`.
  
  A list of available sources can be obtained using [`crm.status.list`](../status/crm-status-list.md) by applying the `{ ENTITY_ID: "SOURCE" }` filter  ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source ||
  || **stageId**
  [`crm_status`](../data-types.md) | String identifier of the item stage.
  
  For example, `'NEW' = 'Unprocessed'`.

  A list of available stages can be obtained using [`crm.status.list`](../status/crm-status-list.md) by applying the `{ ENTITY_ID: "STATUS" }` filter ||
  || **statusDescription**
  [`text`](../../data-types.md) | Additional information about the stage ||
  || **post**
  [`string`](../../data-types.md) | Job Title ||
  || **currencyId**
  [`crm_currency`](../data-types.md) | Item currency identifier  ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Amount calculation mode. Possible values:

  - `Y` — manual
  - `N` — automatic
  ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount ||
  || **opened**
  [`boolean`](../../data-types.md) | Whether the item is available to everyone. Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **comments**
  [`text`](../../data-types.md) | Comment ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the item  ||
  || **companyId**
  [`crm_company`](../data-types.md) | Company identifier linked to the item.

  A list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 4` ||
  || **contactId**
  [`crm_contact`](../data-types.md) | Contact identifier linked to the item.

  A list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 3` ||
  || **contactIds**
  [`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item.

  A list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 3` ||
  || **originatorId**
  [`string`](../../data-types.md) | External source ||
  || **originId**
  [`string`](../../data-types.md) | Identifier of the element in the external source ||
  || **webformId**
  [`integer`](../../data-types.md) | CRM Form identifier ||
  || **observers**
  [`user[]`](../../data-types.md) | Array of user identifiers who will be Observers in the item ||
  || **utmSource**
  [`string`](../../data-types.md) | Ad system. For example: Search Ads, Display Ads, etc. ||
  || **utmMedium**
  [`string`](../../data-types.md) | Traffic type. Possible values:
  
  - CPC — ads
  - CPM — banners ||
  || **utmCampaign**
  [`string`](../../data-types.md) | Advertising campaign designation ||
  || **utmContent**
  [`string`](../../data-types.md) | Campaign content. For example, for contextual ads ||
  || **utmTerm**
  [`string`](../../data-types.md) | Campaign search term. For example, keywords for contextual advertising ||
  || **ufCrm...**
  [`crm_userfield`](../data-types.md) | Custom field.
  
  Read the [{#T}](./user-defined-fields/index.md) section for information about custom fields.
  
  Values for multiple fields are passed as an array.
  
  To upload a file, you must pass an array as the custom field value, where the first item is the filename and the second is the file content encoded in [base64](../../files/how-to-upload-files.md)
  ||
  || **parentId...**
  [`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

  Each such field has a `parentId + {parentEntityTypeId}` code
  ||
  || **fm**
  [`crm_multifield[]`](../data-types.md#crm_multifield) | Array of multipools.

  You can read more about multipools in the [{#T}](../data-types.md#crm_multifield) section.

  Multipool structure:

  - `id` — Unique multipool identifier. If no multipool exists with this id, a new multipool will be created.
  - `typeId` — Multipool type
  - `valueType` — Value type
  - `value` — Value

  Example:

  ```bash
    fm: {
        "15": {
            "valueType": "WORK",
            "value": "+499999999",
            "typeId": "PHONE"
        },
        "16": {
            "valueType": "WORK",
            "value": "bitrix@bitrix.com",
            "typeId": "EMAIL"
        }
    }
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
  [`string`](../../data-types.md) | Name of the item ||
  || **typeId**
  [`crm_status`](../data-types.md) | String identifier of the entity type.

  For example, for a deal: `'SALE' = 'Sale'`

  A list of available entity types can be obtained using [`crm.status.list`](../status/crm-status-list.md) by applying the `{ ENTITY_ID: "DEAL_TYPE" }` filter ||
  || **categoryId**
  [`integer`](../../data-types.md) | Identifier of the deal [direction](./category/index.md) (pipeline) ||
  || **stageId**
  [`crm_status`](../data-types.md) | String identifier of the item stage.
  
  For example, `'NEW' = 'Unprocessed'`.

  A list of available stages can be obtained using [`crm.status.list`](../status/crm-status-list.md) by applying the following filter:
    - If the deal is in the general pipeline (direction) — `{ ENTITY_ID: "DEAL_STAGE" }`
    - If the deal is not in the general pipeline (direction) — `{ ENTITY_ID: "DEAL_STAGE_{categoryId}" }`, where
      `categoryId` is the identifier of the deal pipeline ([direction](./category/index.md))
  ||
  || **isRecurring**
  [`boolean`](../../data-types.md) | Whether the deal is recurring. Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **probability**
  [`integer`](../../data-types.md) | Probability % ||
  || **currencyId**
  [`crm_currency`](../data-types.md) | Item currency identifier ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Amount calculation mode. Possible values:

  - `Y` — manual
  - `N` — automatic
  ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount ||
  || **taxValue**
  [`double`](../../data-types.md) | Tax amount ||
  || **companyId**
  [`crm_company`](../data-types.md) | Company identifier linked to the item.

  A list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 4` ||
  || **contactId**
  [`crm_contact`](../data-types.md) | Contact identifier linked to the item.

  A list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 3` ||
  || **contactIds**
  [`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item.

  A list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 3` ||
  || **quoteId**
  [`crm_quote`](../data-types.md) | Offer identifier that will be linked to the deal ||
  || **begindate**
  [`date`](../../data-types.md) | Item start date ||
  || **closedate**
  [`date`](../../data-types.md) | Item end date ||
  || **opened**
  [`boolean`](../../data-types.md) | Whether the item is available to everyone. Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **comments**
  [`text`](../../data-types.md) | Comment ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the item ||
  || **sourceId**
  [`crm_status`](../data-types.md) | String identifier of the source.
  
  For example, `'CALL' = 'Call'`.
  
  A list of available sources can be obtained using [`crm.status.list`](../status/crm-status-list.md) by applying the `{ ENTITY_ID: "SOURCE" }` filter ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source ||
  || **leadId**
  [`crm_lead`](../data-types.md) | Lead identifier on the basis of which the item is created ||
  || **additionalInfo**
  [`string`](../../data-types.md) | Additional information ||
  || **originatorId**
  [`string`](../../data-types.md) | External source ||
  || **originId**
  [`string`](../../data-types.md) | Identifier of the element in the external source ||
  || **observers**
  [`user[]`](../../data-types.md) | Array of user identifiers who will be Observers in the item ||
  || **locationId**
  [`location`](../data-types.md) | Location identifier. Service field  ||
  || **utmSource**
  [`string`](../../data-types.md) | Advertising system. Google Ads, Facebook Ads, etc. ||
  || **utmMedium**
  [`string`](../../data-types.md) | Traffic type. Possible values:
  
  - CPC — ads
  - CPM — banners ||
  || **utmCampaign** [`string`](../../data-types.md) | Advertising campaign designation ||
  || **utmContent**
  [`string`](../../data-types.md) | Campaign content. For example, for contextual ads ||
  || **utmTerm**
  [`string`](../../data-types.md) | Campaign search term. For example, keywords for contextual advertising ||
  || **ufCrm...**
  [`crm_userfield`](../data-types.md) | Custom field. See section [{#T}](./user-defined-fields/index.md)

    - Values for multiple fields are passed as an array
    - To upload a file, you must pass an array as the custom field value, where the first item is the filename and the second is the file content encoded in [base64](../../files/how-to-upload-files.md)
  
  ||
  || **parentId...**
  [`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

  Each such field has a `parentId + {parentEntityTypeId}` code
  ||
  |#

- Contact

  CRM object identifier **entityTypeId:** `3`

  #|
  || **Name**
  `type` | **Description** ||
  || **honorific**
  [`crm_status`](../data-types.md) | String identifier of the contact request.
  
  For example, `'HNR_DE_1' = 'Mr.'`.

  A list of available requests can be obtained using [`crm.status.list`](../status/crm-status-list.md) by applying the `{ ENTITY_ID: "HONOFIRIC" }` filter ||
  || **name**
  [`string`](../../data-types.md) | First Name ||
  || **secondName**
  [`string`](../../data-types.md) | Middle Name ||
  || **lastName**
  [`string`](../../data-types.md) | Last Name ||
  || **photo**
  [`file`](../../data-types.md) | Photograph ||
  || **birthdate**
  [`date`](../../data-types.md) | Date of birth ||
  || **typeId**
  [`crm_status`](../data-types.md) | String identifier of the entity type.
  
  For example, for a deal: `'SALE' = 'Sale'`.
  
  A list of available entity types can be obtained using [`crm.status.list`](../status/crm-status-list.md) by applying the `{ ENTITY_ID: "CONTACT_TYPE" }` filter  ||
  || **sourceId**
  [`crm_status`](../data-types.md) | String identifier of the source.
  
  For example, `'CALL' = 'Call'`.
  
  A list of available sources can be obtained using [`crm.status.list`](../status/crm-status-list.md) by applying the `{ ENTITY_ID: "SOURCE" }` filter  ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source ||
  || **post**
  [`string`](../../data-types.md) | Job Title ||
  || **comments**
  [`text`](../../data-types.md) | Comment ||
  || **opened**
  [`boolean`](../../data-types.md) | Whether the item is available to everyone. Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **export**
  [`boolean`](../../data-types.md) | Whether the contact is included in the export ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the item ||
  || **companyId**
  [`crm_company`](../data-types.md) | Company identifier linked to the item.

  A list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method via `entityTypeId = 4` ||
  || **companyIds**
  [`crm_company`](../data-types.md)     | Array of company identifiers that will be linked to the item ||
  || **leadId**
  [`crm_lead`](../data-types.md) | Lead identifier on the basis of which the item is created ||
  || **originatorId**
  [`string`](../../data-types.md) | External source ||
  || **originId**
  [`string`](../../data-types.md) | Identifier of the element in the external source ||
  || **originVersion**
  [`string`](../../data-types.md)          | Original version ||
  || **observers**
  [`user[]`](../../data-types.md) | Array of user identifiers who will be Observers in the item ||
  || **utmSource**
  [`string`](../../data-types.md) | Advertising system. Google Ads, Facebook Ads, etc. ||
  || **utmMedium**
  [`string`](../../data-types.md) | Traffic type. Possible values:
  
  - CPC — ads
  - CPM — banners ||
  || **utmCampaign**
  [`string`](../../data-types.md) | Advertising campaign designation ||
  || **utmContent**
  [`string`](../../data-types.md) | Campaign content. For example, for contextual ads ||
  || **utmTerm**
  [`string`](../../data-types.md) | Campaign search term. For example, keywords for contextual advertising ||
  || **ufCrm...**
  [`crm_userfield`](../data-types.md) | Custom field. See section [{#T}](./user-defined-fields/index.md)

    - Values for multiple fields are passed as an array
    - To upload a file, you must pass an array as the custom field value, where the first item is the filename and the second is the file content encoded in [base64](../../files/how-to-upload-files.md)

  ||
  || **parentId...**
  [`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

  Each such field has a `parentId + {parentEntityTypeId}` code
  ||
  || **fm**
  [`crm_multifield[]`](../data-types.md#crm_multifield) | Array of multi-fields (phones, e-mail, messengers).

  Structure of each item:
  - `typeId` — multi-field type: `PHONE`, `EMAIL`, `WEB`, `IM`
  - `valueType` — value subtype: `WORK`, `MOBILE`, `HOME`, `MAILING`, `OTHER`
  - `value` — value

  The item key in the object determines the operation:

  **Add** a new value — use keys `n0`, `n1`, `n2` ...:

  ```json
  "fm": {
      "n0": { "typeId": "PHONE", "valueType": "WORK", "value": "+499991234567" },
      "n1": { "typeId": "EMAIL", "valueType": "WORK", "value": "new@example.com" }
  }
  ```

  **Update** an existing value — use the numeric `id` of the record (taken from the [`crm.item.get`](./crm-item-get.md) response in the `fm` field):

  ```json
  "fm": {
      "15": { "typeId": "PHONE", "valueType": "MOBILE", "value": "+499990000000" }
  }
  ```

  **Delete** a value — pass the numeric `id` of the record with an empty `value`:

  ```json
  "fm": {
      "16": { "typeId": "EMAIL", "value": "" }
  }
  ```

  Operations can be combined in a single request.

  By default — `null`
  ||
  |#

- Company

  CRM object identifier **entityTypeId:** `4`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../data-types.md) | Name of the item ||
  || **typeId**
  [`crm_status`](../data-types.md) | String identifier of the entity type.
  
  For example, for a deal: `'SALE' = 'Sale'`.
  
  A list of available entity types can be obtained using [`crm.status.list`](../status/crm-status-list.md) by applying the `{ ENTITY_ID: "COMPANY_TYPE" }` filter ||
  || **logo**
  [`file`](../../data-types.md) | Logo ||
  || **bankingDetails**
  [`string`](../../data-types.md) | Banking Details ||
  || **industry**
  [`crm_status`](../data-types.md) | String identifier of the industry type.
  
  For example `'IT' = 'Information Technology'`.
  
  A list of available industry types can be obtained using the [`crm.status.list`](../status/crm-status-list.md) method by applying the `{ ENTITY_ID: "INDUSTRY"}` filter ||
  || **employees**
  [`crm_status`](../data-types.md) | String identifier of the employee count type.
  
  The value is taken from the list of available values, for example `'EMPLOYEES_1' = 'less than 50'`.

  A list of available employee count types can be obtained using the [`crm.status.list`](../status/crm-status-list.md) method by applying the `{ ENTITY_ID: "EMPLOYEES" }` filter ||
  || **currencyId**
  [`crm_currency`](../data-types.md) | Item currency identifier ||
  || **revenue**
  [`double`](../../data-types.md) | Annual revenue ||
  || **opened**
  [`boolean`](../../data-types.md) | Whether the item is available to everyone. Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **comments**
  [`text`](../../data-types.md) | Comment ||
  || **isMyCompany**
  [`boolean`](../../data-types.md) | Whether the company is my company ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the item ||
  || **contactIds**
  [`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item.

  A list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method via `entityTypeId = 3`||
  || **leadId**
  [`crm_lead`](../data-types.md) | Lead identifier on the basis of which the item is created||
  || **originatorId**
  [`string`](../../data-types.md) | External source ||
  || **originId**
  [`string`](../../data-types.md) | Identifier of the element in the external source ||
  || **originVersion**
  [`string`](../../data-types.md) | Original version ||
  || **observers**
  [`user[]`](../../data-types.md) | Array of user identifiers who will be Observers in the item ||
  || **utmSource**
  [`string`](../../data-types.md) | Advertising system. Google Ads, Facebook Ads, etc. ||
  || **utmMedium**
  [`string`](../../data-types.md) | Traffic type. Possible values:
  - CPC — ads
  - CPM — banners ||
  || **utmCampaign**
  [`string`](../../data-types.md) | Advertising campaign designation ||
  || **utmContent**
  [`string`](../../data-types.md) | Campaign content. For example, for contextual ads ||
  || **utmTerm**
  [`string`](../../data-types.md) | Campaign search term. For example, keywords for contextual advertising ||
  || **ufCrm...**
  [`crm_userfield`](../data-types.md) | Custom field. See section [{#T}](./user-defined-fields/index.md)

    - Values for multiple fields are passed as an array
    - To upload a file, you must pass an array as the custom field value, where the first item is the filename and the second is the file content encoded in [base64](../../files/how-to-upload-files.md)

  ||
  || **parentId...**
  [`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

  Each such field has a `parentId + {parentEntityTypeId}` code
  ||
  || **fm**
  [`crm_multifield[]`](../data-types.md#crm_multifield) | Array of multi-fields (phones, e-mail, messengers).

  Structure of each item:
  - `typeId` — multi-field type: `PHONE`, `EMAIL`, `WEB`, `IM`
  - `valueType` — value subtype: `WORK`, `MOBILE`, `HOME`, `MAILING`, `OTHER`
  - `value` — value

  The item key in the object determines the operation:

  **Add** a new value — use keys `n0`, `n1`, `n2` ...:

  ```json
  "fm": {
      "n0": { "typeId": "PHONE", "valueType": "WORK", "value": "+499991234567" },
      "n1": { "typeId": "EMAIL", "valueType": "WORK", "value": "new@example.com" }
  }
  ```

  **Update** an existing value — use the numeric `id` of the record (taken from the [`crm.item.get`](./crm-item-get.md) response in the `fm` field):

  ```json
  "fm": {
      "15": { "typeId": "PHONE", "valueType": "MOBILE", "value": "+499990000000" }
  }
  ```

  **Delete** a value — pass the numeric `id` of the record with an empty `value`:
  
  ```json
  "fm": {
      "16": { "typeId": "EMAIL", "value": "" }
  }
  ```

  Operations can be combined in a single request.

  By default — `null`
  ||
  |#

- Estimate

  CRM object identifier **entityTypeId:** `7`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../data-types.md) | Name of the item ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the item ||
  || **opened**
  [`boolean`](../../data-types.md) | Whether the item is available to everyone. Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **content**
  [`text`](../../data-types.md) | Content ||
  || **terms**
  [`text`](../../data-types.md) | Conditions ||
  || **comments**
  [`text`](../../data-types.md) | Comment ||
  || **dealId**
  [`crm_deal`](../data-types.md) | Linked deal identifier ||
  || **leadId**
  [`crm_lead`](../data-types.md) | Lead identifier on the basis of which the item is created ||
  || **storageTypeId**
  [`integer`](../../data-types.md) | Storage type identifier. Possible values:
  - `1` — file
  - `2` — WebDAV
  - `3` — Drive
  ||
  || **storageElementIds**
  [`integer`](../../data-types.md) | Files array ||
  || **webformId**
  [`integer`](../../data-types.md) | CRM Form identifier ||
  || **companyId**
  [`crm_company`](../data-types.md) | Company identifier linked to the item.

  A list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 4` ||
  || **contactId**
  [`crm_contact`](../data-types.md) | Contact identifier linked to the item.

  A list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 3` ||
  || **contactIds**
  [`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item.

  A list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 3` ||
  || **locationId**
  [`location`](../data-types.md) | Location identifier. Service field ||
  || **currencyId**
  [`crm_currency`](../data-types.md) | Item currency identifier ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Amount calculation mode.

  - `Y` — manual
  - `N` — automatic
  ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount ||
  || **taxValue**
  [`double`](../../data-types.md) | Tax amount ||
  || **stageId**
  [`crm_status`](../data-types.md) | String identifier of the item stage.
  
  For example `'DRAFT' = 'New'`.

  A list of available stages can be obtained using [`crm.status.list`](../status/crm-status-list.md) by applying the `{ ENTITY_ID: "QUOTE_STATUS" }` filter ||
  || **begindate**
  [`date`](../../data-types.md) | Item start date ||
  || **closedate**
  [`date`](../../data-types.md) | Item end date ||
  || **actualDate**
  [`date`](../../data-types.md) | Valid until ||
  || **mycompanyId**
  [`crm_company`](../data-types.md) | My company identifier ||
  || **utmSource**
  [`string`](../../data-types.md) | Advertising system. Google Ads, Facebook Ads, etc. ||
  || **utmMedium**
  [`string`](../../data-types.md) | Traffic type.
  
  - CPC — ads
  - CPM — banners ||
  || **utmCampaign**
  [`string`](../../data-types.md) | Advertising campaign designation ||
  || **utmContent**
  [`string`](../../data-types.md) | Campaign content. For example, for contextual ads ||
  || **utmTerm**
  [`string`](../../data-types.md) | Campaign search term. For example, keywords for contextual advertising ||
  || **ufCrm...**
  [`crm_userfield`](../data-types.md) | Custom field. See section [{#T}](./user-defined-fields/index.md).

  - Values for multiple fields are passed as an array
  - To upload a file, you must pass an array as the custom field value, where the first item is the filename and the second is the file content encoded in [base64](../../files/how-to-upload-files.md)

  ||
  || **parentId...**
  [`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

  Each such field has a `parentId + {parentEntityTypeId}` code
  ||
  |#

- Invoice

  CRM object identifier **entityTypeId:** `31`

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../data-types.md) | Name of the item
  ||
  || **xmlId**
  [`string`](../../data-types.md) | External code ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the item ||
  || **opened**
  [`boolean`](../../data-types.md) | Whether the item is available to everyone. Possible values:

  - `Y` — yes
  - `N` — no
  ||
  || **webformId**
  [`integer`](../../data-types.md) | CRM Form identifier ||
  || **begindate**
  [`date`](../../data-types.md) | Item start date ||
  || **closedate**
  [`date`](../../data-types.md) | Item end date ||
  || **companyId**
  [`crm_company`](../data-types.md) | Company identifier linked to the item.

  A list of companies can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 4` ||
  || **contactId**
  [`crm_contact`](../data-types.md) | Contact identifier linked to the item.

  A list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 3` ||
  || **contactIds**
  [`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item.

  A list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 3` ||
  || **observers**
  [`user[]`](../../data-types.md) | Array of user identifiers who will be Observers in the item ||
  || **stageId**
  [`crm_status`](../data-types.md) | String identifier of the item stage.
  
  For example `'DT31_13:N' = 'New'`.

  A list of available stages can be obtained using [`crm.status.list`](../status/crm-status-list.md) by applying the filter: `{ ENTITY_ID: "SMART_INVOICE_STAGE_{categoryId}" }`, where
  `categoryId` — the default invoice pipeline identifier. It can be found using [`crm.category.list`](category/crm-category-list.md) via `entityTypeId = 31` ||
  || **sourceId**
  [`crm_status`](../data-types.md) | String identifier of the source.
  
  For example `'CALL' = 'Call'`.
  
  A list of available sources can be obtained using [`crm.status.list`](../status/crm-status-list.md) by applying the `{ ENTITY_ID: "SOURCE" }` filter ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source ||
  || **currencyId**
  [`crm_currency`](../data-types.md) | Item currency identifier ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Amount calculation mode. Possible values:

  - `Y` — manual
  - `N` — automatic
  ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount ||
  || **taxValue**
  [`double`](../../data-types.md) | Tax amount ||
  || **mycompanyId**
  [`crm_company`](../data-types.md) | My company identifier ||
  || **comments**
  [`text`](../../data-types.md) | Comment ||
  || **locationId**
  [`location`](../data-types.md) | Location identifier. Service field ||
  || **ufCrm...**
  [`crm_userfield`](../data-types.md) | Custom field. See section [{#T}](./user-defined-fields/index.md).

    - Values for multiple fields are passed as an array
    - To upload a file, you must pass an array as the custom field value, where the first item is the filename and the second is the file content encoded in [base64](../../files/how-to-upload-files.md)

  ||
  || **parentId...**
  [`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

  Each such field has a `parentId + {parentEntityTypeId}` code
  ||
  |#

- SPA

  CRM object identifier **entityTypeId:** can be retrieved via the [`crm.type.list`](user-defined-object-types/crm-type-list.md) method or created via the [`crm.type.add`](user-defined-object-types/crm-type-add.md) method

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../data-types.md) | Name of the item  ||
  || **xmlId**
  [`string`](../../data-types.md) | External code ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the person responsible for the item  ||
  || **opened**
  [`boolean`](../../data-types.md) | Whether the item is available to everyone.

  - `Y` — yes
  - `N` — no
  ||
  || **webformId**
  [`integer`](../../data-types.md) | CRM Form identifier ||
  || **begindate**
  [`date`](../../data-types.md) | Item start date.

  Only available if the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA
  ||
  || **closedate**
  [`date`](../../data-types.md) | Item end date.

  Only available if the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA ||
  || **companyId**
  [`crm_company`](../data-types.md) | Company identifier linked to the item.

  The company list can be obtained using the [`crm.item.list`](crm-item-list.md) method via `entityTypeId = 4`.

  Only available if the `isClientEnabled` setting is enabled for the corresponding SPA ||
  || **contactId**
  [`crm_contact`](../data-types.md) | Contact identifier linked to the item.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 3`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA ||
  || **contactIds**
  [`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item.

  The list of contacts can be obtained using the [`crm.item.list`](crm-item-list.md) method by `entityTypeId = 3`.

  Available only if the `isClientEnabled` setting is enabled for the corresponding SPA ||
  || **observers**
  [`user[]`](../../data-types.md) | An array of user identifiers who will be Observers in the item.

  Available only if the `isObserversEnabled` setting is enabled for the corresponding SPA ||
  || **categoryId**
  [`crm_category`](../data-types.md) | SPA item pipeline identifier.

  If the identifier is not specified, the SPA will be moved to the main pipeline.

  The list of available pipelines can be found using [`crm.category.list`](category/crm-category-list.md) by applying the corresponding `entityTypeId` ||
  || **stageId**
  [`crm_status`](../data-types.md) | String identifier of the item stage.
  
  For example, `'DT1220_30:NEW' = 'Start'`.

  The list of available stages can be found using [`crm.status.list`](../status/crm-status-list.md) by applying the `{ ENTITY_ID: "DYNAMIC_{entityTypeId}_STAGE_{categoryId}" }` filter, where:
  - `entityTypeId` — SPA type identifier
  - `categoryId` — SPA item pipeline (direction) identifier

  [More details about pipelines (directions)](category/index.md).

  Available only if the `isStagesEnabled` setting is enabled for the corresponding SPA  ||
  || **sourceId**
  [`crm_status`](../data-types.md) | String identifier of the source. (for example `'CALL' = 'Call'`).
  
  The list of available sources can be found using [`crm.status.list`](../status/crm-status-list.md) by applying the `{ ENTITY_ID: "SOURCE" }` filter.

  Available only if the `isSourceEnabled` setting is enabled for the corresponding SPA  ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source.

  Available only if the `isSourceEnabled` setting is enabled for the corresponding SPA ||
  || **currencyId**
  [`crm_currency`](../data-types.md) | Item currency identifier.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA  ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Amount calculation mode. Possible values:

  - `Y` — manual
  - `N` — automatic

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA||
  || **opportunity**
  [`double`](../../data-types.md) | Amount.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA ||
  || **taxValue**
  [`double`](../../data-types.md) | Tax amount.

  Available only if the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA ||
  || **mycompanyId**
  [`crm_company`](../data-types.md) | My company identifier.

  Available only when the `isMycompanyEnabled` setting is enabled for the corresponding SPA ||
  || **ufCrm...**
  [`crm_userfield`](../data-types.md) | Custom field. See section [{#T}](./user-defined-fields/index.md).

    - Values for multiple fields are passed as an array
    - To upload a file, you must pass an array as the custom field value, where the first item is the filename and the second is the file content encoded in [base64](../../files/how-to-upload-files.md)

  ||
  || **parentId...**
  [`crm_entity`](../data-types.md) | Parent field. An item of another CRM object type that is linked to this item.

  Each such field has a `parentId + {parentEntityTypeId}` code
  ||
  |#

  {% note info "SPA settings" %}

  You can read more about managing SPA configurations in [{#T}](./user-defined-object-types/index.md)

  {% endnote %}

{% endlist %}

## How to Update a File Type Custom Field

1. Upload a new file to replace the old one (non-multiple field)

    To replace a file in a single-value field, upload a new file. The old one will be deleted automatically.

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

2. Remove the value of a custom file field

    To do this, simply pass an empty string (`''`) instead of a value.

3. Keep the value of a non-multiple file field unchanged

    The simplest option is to not include the key for this field in `fields`.
    
    However, if you need to pass the key without changing the value, you must pass a list where the file ID is provided under the `id` key.

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
    
    If you pass a value different from the current one in `id`, the field value will be reset and the file will be deleted.
    
    {% endnote %}

4. Working with a multiple file field

    The value of a multiple field is an array. Each array element follows the same rules as non-multiple values.

    **How to partially overwrite values in a multiple file field**

    For example, the multiple file field currently contains the values `[12, 255, 44]`.

    You need to keep files `12` and `44`, and instead of `255`, upload a new one.

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
    -d '{"entityTypeId":2,"id":351,"fields":{"title":"REST Deal #1","stageId":"C9:UC_NYL06U","assignedById":6,"observers":[1,2,3],"opened":"N","typeId":"SERVICE","opportunity":10000,"currencyId":"USD","additionalInfo":"Changing a deal via REST","isManualOpportunity":"N","utmSource":"google","ufCrm_1721244707107":200.05,"parentId1220":2}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"id":351,"fields":{"title":"REST Deal #1","stageId":"C9:UC_NYL06U","assignedById":6,"observers":[1,2,3],"opened":"N","typeId":"SERVICE","opportunity":10000,"currencyId":"USD","additionalInfo":"Changing a deal via REST","isManualOpportunity":"N","utmSource":"google","ufCrm_1721244707107":200.05,"parentId1220":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.update
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
    type ItemUpdateResult = {
      item: CrmItem
    }

    try {
      const response = await $b24.actions.v2.call.make<ItemUpdateResult>({
        method: 'crm.item.update',
        params: {
          entityTypeId: 2,
          id: 351,
          fields: {
            title: 'REST Deal #1',
            stageId: 'C9:UC_NYL06U',
            assignedById: 6,
            observers: [1, 2, 3],
            opened: 'N',
            typeId: 'SERVICE',
            opportunity: 10000,
            currencyId: 'USD',
            additionalInfo: 'Update a deal via REST',
            isManualOpportunity: 'N',
            utmSource: 'google',
            ufCrm_1721244707107: 200.05,
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
        console.info(`Updated item #${result.item.id} (${result.item.title})`)
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
      async function updateCrmItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.update',
            params: {
              entityTypeId: 2,
              id: 351,
              fields: {
                title: 'REST Deal #1',
                stageId: 'C9:UC_NYL06U',
                assignedById: 6,
                observers: [1, 2, 3],
                opened: 'N',
                typeId: 'SERVICE',
                opportunity: 10000,
                currencyId: 'USD',
                additionalInfo: 'Update a deal via REST',
                isManualOpportunity: 'N',
                utmSource: 'google',
                ufCrm_1721244707107: 200.05,
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
          console.info(`Updated item #${result.item.id} (${result.item.title})`)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateCrmItem)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.update(
            entity_type_id=2,
            bitrix_id=351,
            fields={
                "title": "REST Deal #1",
                "stageId": "C9:UC_NYL06U",
                "assignedById": 6,
                "observers": [1, 2, 3],
                "opened": "N",
                "typeId": "SERVICE",
                "opportunity": 10000,
                "currencyId": "USD",
                "additionalInfo": "Changing a deal via REST",
                "isManualOpportunity": "N",
                "utmSource": "google",
                "ufCrm_1721244707107": 200.05,
                "parentId1220": 2,
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $entityTypeId = 1; // Set your entity type ID
        $id = 123; // Set the ID of the item to update
        $fields = [
            'TITLE' => 'Updated Title',
            'DATE_MODIFIED' => (new DateTime())->format(DateTime::ATOM), // Example DateTime field
            // Add other fields as necessary
        ];

        $itemService = $serviceBuilder->getCRMScope()->item();
        $updateResult = $itemService->update($entityTypeId, $id, $fields);

        if ($updateResult->isSuccess()) {
            print("Item updated successfully: " . json_encode($updateResult));
        } else {
            print("Failed to update item.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

- PHP CRest

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
                'additionalInfo' => "Changing a deal via REST",
                'isManualOpportunity' => "N",
                'utmSource' => "google",
                'ufCrm_1721244707107' => 200.05,
                'parentId1220' => 2,
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.item.update", b24.Params{
    	"entityTypeId": 2,
    	"id":           351,
    	"fields": b24.Params{
    		"title":               "REST Deal #1",
    		"stageId":             "C9:UC_NYL06U",
    		"assignedById":        6,
    		"observers":           []int{1, 2, 3},
    		"opened":              "N",
    		"typeId":              "SERVICE",
    		"opportunity":         10000,
    		"currencyId":          "USD",
    		"additionalInfo":      "Changing a deal via REST",
    		"isManualOpportunity": "N",
    		"utmSource":           "google",
    		"ufCrm_1721244707107": 200.05,
    		"parentId1220":        2,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.item.update: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID           b24.ID `json:"id"`
    	CreatedTime  string `json:"createdTime"`
    	UpdatedTime  string `json:"updatedTime"`
    	CreatedBy    int    `json:"createdBy"`
    	UpdatedBy    int    `json:"updatedBy"`
    	AssignedByID b24.ID `json:"assignedById"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.CreatedTime)
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
            "additionalInfo": "Changing a deal via REST",
            "searchContent": "351 Deal #351 10200.00 EUR Not Invented Not Invented Sale Name2134234233 23.07.2024 31.07.2024",
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
            "parentId1220": 2,
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
[`object`](../../data-types.md) | Root element of the response, contains a single key `item` ||
|| **item**
[`item`](./object-fields.md) | Information about the updated item, [description of fields](./object-fields.md) ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

{% note info " " %}

By default, custom field names are passed and returned in camelCase, for example `ufCrm2_1639669411830`.
When passing the `useOriginalUfNames` parameter with the value `Y`, custom fields will be returned with their original names, for example `UF_CRM_2_1639669411830`.

{% endnote %}

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
|| `400`      | `ACCESS_DENIED`                   | Access denied                                                    | User does not have permission to modify items of type `entityTypeId`                              ||
|| `400`      | `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid value for field "`field`"                                   | Incorrect value passed for field `field`                                                     ||
|| `400`      | `100`                             | Expected iterable value for multiple field, but got `type` instead | One of the multiple fields received a value of type `type`, while an iterable type was expected ||
|| `400`      | `-`                               | Insufficient permissions to change the stage                                  | If the user tries to change the stage of the item while lacking sufficient rights      ||
|| `400`      | `UPDATE_DYNAMIC_ITEM_RESTRICTED`  | You cannot change the item due to your plan restrictions      | Plan restrictions do not allow modifying smart process items                               ||
|#

{% include [System errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](crm-item-add.md)
- [{#T}](crm-item-get.md)
- [{#T}](crm-item-list.md)
- [{#T}](crm-item-delete.md)
- [{#T}](crm-item-fields.md)
- [{#T}](./object-fields.md)
