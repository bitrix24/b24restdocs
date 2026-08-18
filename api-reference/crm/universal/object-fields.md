# CRM Object Fields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In the section [common fields](#common), you will find a list of standard fields used across all types of CRM objects.

The sections for each object type provide lists of standard fields specific to that object type:

- [lead](#lead),
- [deal](#deal),
- [contact](#contact),
- [company](#company),
- [estimate](#quote),
- [invoice](#invoice),
- [SPA](#spa).

Use the method [crm.item.fields](./crm-item-fields.md) with the specified [object type](../data-types.md#object_type) in `entityTypeId` to get a complete list of fields for the object, including custom fields.

## Common Fields {#common}

  #|
  || **Name**
  `type` | **Description** ||
  || **assignedById**
  [`user`](../../data-types.md) | Identifier of the user responsible for the item ||
  || **createdBy**
  [`user`](../../data-types.md) | Identifier of the user who created the item ||
  || **createdTime**
  [`datetime`](../../data-types.md) | Time of item creation ||
  || **entityTypeId**
  [`integer`](../../data-types.md) | Identifier of the entity type ||
  || **id**
  [`integer`](../../data-types.md) | Identifier of the item ||
  || **lastActivityBy**
  [`user`](../../data-types.md) | Identifier of the user who last interacted in the timeline ||
  || **lastActivityTime**
  [`datetime`](../../data-types.md) | Time of the last activity in the timeline ||
  || **opened**
  [`boolean`](../../data-types.md) | Is the item open ||
  || **parentId...**
  [`crm_entity`](../data-types.md) | Parent field. An element of another CRM object type that is linked to this item.
  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  || **ufCrm...**
  [`crm_userfield`](../data-types.md) | Custom field. See the section [{#T}](./user-defined-fields/index.md).
  - Values of multiple fields are returned as an array
  - Value of the `file` type field is returned as an object:
  - `id` — identifier
  - `url` — link to the file on the account
  - `urlMachine` — link to the file for the application
  ||
  || **updatedBy**
  [`user`](../../data-types.md) | Identifier of the user who modified the item ||
  || **updatedTime**
  [`datetime`](../../data-types.md) | Time of the last modification of the item ||
  || **utmCampaign**
  [`string`](../../data-types.md) | Identifier of the advertising campaign ||
  || **utmContent**
  [`string`](../../data-types.md) | Content of the campaign.
  For example, for contextual ads
  ||
  || **utmMedium**
  [`string`](../../data-types.md) | Type of traffic. Possible values:
  - CPC — ads
  - CPM — banners
  ||
  || **utmSource**
  [`string`](../../data-types.md) | Advertising system. Google Ads and others ||
  || **utmTerm**
  [`string`](../../data-types.md) | Search condition of the campaign.
  For example, keywords for contextual advertising
  ||
  || **webformId**
  [`integer`](../../data-types.md) | Identifier of the CRM form ||
  |#

## Object Fields

### Lead {#lead}

  #|
  || **Name**
  `type` | **Description** ||
  || **dateCreateShort**
  [`datetime`](../../data-types.md) | Time of item creation (short format).
  Field is disabled
  ||
  || **dateModifyShort**
  [`datetime`](../../data-types.md) | Time of the last modification of the item (short format).
  Field is disabled
  ||
  || **companyId**
  [`crm_company`](../data-types.md) | Identifier of the company linked to the item ||
  || **contactId**
  [`crm_contact`](../data-types.md) | Identifier of the contact linked to the item ||
  || **stageId**
  [`crm_status`](../data-types.md) | String identifier of the item's stage ||
  || **isConvert**
  [`boolean`](../../data-types.md) | Has the lead been converted.
  Field is disabled
  ||
  || **statusDescription**
  [`text`](../../data-types.md) | Additional information about the stage ||
  || **stageSemanticId**
  [`string`](../../data-types.md) | Group of the stage. Possible values:
  - `P` — in progress
  - `S` — successful
  - `F` — unsuccessful
  ||
  || **productId**
  [`string`](../../data-types.md) | Identifier of the product.
  Deprecated.
  Field is disabled
  ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount ||
  || **currencyId**
  [`crm_currency`](../data-types.md) | Identifier of the item's currency ||
  || **sourceId**
  [`crm_status`](../data-types.md) | String identifier of the source type ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source ||
  || **title**
  [`string`](../../data-types.md) | Name of the item ||
  || **name**
  [`string`](../../data-types.md) | First name ||
  || **lastName**
  [`string`](../../data-types.md) | Last name ||
  || **secondName**
  [`string`](../../data-types.md) | Middle name ||
  || **shortName**
  [`string`](../../data-types.md) | Last name First name.
  Short format: for example 'Smith John' -> 'Smith J.'.
  Field is disabled
  ||
  || **companyTitle**
  [`string`](../../data-types.md) | Company name ||
  || **post**
  [`string`](../../data-types.md) | Position ||
  || **address**
  [`text`](../../data-types.md) | Address.
  Deprecated.
  Field is disabled
  ||
  || **comments**
  [`text`](../../data-types.md) | Comment ||
  || **originatorId**
  [`string`](../../data-types.md) | External source ||
  || **originId**
  [`string`](../../data-types.md) | Identifier of the item in the external source ||
  || **dateClosed**
  [`datetime`](../../data-types.md) | Time of item closure ||
  || **birthdate**
  [`date`](../../data-types.md) | Date of birth ||
  || **honorific**
  [`crm_status`](../data-types.md) | String identifier of the salutation type ||
  || **hasPhone**
  [`boolean`](../../data-types.md) | Does the item have a phone ||
  || **hasEmail**
  [`boolean`](../../data-types.md) | Does the item have an email ||
  || **hasImol**
  [`boolean`](../../data-types.md) | Does the item have open channels ||
  || **login**
  [`string`](../../data-types.md) | Login.
  Deprecated.
  Field is disabled
  ||
  || **isReturnCustomer**
  [`boolean`](../../data-types.md) | Is the item a repeat customer ||
  || **searchContent**
  [`text`](../../data-types.md) | Information for full-text search.
  System field
  ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Is manual mode for calculating the amount set ||
  || **movedBy**
  [`user`](../../data-types.md) | Identifier of the user who last changed the stage ||
  || **movedTime**
  [`datetime`](../../data-types.md) | Time of the last stage change ||
  || **phoneMobile**
  [`string`](../../data-types.md) | Mobile phone ||
  || **phoneWork**
  [`string`](../../data-types.md) | Work phone ||
  || **phoneMailing**
  [`string`](../../data-types.md) | Mailing phone ||
  || **emailHome**
  [`string`](../../data-types.md) | Personal E-mail ||
  || **emailWork**
  [`string`](../../data-types.md) | Work E-mail ||
  || **emailMailing**
  [`string`](../../data-types.md) | Mailing email ||
  || **skype**
  [`string`](../../data-types.md) | Skype ||
  || **icq**
  [`string`](../../data-types.md) | ICQ ||
  || **imol**
  [`string`](../../data-types.md) | IMOL ||
  || **email**
  [`string`](../../data-types.md) | E-mail ||
  || **phone**
  [`string`](../../data-types.md) | Phone ||
  || **observers**
  [`user[]`](../../data-types.md) | List of user identifiers who are Observers ||
  || **contactIds**
  [`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item ||
  || **fm**
  [`crm_multifield`](../data-types.md#crm_multifield) | Array of multifields.
  More about multifields can be found in the section [{#T}](../data-types.md#crm_multifield)
  Structure of the multifield:
  - `id` — Unique identifier
  - `typeId` — Type of multifield
  - `valueType` — Type of value
  - `value` — Value
  ||
  |#

### Deal {#deal}

  #|
  || **Name**
  `type` | **Description** ||
  || **dateCreateShort**
  [`datetime`](../../data-types.md) | Time of item creation (short format).
  Field is disabled
  ||
  || **dateModifyShort**
  [`datetime`](../../data-types.md) | Time of the last modification of the item (short format).
  Field is disabled
  ||
  || **leadId**
  [`crm_lead`](../data-types.md) | Identifier of the lead based on which the item was created ||
  || **companyId**
  [`crm_company`](../data-types.md) | Identifier of the company linked to the item ||
  || **contactId**
  [`crm_contact`](../data-types.md) | Identifier of the contact linked to the item ||
  || **quoteId**
  [`crm_quote`](../data-types.md) | Identifier of the estimate linked to the item ||
  || **title**
  [`string`](../../data-types.md) | Name of the item ||
  || **productId**
  [`string`](../../data-types.md) | Identifier of the product.
  Deprecated. Field is disabled
  ||
  || **categoryId**
  [`crm_category`](../data-types.md) | Identifier of the funnel (direction) of the item ||
  || **stageId**
  [`crm_status`](../data-types.md) | String identifier of the item's stage ||
  || **stageSemanticId**
  [`string`](../../data-types.md) | Group of the stage
  - `P` — in progress
  - `S` — successful
  - `F` — unsuccessful
  ||
  || **isNew**
  [`boolean`](../../data-types.md) | Is the deal new ||
  || **isRecurring**
  [`boolean`](../../data-types.md) | Is the deal recurring ||
  || **isReturnCustomer**
  [`boolean`](../../data-types.md) | Is the item a repeat customer ||
  || **isRepeatedApproach**
  [`boolean`](../../data-types.md) | Is the deal a repeated approach ||
  || **closed**
  [`boolean`](../../data-types.md) | Is the deal closed ||
  || **typeId**
  [`crm_status`](../data-types.md) | String identifier of the deal type ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Is manual mode for calculating the amount set ||
  || **taxValue**
  [`double`](../../data-types.md) | Tax amount ||
  || **currencyId**
  [`crm_currency`](../data-types.md) | Identifier of the item's currency ||
  || **probability**
  [`integer`](../../data-types.md) | Probability, % ||
  || **comments**
  [`text`](../../data-types.md) | Comment ||
  || **begindate**
  [`date`](../../data-types.md) | Start date of the item ||
  || **begindateShort**
  [`datetime`](../../data-types.md) | Start time of the item (short format).
  Field is disabled
  ||
  || **closedate**
  [`date`](../../data-types.md) | Completion date of the item ||
  || **closedateShort**
  [`datetime`](../../data-types.md) | End time of the item (short format).
  Field is disabled
  ||
  || **eventDate**
  [`datetime`](../../data-types.md) | Event date ||
  || **eventDateShort**
  [`datetime`](../../data-types.md) | Event date (short format).
  Field is disabled
  ||
  || **eventId**
  [`crm_status`](../data-types.md) | String identifier of the event type ||
  || **eventDescription**
  [`text`](../../data-types.md) | Description of the event ||
  || **locationId**
  [`location`](../data-types.md) | Identifier of the location.
  System field
  ||
  || **sourceId**
  [`crm_status`](../data-types.md) | String identifier of the source type ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source ||
  || **originatorId**
  [`string`](../../data-types.md) | External source ||
  || **originId**
  [`string`](../../data-types.md) | Identifier of the item in the external source ||
  || **additionalInfo**
  [`string`](../../data-types.md) | Additional information ||
  || **searchContent**
  [`text`](../../data-types.md) | Information for full-text search.
  System field
  ||
  || **orderStage**
  [`string`](../../data-types.md) | Payment status of the deal ||
  || **movedBy**
  [`user`](../../data-types.md) | Identifier of the user who last changed the stage ||
  || **movedTime**
  [`datetime`](../../data-types.md) | Time of the last stage change ||
  || **isWork**
  [`boolean`](../../data-types.md) | Is the deal in progress.
  Field is disabled
  ||
  || **isWon**
  [`boolean`](../../data-types.md) | Is the deal won.
  Field is disabled
  ||
  || **isLose**
  [`boolean`](../../data-types.md) | Is the deal lost.
  Field is disabled
  ||
  || **receivedAmount**
  [`string`](../../data-types.md) | Amount received.
  Field is disabled
  ||
  || **lostAmount**
  [`string`](../../data-types.md) | Amount lost.
  Field is disabled
  ||
  || **hasProducts**
  [`boolean`](../../data-types.md) | Does the item contain products.
  Field is disabled
  ||
  || **observers**
  [`user[]`](../../data-types.md) | List of user identifiers who are Observers ||
  || **contactIds**
  [`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item ||
  |#

### Contact {#contact}

  #|
  || **Name**
  `type` | **Description** ||
  || **companyId**
  [`crm_company`](../data-types.md) | Identifier of the company linked to the item ||
  || **sourceId**
  [`crm_status`](../data-types.md) | String identifier of the source type ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source ||
  || **name**
  [`string`](../../data-types.md) | First name ||
  || **lastName**
  [`string`](../../data-types.md) | Last name ||
  || **secondName**
  [`string`](../../data-types.md) | Middle name ||
  || **shortName**
  [`string`](../../data-types.md) | Last name First name.
  Short format: for example 'Smith John' -> 'Smith J.'.
  Field is disabled
  ||
  || **photo**
  [`file`](../../data-types.md) | Photo ||
  || **post**
  [`string`](../../data-types.md) | Position ||
  || **address**
  [`text`](../../data-types.md) | Address.
  Deprecated. Field is disabled
  ||
  || **comments**
  [`text`](../../data-types.md) | Comment ||
  || **leadId**
  [`crm_lead`](../data-types.md) | Identifier of the lead based on which the item was created ||
  || **export**
  [`boolean`](../../data-types.md) | Is exporting the contact allowed ||
  || **typeId**
  [`crm_status`](../data-types.md) | String identifier of the deal type ||
  || **originatorId**
  [`string`](../../data-types.md) | External source ||
  || **originId**
  [`string`](../../data-types.md) | Identifier of the item in the external source ||
  || **originVersion**
  [`string`](../../data-types.md) | Version of the original ||
  || **birthdate**
  [`date`](../../data-types.md) | Date of birth ||
  || **honorific**
  [`crm_status`](../data-types.md) | String identifier of the salutation type ||
  || **hasPhone**
  [`boolean`](../../data-types.md) | Does the item have a phone ||
  || **hasEmail**
  [`boolean`](../../data-types.md) | Does the item have an email ||
  || **hasImol**
  [`boolean`](../../data-types.md) | Does the item have open channels ||
  || **searchContent**
  [`text`](../../data-types.md) | Information for full-text search. System field ||
  || **categoryId**
  [`crm_category`](../data-types.md) | Identifier of the funnel (direction) of the item ||
  || **login**
  [`string`](../../data-types.md) | Login.
  Deprecated. Field is disabled
  ||
  || **emailHome**
  [`string`](../../data-types.md) | Personal E-mail ||
  || **emailWork**
  [`string`](../../data-types.md) | Work E-mail ||
  || **emailMailing**
  [`string`](../../data-types.md) | Mailing email ||
  || **phoneMobile**
  [`string`](../../data-types.md) | Mobile phone ||
  || **phoneWork**
  [`string`](../../data-types.md) | Work phone ||
  || **phoneMailing**
  [`string`](../../data-types.md) | Mailing phone ||
  || **imol**
  [`string`](../../data-types.md) | IMOL ||
  || **email**
  [`string`](../../data-types.md) | E-mail ||
  || **phone**
  [`string`](../../data-types.md) | Phone ||
  || **observers**
  [`user[]`](../../data-types.md) | List of user identifiers who are Observers ||
  || **companyIds**
  [`crm_company[]`](../data-types.md) | List of company identifiers linked to the item ||
  || **fm**
  [`crm_multifield`](../data-types.md#crm_multifield) | Array of multifields.
  More about multifields can be found in the section [{#T}](../data-types.md#crm_multifield)
  Structure of the multifield:
  - `id` — Unique identifier
  - `typeId` — Type of multifield
  - `valueType` — Type of value
  - `value` — Value
  ||
  |#

### Company {#company}

  #|
  || **Name**
  `type` | **Description** ||
  || **title**
  [`string`](../../data-types.md) | Name of the item ||
  || **logo**
  [`file`](../../data-types.md) | Logo ||
  || **address**
  [`text`](../../data-types.md) | Address.
  Deprecated. Field is disabled
  ||
  || **addressLegal**
  [`text`](../../data-types.md) | Legal address.
  Deprecated
  ||
  || **bankingDetails**
  [`string`](../../data-types.md) | Banking details ||
  || **comments**
  [`text`](../../data-types.md) | Comment ||
  || **typeId**
  [`crm_status`](../data-types.md) | String identifier of the deal type ||
  || **industry**
  [`crm_status`](../data-types.md) | String identifier of the industry type ||
  || **revenue**
  [`double`](../../data-types.md) | Annual turnover ||
  || **currencyId**
  [`crm_currency`](../data-types.md) | Identifier of the item's currency ||
  || **employees**
  [`crm_status`](../data-types.md) | String identifier of the number of employees type ||
  || **leadId**
  [`crm_lead`](../data-types.md) | Identifier of the lead based on which the item was created ||
  || **originatorId**
  [`string`](../../data-types.md) | External source ||
  || **originId**
  [`string`](../../data-types.md) | Identifier of the item in the external source ||
  || **originVersion**
  [`string`](../../data-types.md) | Version of the original ||
  || **hasPhone**
  [`boolean`](../../data-types.md) | Does the item have a phone ||
  || **hasEmail**
  [`boolean`](../../data-types.md) | Does the item have an email ||
  || **hasImol**
  [`boolean`](../../data-types.md) | Does the item have open channels ||
  || **isMyCompany**
  [`boolean`](../../data-types.md) | Is the company my company ||
  || **searchContent**
  [`text`](../../data-types.md) | Information for full-text search.
  System field
  ||
  || **categoryId**
  [`crm_category`](../data-types.md) | Identifier of the funnel (direction) of the item ||
  || **emailHome**
  [`string`](../../data-types.md) | Personal E-mail ||
  || **emailWork**
  [`string`](../../data-types.md) | Work E-mail ||
  || **emailMailing**
  [`string`](../../data-types.md) | Mailing email ||
  || **phoneMobile**
  [`string`](../../data-types.md) | Mobile phone ||
  || **phoneWork**
  [`string`](../../data-types.md) | Work phone ||
  || **phoneMailing**
  [`string`](../../data-types.md) | Mailing phone ||
  || **imol**
  [`string`](../../data-types.md) | IMOL ||
  || **email**
  [`string`](../../data-types.md) | E-mail ||
  || **phone**
  [`string`](../../data-types.md) | Phone ||
  || **ufLogo**
  [`file`](../../data-types.md) | Logo (document generator) ||
  || **ufStamp**
  [`file`](../../data-types.md) | Company seal (document generator) ||
  || **ufDirectorSign**
  [`file`](../../data-types.md) | Director's signature (document generator) ||
  || **ufAccountantSign**
  [`file`](../../data-types.md) | Chief accountant's signature (document generator) ||
  || **observers**
  [`user[]`](../../data-types.md) | List of user identifiers who are Observers ||
  || **contactIds**
  [`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item ||
  || **fm**
  [`crm_multifield`](../data-types.md#crm_multifield) | Array of multifields.
  More about multifields can be found in the section [{#T}](../data-types.md#crm_multifield)
  Structure of the multifield:
  - `id` — Unique identifier
  - `typeId` — Type of multifield
  - `valueType` — Type of value
  - `value` — Value
  ||
  |#

### Estimate {#quote}

  #|
  || **Name**
  `type` | **Description** ||
  || **dateCreateShort**
  [`datetime`](../../data-types.md) | Time of item creation (short format).
  Field is disabled
  ||
  || **dateModifyShort**
  [`datetime`](../../data-types.md) | Time of the last modification of the item (short format).
  Field is disabled
  ||
  || **leadId**
  [`crm_lead`](../data-types.md) | Identifier of the lead based on which the item was created ||
  || **dealId**
  [`crm_deal`](../data-types.md) | Identifier of the deal linked to the item ||
  || **companyId**
  [`crm_company`](../data-types.md) | Identifier of the company linked to the item ||
  || **contactId**
  [`crm_contact`](../data-types.md) | Identifier of the contact linked to the item ||
  || **personTypeId**
  [`integer`](../../data-types.md) | Identifier of the payer type ||
  || **mycompanyId**
  [`crm_company`](../data-types.md) | Identifier of "my" company ||
  || **title**
  [`string`](../../data-types.md) | Name of the item ||
  || **stageId**
  [`crm_status`](../data-types.md) | String identifier of the item's stage ||
  || **closed**
  [`boolean`](../../data-types.md) | Is the deal closed ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Is manual mode for calculating the amount set ||
  || **taxValue**
  [`double`](../../data-types.md) | Tax amount ||
  || **currencyId**
  [`crm_currency`](../data-types.md) | Identifier of the item's currency ||
  || **comments**
  [`text`](../../data-types.md) | Comment ||
  || **commentsType**
  [`integer`](../../data-types.md) | Identifier of the comment type.
  Possible values:
  - `0` — unknown
  - `1` — text
  - `2` — bb-code
  - `3` — HTML
  ||
  || **begindate**
  [`date`](../../data-types.md) | Start date of the item ||
  || **begindateShort**
  [`datetime`](../../data-types.md) | Start time of the item (short format).
  Field is disabled
  ||
  || **closedate**
  [`date`](../../data-types.md) | Completion date of the item ||
  || **closedateShort**
  [`datetime`](../../data-types.md) | End time of the item (short format).
  Field is disabled
  ||
  || **quoteNumber**
  [`string`](../../data-types.md) | Estimate number ||
  || **content**
  [`text`](../../data-types.md) | Content ||
  || **contentType**
  [`integer`](../../data-types.md) | Identifier of the content type.
  Possible values:
  - `0` — unknown
  - `1` — text
  - `2` — bb-code
  - `3` — HTML
  ||
  || **terms**
  [`text`](../../data-types.md) | Terms ||
  || **termsType**
  [`integer`](../../data-types.md) | Identifier of the terms type.
  Possible values:
  - `0` — unknown
  - `1` — text
  - `2` — bb-code
  - `3` — HTML
  ||
  || **storageTypeId**
  [`integer`](../../data-types.md) | Identifier of the storage type ||
  || **storageElementIds**
  [`integer[]`](../../data-types.md) | Array of files ||
  || **locationId**
  [`location`](../data-types.md) | Identifier of the location. System field ||
  || **clientTitle**
  [`string`](../../data-types.md) | Client name ||
  || **clientAddr**
  [`string`](../../data-types.md) | Client address ||
  || **clientContact**
  [`string`](../../data-types.md) | Client contacts ||
  || **clientEmail**
  [`string`](../../data-types.md) | Client E-mail ||
  || **clientPhone**
  [`string`](../../data-types.md) | Client phone ||
  || **clientTpId**
  [`string`](../../data-types.md) | Client TIN ||
  || **clientTpaId**
  [`string`](../../data-types.md) | Client TPP ||
  || **searchContent**
  [`text`](../../data-types.md) | Information for full-text search. System field ||
  || **hasProducts**
  [`boolean`](../../data-types.md) | Does the item contain products.
  Field is disabled
  ||
  || **actualDate**
  [`date`](../../data-types.md) | Valid until ||
  || **contactIds**
  [`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item ||
  |#

### Invoice {#invoice}

  #|
  || **Name**
  `type` | **Description** ||
  || **xmlId**
  [`string`](../../data-types.md) | External code ||
  || **title**
  [`string`](../../data-types.md) | Name of the item ||
  || **movedBy**
  [`user`](../../data-types.md) | Identifier of the user who last changed the stage ||
  || **movedTime**
  [`datetime`](../../data-types.md) | Time of the last stage change ||
  || **categoryId**
  [`crm_category`](../data-types.md) | Identifier of the funnel (direction) of the item ||
  || **stageId**
  [`crm_status`](../data-types.md) | String identifier of the item's stage ||
  || **previousStageId**
  [`crm_status`](../data-types.md) | Identifier of the previous stage type ||
  || **begindate**
  [`date`](../../data-types.md) | Start date of the item ||
  || **closedate**
  [`date`](../../data-types.md) | Completion date of the item ||
  || **companyId**
  [`crm_company`](../data-types.md) | Identifier of the company linked to the item ||
  || **contactId**
  [`crm_contact`](../data-types.md) | Identifier of the contact linked to the item ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Is manual mode for calculating the amount set ||
  || **taxValue**
  [`double`](../../data-types.md) | Tax amount ||
  || **currencyId**
  [`crm_currency`](../data-types.md) | Identifier of the item's currency ||
  || **mycompanyId**
  [`crm_company`](../data-types.md) | Identifier of "my" company ||
  || **sourceId**
  [`crm_status`](../data-types.md) | String identifier of the source type ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source ||
  || **comments**
  [`text`](../../data-types.md) | Comment ||
  || **accountNumber**
  [`string`](../../data-types.md) | Invoice number ||
  || **locationId**
  [`location`](../data-types.md) | Identifier of the location.
  System field
  ||
  || **observers**
  [`user[]`](../../data-types.md) | List of user identifiers who are Observers ||
  || **contactIds**
  [`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item ||
  |#

### SPA {#spa}

  #|
  || **Name**
  `type` | **Description** ||
  || **xmlId**
  [`string`](../../data-types.md) | External code ||
  || **title**
  [`string`](../../data-types.md) | Name of the item ||
  || **movedBy**
  [`user`](../../data-types.md) | Identifier of the user who last changed the stage.
  Available only when the `isStagesEnabled` setting is enabled for the corresponding SPA
  ||
  || **movedTime**
  [`datetime`](../../data-types.md) | Time of the last stage change.
  Available only when the `isStagesEnabled` setting is enabled for the corresponding SPA
  ||
  || **categoryId**
  [`crm_category`](../data-types.md) | Identifier of the funnel (direction) of the item ||
  || **stageId**
  [`crm_status`](../data-types.md) | String identifier of the item's stage.
  Available only when the `isStagesEnabled` setting is enabled for the corresponding SPA
  ||
  || **previousStageId**
  [`crm_status`](../data-types.md) | Identifier of the previous stage type.
  Available only when the `isStagesEnabled` setting is enabled for the corresponding SPA
  ||
  || **begindate**
  [`date`](../../data-types.md) | Start date of the item.
  Available only when the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA
  ||
  || **closedate**
  [`date`](../../data-types.md) | Completion date of the item.
  Available only when the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA
  ||
  || **companyId**
  [`crm_company`](../data-types.md) | Identifier of the company linked to the item.
  Available only when the `isClientEnabled` setting is enabled for the corresponding SPA
  ||
  || **contactId**
  [`crm_contact`](../data-types.md) | Identifier of the contact linked to the item.
  Available only when the `isClientEnabled` setting is enabled for the corresponding SPA
  ||
  || **opportunity**
  [`double`](../../data-types.md) | Amount.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **isManualOpportunity**
  [`boolean`](../../data-types.md) | Is manual mode for calculating the amount set.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **taxValue**
  [`double`](../../data-types.md) | Tax amount.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **currencyId**
  [`crm_currency`](../data-types.md) | Identifier of the item's currency.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **opportunityAccount**
  [`double`](../../data-types.md) | Amount in accounting currency.
  Deprecated. Field is disabled.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **taxValueAccount**
  [`double`](../../data-types.md) | Tax amount in accounting currency.
  Deprecated. Field is disabled.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **accountCurrencyId**
  [`crm_currency`](../data-types.md) | Accounting currency.
  Field is disabled.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **mycompanyId**
  [`crm_company`](../data-types.md) | Identifier of "my" company.
  Available only when the `isMycompanyEnabled` setting is enabled for the corresponding SPA
  ||
  || **sourceId**
  [`crm_status`](../data-types.md) | String identifier of the source type.
  Available only when the `isSourceEnabled` setting is enabled for the corresponding SPA
  ||
  || **sourceDescription**
  [`text`](../../data-types.md) | Additional information about the source.
  Available only when the `isSourceEnabled` setting is enabled for the corresponding SPA
  ||
  || **observers**
  [`user[]`](../../data-types.md) | List of user identifiers who are Observers.
  Available only when the `isObserversEnabled` setting is enabled for the corresponding SPA
  ||
  || **contactIds**
  [`crm_contact[]`](../data-types.md) | List of contact identifiers linked to the item.
  Available only when the `isClientEnabled` setting is enabled for the corresponding SPA
  ||
  |#
