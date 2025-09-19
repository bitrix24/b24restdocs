# CRM Object Fields

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
  [`user`][1] | Identifier of the user responsible for the item ||
  || **createdBy**
  [`user`][1] | Identifier of the user who created the item ||
  || **createdTime**
  [`datetime`][1] | Time of item creation ||
  || **entityTypeId**
  [`integer`][1] | Identifier of the entity type ||
  || **id**
  [`integer`][1] | Identifier of the item ||
  || **lastActivityBy**
  [`user`][1] | Identifier of the user who last interacted in the timeline ||
  || **lastActivityTime**
  [`datetime`][1] | Time of the last activity in the timeline ||
  || **opened**
  [`boolean`][1] | Is the item open ||
  || **parentId...**
  [`crm_entity`][1] | Parent field. An element of another CRM object type that is linked to this item.
  Each such field has the code `parentId + {parentEntityTypeId}`
  ||
  || **ufCrm...**
  [`crm_userfield`][1] | Custom field. See the section [{#T}](./user-defined-fields/index.md).
  - Values of multiple fields are returned as an array
  - Value of the `file` type field is returned as an object:
  - `id` — identifier
  - `url` — link to the file on the account
  - `urlMachine` — link to the file for the application
  ||
  || **updatedBy**
  [`user`][1] | Identifier of the user who modified the item ||
  || **updatedTime**
  [`datetime`][1] | Time of the last modification of the item ||
  || **utmCampaign**
  [`string`][1] | Identifier of the advertising campaign ||
  || **utmContent**
  [`string`][1] | Content of the campaign.
  For example, for contextual ads
  ||
  || **utmMedium**
  [`string`][1] | Type of traffic. Possible values:
  - CPC — ads
  - CPM — banners
  ||
  || **utmSource**
  [`string`][1] | Advertising system. Google Ads and others ||
  || **utmTerm**
  [`string`][1] | Search condition of the campaign.
  For example, keywords for contextual advertising
  ||
  || **webformId**
  [`integer`][1] | Identifier of the CRM form ||
  |#

## Object Fields

### Lead {#lead}

  #|
  || **Name**
  `type` | **Description** ||
  || **dateCreateShort**
  [`datetime`][1] | Time of item creation (short format).
  Field is disabled
  ||
  || **dateModifyShort**
  [`datetime`][1] | Time of the last modification of the item (short format).
  Field is disabled
  ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the item ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the item ||
  || **stageId**
  [`crm_status`][1] | String identifier of the item's stage ||
  || **isConvert**
  [`boolean`][1] | Has the lead been converted.
  Field is disabled
  ||
  || **statusDescription**
  [`text`][1] | Additional information about the stage ||
  || **stageSemanticId**
  [`string`][1] | Group of the stage. Possible values:
  - `P` — in progress
  - `S` — successful
  - `F` — unsuccessful
  ||
  || **productId**
  [`string`][1] | Identifier of the product.
  Deprecated.
  Field is disabled
  ||
  || **opportunity**
  [`double`][1] | Amount ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the item's currency ||
  || **sourceId**
  [`crm_status`][1] | String identifier of the source type ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source ||
  || **title**
  [`string`][1] | Name of the item ||
  || **name**
  [`string`][1] | First name ||
  || **lastName**
  [`string`][1] | Last name ||
  || **secondName**
  [`string`][1] | Middle name ||
  || **shortName**
  [`string`][1] | Last name First name.
  Short format: for example 'Smith John' -> 'Smith J.'.
  Field is disabled
  ||
  || **companyTitle**
  [`string`][1] | Company name ||
  || **post**
  [`string`][1] | Position ||
  || **address**
  [`text`][1] | Address.
  Deprecated.
  Field is disabled
  ||
  || **comments**
  [`text`][1] | Comment ||
  || **originatorId**
  [`string`][1] | External source ||
  || **originId**
  [`string`][1] | Identifier of the item in the external source ||
  || **dateClosed**
  [`datetime`][1] | Time of item closure ||
  || **birthdate**
  [`date`][1] | Date of birth ||
  || **honorific**
  [`crm_status`][1] | String identifier of the salutation type ||
  || **hasPhone**
  [`boolean`][1] | Does the item have a phone ||
  || **hasEmail**
  [`boolean`][1] | Does the item have an email ||
  || **hasImol**
  [`boolean`][1] | Does the item have open channels ||
  || **login**
  [`string`][1] | Login.
  Deprecated.
  Field is disabled
  ||
  || **isReturnCustomer**
  [`boolean`][1] | Is the item a repeat customer ||
  || **searchContent**
  [`text`][1] | Information for full-text search.
  System field
  ||
  || **isManualOpportunity**
  [`boolean`][1] | Is manual mode for calculating the amount set ||
  || **movedBy**
  [`user`][1] | Identifier of the user who last changed the stage ||
  || **movedTime**
  [`datetime`][1] | Time of the last stage change ||
  || **phoneMobile**
  [`string`][1] | Mobile phone ||
  || **phoneWork**
  [`string`][1] | Work phone ||
  || **phoneMailing**
  [`string`][1] | Mailing phone ||
  || **emailHome**
  [`string`][1] | Personal E-mail ||
  || **emailWork**
  [`string`][1] | Work E-mail ||
  || **emailMailing**
  [`string`][1] | Mailing email ||
  || **skype**
  [`string`][1] | Skype ||
  || **icq**
  [`string`][1] | ICQ ||
  || **imol**
  [`string`][1] | IMOL ||
  || **email**
  [`string`][1] | E-mail ||
  || **phone**
  [`string`][1] | Phone ||
  || **observers**
  [`user[]`][1] | List of user identifiers who are Observers ||
  || **contactIds**
  [`crm_contact[]`][1] | List of contact identifiers linked to the item ||
  || **fm**
  [`multifield`][1] | Array of multifields.
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
  [`datetime`][1] | Time of item creation (short format).
  Field is disabled
  ||
  || **dateModifyShort**
  [`datetime`][1] | Time of the last modification of the item (short format).
  Field is disabled
  ||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the item was created ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the item ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the item ||
  || **quoteId**
  [`crm_quote`][1] | Identifier of the estimate linked to the item ||
  || **title**
  [`string`][1] | Name of the item ||
  || **productId**
  [`string`][1] | Identifier of the product.
  Deprecated. Field is disabled
  ||
  || **categoryId**
  [`crm_category`][1] | Identifier of the funnel (direction) of the item ||
  || **stageId**
  [`crm_status`][1] | String identifier of the item's stage ||
  || **stageSemanticId**
  [`string`][1] | Group of the stage
  - `P` — in progress
  - `S` — successful
  - `F` — unsuccessful
  ||
  || **isNew**
  [`boolean`][1] | Is the deal new ||
  || **isRecurring**
  [`boolean`][1] | Is the deal recurring ||
  || **isReturnCustomer**
  [`boolean`][1] | Is the item a repeat customer ||
  || **isRepeatedApproach**
  [`boolean`][1] | Is the deal a repeated approach ||
  || **closed**
  [`boolean`][1] | Is the deal closed ||
  || **typeId**
  [`crm_status`][1] | String identifier of the deal type ||
  || **opportunity**
  [`double`][1] | Amount ||
  || **isManualOpportunity**
  [`boolean`][1] | Is manual mode for calculating the amount set ||
  || **taxValue**
  [`double`][1] | Tax amount ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the item's currency ||
  || **probability**
  [`integer`][1] | Probability, % ||
  || **comments**
  [`text`][1] | Comment ||
  || **begindate**
  [`date`][1] | Start date of the item ||
  || **begindateShort**
  [`datetime`][1] | Start time of the item (short format).
  Field is disabled
  ||
  || **closedate**
  [`date`][1] | Completion date of the item ||
  || **closedateShort**
  [`datetime`][1] | End time of the item (short format).
  Field is disabled
  ||
  || **eventDate**
  [`datetime`][1] | Event date ||
  || **eventDateShort**
  [`datetime`][1] | Event date (short format).
  Field is disabled
  ||
  || **eventId**
  [`crm_status`][1] | String identifier of the event type ||
  || **eventDescription**
  [`text`][1] | Description of the event ||
  || **locationId**
  [`location`][1] | Identifier of the location.
  System field
  ||
  || **sourceId**
  [`crm_status`][1] | String identifier of the source type ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source ||
  || **originatorId**
  [`string`][1] | External source ||
  || **originId**
  [`string`][1] | Identifier of the item in the external source ||
  || **additionalInfo**
  [`string`][1] | Additional information ||
  || **searchContent**
  [`text`][1] | Information for full-text search.
  System field
  ||
  || **orderStage**
  [`string`][1] | Payment status of the deal ||
  || **movedBy**
  [`user`][1] | Identifier of the user who last changed the stage ||
  || **movedTime**
  [`datetime`][1] | Time of the last stage change ||
  || **isWork**
  [`boolean`][1] | Is the deal in progress.
  Field is disabled
  ||
  || **isWon**
  [`boolean`][1] | Is the deal won.
  Field is disabled
  ||
  || **isLose**
  [`boolean`][1] | Is the deal lost.
  Field is disabled
  ||
  || **receivedAmount**
  [`string`][1] | Amount received.
  Field is disabled
  ||
  || **lostAmount**
  [`string`][1] | Amount lost.
  Field is disabled
  ||
  || **hasProducts**
  [`boolean`][1] | Does the item contain products.
  Field is disabled
  ||
  || **observers**
  [`user[]`][1] | List of user identifiers who are Observers ||
  || **contactIds**
  [`crm_contact[]`][1] | List of contact identifiers linked to the item ||
  |#

### Contact {#contact}

  #|
  || **Name**
  `type` | **Description** ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the item ||
  || **sourceId**
  [`crm_status`][1] | String identifier of the source type ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source ||
  || **name**
  [`string`][1] | First name ||
  || **lastName**
  [`string`][1] | Last name ||
  || **secondName**
  [`string`][1] | Middle name ||
  || **shortName**
  [`string`][1] | Last name First name.
  Short format: for example 'Smith John' -> 'Smith J.'.
  Field is disabled
  ||
  || **photo**
  [`file`][1] | Photo ||
  || **post**
  [`string`][1] | Position ||
  || **address**
  [`text`][1] | Address.
  Deprecated. Field is disabled
  ||
  || **comments**
  [`text`][1] | Comment ||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the item was created ||
  || **export**
  [`boolean`][1] | Is exporting the contact allowed ||
  || **typeId**
  [`crm_status`][1] | String identifier of the deal type ||
  || **originatorId**
  [`string`][1] | External source ||
  || **originId**
  [`string`][1] | Identifier of the item in the external source ||
  || **originVersion**
  [`string`][1] | Version of the original ||
  || **birthdate**
  [`date`][1] | Date of birth ||
  || **honorific**
  [`crm_status`][1] | String identifier of the salutation type ||
  || **hasPhone**
  [`boolean`][1] | Does the item have a phone ||
  || **hasEmail**
  [`boolean`][1] | Does the item have an email ||
  || **hasImol**
  [`boolean`][1] | Does the item have open channels ||
  || **searchContent**
  [`text`][1] | Information for full-text search. System field ||
  || **categoryId**
  [`crm_category`][1] | Identifier of the funnel (direction) of the item ||
  || **login**
  [`string`][1] | Login.
  Deprecated. Field is disabled
  ||
  || **emailHome**
  [`string`][1] | Personal E-mail ||
  || **emailWork**
  [`string`][1] | Work E-mail ||
  || **emailMailing**
  [`string`][1] | Mailing email ||
  || **phoneMobile**
  [`string`][1] | Mobile phone ||
  || **phoneWork**
  [`string`][1] | Work phone ||
  || **phoneMailing**
  [`string`][1] | Mailing phone ||
  || **imol**
  [`string`][1] | IMOL ||
  || **email**
  [`string`][1] | E-mail ||
  || **phone**
  [`string`][1] | Phone ||
  || **observers**
  [`user[]`][1] | List of user identifiers who are Observers ||
  || **companyIds**
  [`crm_company[]`][1] | List of company identifiers linked to the item ||
  || **fm**
  [`multifield`][1] | Array of multifields.
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
  [`string`][1] | Name of the item ||
  || **logo**
  [`file`][1] | Logo ||
  || **address**
  [`text`][1] | Address.
  Deprecated. Field is disabled
  ||
  || **addressLegal**
  [`text`][1] | Legal address.
  Deprecated
  ||
  || **bankingDetails**
  [`string`][1] | Banking details ||
  || **comments**
  [`text`][1] | Comment ||
  || **typeId**
  [`crm_status`][1] | String identifier of the deal type ||
  || **industry**
  [`crm_status`][1] | String identifier of the industry type ||
  || **revenue**
  [`double`][1] | Annual turnover ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the item's currency ||
  || **employees**
  [`crm_status`][1] | String identifier of the number of employees type ||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the item was created ||
  || **originatorId**
  [`string`][1] | External source ||
  || **originId**
  [`string`][1] | Identifier of the item in the external source ||
  || **originVersion**
  [`string`][1] | Version of the original ||
  || **hasPhone**
  [`boolean`][1] | Does the item have a phone ||
  || **hasEmail**
  [`boolean`][1] | Does the item have an email ||
  || **hasImol**
  [`boolean`][1] | Does the item have open channels ||
  || **isMyCompany**
  [`boolean`][1] | Is the company my company ||
  || **searchContent**
  [`text`][1] | Information for full-text search.
  System field
  ||
  || **categoryId**
  [`crm_category`][1] | Identifier of the funnel (direction) of the item ||
  || **emailHome**
  [`string`][1] | Personal E-mail ||
  || **emailWork**
  [`string`][1] | Work E-mail ||
  || **emailMailing**
  [`string`][1] | Mailing email ||
  || **phoneMobile**
  [`string`][1] | Mobile phone ||
  || **phoneWork**
  [`string`][1] | Work phone ||
  || **phoneMailing**
  [`string`][1] | Mailing phone ||
  || **imol**
  [`string`][1] | IMOL ||
  || **email**
  [`string`][1] | E-mail ||
  || **phone**
  [`string`][1] | Phone ||
  || **ufLogo**
  [`file`][1] | Logo (document generator) ||
  || **ufStamp**
  [`file`][1] | Company seal (document generator) ||
  || **ufDirectorSign**
  [`file`][1] | Director's signature (document generator) ||
  || **ufAccountantSign**
  [`file`][1] | Chief accountant's signature (document generator) ||
  || **observers**
  [`user[]`][1] | List of user identifiers who are Observers ||
  || **contactIds**
  [`crm_contact[]`][1] | List of contact identifiers linked to the item ||
  || **fm**
  [`multifield`][1] | Array of multifields.
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
  [`datetime`][1] | Time of item creation (short format).
  Field is disabled
  ||
  || **dateModifyShort**
  [`datetime`][1] | Time of the last modification of the item (short format).
  Field is disabled
  ||
  || **leadId**
  [`crm_lead`][1] | Identifier of the lead based on which the item was created ||
  || **dealId**
  [`crm_deal`][1] | Identifier of the deal linked to the item ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the item ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the item ||
  || **personTypeId**
  [`integer`][1] | Identifier of the payer type ||
  || **mycompanyId**
  [`crm_company`][1] | Identifier of "my" company ||
  || **title**
  [`string`][1] | Name of the item ||
  || **stageId**
  [`crm_status`][1] | String identifier of the item's stage ||
  || **closed**
  [`boolean`][1] | Is the deal closed ||
  || **opportunity**
  [`double`][1] | Amount ||
  || **isManualOpportunity**
  [`boolean`][1] | Is manual mode for calculating the amount set ||
  || **taxValue**
  [`double`][1] | Tax amount ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the item's currency ||
  || **comments**
  [`text`][1] | Comment ||
  || **commentsType**
  [`integer`][1] | Identifier of the comment type.
  Possible values:
  - `0` — unknown
  - `1` — text
  - `2` — bb-code
  - `3` — HTML
  ||
  || **begindate**
  [`date`][1] | Start date of the item ||
  || **begindateShort**
  [`datetime`][1] | Start time of the item (short format).
  Field is disabled
  ||
  || **closedate**
  [`date`][1] | Completion date of the item ||
  || **closedateShort**
  [`datetime`][1] | End time of the item (short format).
  Field is disabled
  ||
  || **quoteNumber**
  [`string`][1] | Estimate number ||
  || **content**
  [`text`][1] | Content ||
  || **contentType**
  [`integer`][1] | Identifier of the content type.
  Possible values:
  - `0` — unknown
  - `1` — text
  - `2` — bb-code
  - `3` — HTML
  ||
  || **terms**
  [`text`][1] | Terms ||
  || **termsType**
  [`integer`][1] | Identifier of the terms type.
  Possible values:
  - `0` — unknown
  - `1` — text
  - `2` — bb-code
  - `3` — HTML
  ||
  || **storageTypeId**
  [`integer`][1] | Identifier of the storage type ||
  || **storageElementIds**
  [`integer[]`][1] | Array of files ||
  || **locationId**
  [`location`][1] | Identifier of the location. System field ||
  || **clientTitle**
  [`string`][1] | Client name ||
  || **clientAddr**
  [`string`][1] | Client address ||
  || **clientContact**
  [`string`][1] | Client contacts ||
  || **clientEmail**
  [`string`][1] | Client E-mail ||
  || **clientPhone**
  [`string`][1] | Client phone ||
  || **clientTpId**
  [`string`][1] | Client TIN ||
  || **clientTpaId**
  [`string`][1] | Client TPP ||
  || **searchContent**
  [`text`][1] | Information for full-text search. System field ||
  || **hasProducts**
  [`boolean`][1] | Does the item contain products.
  Field is disabled
  ||
  || **actualDate**
  [`date`][1] | Valid until ||
  || **contactIds**
  [`crm_contact[]`][1] | List of contact identifiers linked to the item ||
  |#

### Invoice {#invoice}

  #|
  || **Name**
  `type` | **Description** ||
  || **xmlId**
  [`string`][1] | External code ||
  || **title**
  [`string`][1] | Name of the item ||
  || **movedBy**
  [`user`][1] | Identifier of the user who last changed the stage ||
  || **movedTime**
  [`datetime`][1] | Time of the last stage change ||
  || **categoryId**
  [`crm_category`][1] | Identifier of the funnel (direction) of the item ||
  || **stageId**
  [`crm_status`][1] | String identifier of the item's stage ||
  || **previousStageId**
  [`crm_status`][1] | Identifier of the previous stage type ||
  || **begindate**
  [`date`][1] | Start date of the item ||
  || **closedate**
  [`date`][1] | Completion date of the item ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the item ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the item ||
  || **opportunity**
  [`double`][1] | Amount ||
  || **isManualOpportunity**
  [`boolean`][1] | Is manual mode for calculating the amount set ||
  || **taxValue**
  [`double`][1] | Tax amount ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the item's currency ||
  || **mycompanyId**
  [`crm_company`][1] | Identifier of "my" company ||
  || **sourceId**
  [`crm_status`][1] | String identifier of the source type ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source ||
  || **comments**
  [`text`][1] | Comment ||
  || **accountNumber**
  [`string`][1] | Invoice number ||
  || **locationId**
  [`location`][1] | Identifier of the location.
  System field
  ||
  || **observers**
  [`user[]`][1] | List of user identifiers who are Observers ||
  || **contactIds**
  [`crm_contact[]`][1] | List of contact identifiers linked to the item ||
  |#

### SPA {#spa}

  #|
  || **Name**
  `type` | **Description** ||
  || **xmlId**
  [`string`][1] | External code ||
  || **title**
  [`string`][1] | Name of the item ||
  || **movedBy**
  [`user`][1] | Identifier of the user who last changed the stage.
  Available only when the `isStagesEnabled` setting is enabled for the corresponding SPA
  ||
  || **movedTime**
  [`datetime`][1] | Time of the last stage change.
  Available only when the `isStagesEnabled` setting is enabled for the corresponding SPA
  ||
  || **categoryId**
  [`crm_category`][1] | Identifier of the funnel (direction) of the item ||
  || **stageId**
  [`crm_status`][1] | String identifier of the item's stage.
  Available only when the `isStagesEnabled` setting is enabled for the corresponding SPA
  ||
  || **previousStageId**
  [`crm_status`][1] | Identifier of the previous stage type.
  Available only when the `isStagesEnabled` setting is enabled for the corresponding SPA
  ||
  || **begindate**
  [`date`][1] | Start date of the item.
  Available only when the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA
  ||
  || **closedate**
  [`date`][1] | Completion date of the item.
  Available only when the `isBeginCloseDatesEnabled` setting is enabled for the corresponding SPA
  ||
  || **companyId**
  [`crm_company`][1] | Identifier of the company linked to the item.
  Available only when the `isClientEnabled` setting is enabled for the corresponding SPA
  ||
  || **contactId**
  [`crm_contact`][1] | Identifier of the contact linked to the item.
  Available only when the `isClientEnabled` setting is enabled for the corresponding SPA
  ||
  || **opportunity**
  [`double`][1] | Amount.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **isManualOpportunity**
  [`boolean`][1] | Is manual mode for calculating the amount set.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **taxValue**
  [`double`][1] | Tax amount.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **currencyId**
  [`crm_currency`][1] | Identifier of the item's currency.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **opportunityAccount**
  [`double`][1] | Amount in accounting currency.
  Deprecated. Field is disabled.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **taxValueAccount**
  [`double`][1] | Tax amount in accounting currency.
  Deprecated. Field is disabled.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **accountCurrencyId**
  [`crm_currency`][1] | Accounting currency.
  Field is disabled.
  Available only when the `isLinkWithProductsEnabled` setting is enabled for the corresponding SPA
  ||
  || **mycompanyId**
  [`crm_company`][1] | Identifier of "my" company.
  Available only when the `isMycompanyEnabled` setting is enabled for the corresponding SPA
  ||
  || **sourceId**
  [`crm_status`][1] | String identifier of the source type.
  Available only when the `isSourceEnabled` setting is enabled for the corresponding SPA
  ||
  || **sourceDescription**
  [`text`][1] | Additional information about the source.
  Available only when the `isSourceEnabled` setting is enabled for the corresponding SPA
  ||
  || **observers**
  [`user[]`][1] | List of user identifiers who are Observers.
  Available only when the `isObserversEnabled` setting is enabled for the corresponding SPA
  ||
  || **contactIds**
  [`crm_contact[]`][1] | List of contact identifiers linked to the item.
  Available only when the `isClientEnabled` setting is enabled for the corresponding SPA
  ||
  |#


[1]: ../data-types.md