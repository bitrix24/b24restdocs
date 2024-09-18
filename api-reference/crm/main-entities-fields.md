# Fields of Main CRM Objects

> **Attention!** A more complete list of fields can be found on the pages of methods that return the description of object fields. Such methods are named **crm.object_name.fields**.

{% list tabs %}

- Deals

    The field description is returned by the method [crm.deal.fields](./deals/crm-deal-fields.md)

    #| 
    || **Name**
    `type` | **Description** | **Read** | **Write** ||
    || **ID**
    [`integer`][1] | Deal identifier | Yes | No ||
    || **TITLE**
    [`string`][1] | Title | Yes | Yes ||
    || **TYPE_ID**
    [`crm_status`][2] | Type of deal. Used only for linking to external sources. | Yes | Yes ||
    || **CATEGORY_ID**
    [`crm_category`][1] | Identifier of the direction. Immutable. If this field is not provided when creating a deal, the deal will be created in the general direction. | Yes | Yes ||
    || **STAGE_ID**
    [`crm_status`][2] | Identifier of the stage. Possible values:
    - `NEW` — new deal
    - `PREPARATION` — preparing documents
    - `PREPAYMENT_INVOICE` — sending invoice
    - `EXECUTING` — in progress
    - `FINAL_INVOICE` — final invoice
    - `WON` — won
    - `>LOSE` — lost, no analysis required
    - `APOLOGY` — lost, analysis required | Yes | Yes ||
    || **STAGE_SEMANTIC_ID**
    [`string`][1] | Name. Read-only. In a sense, summarizes the values of the deal identifier `STAGE_ID`:
    - `P` — for stages with identifiers `NEW`, `PREPARATION`, `PREPAYMENT_INVOICE`, `EXECUTING`, and `FINAL_INVOICE`
    - `S` — for the stage with identifier `WON`
    - `F` — for stages with identifiers `>LOSE` and `APOLOGY` | Yes | No ||
    || **IS_NEW**
    [`char`][1] | Flag for a new deal (deals in the first stage) | Yes | No ||
    || **IS_RECURRING**
    [`char`][1] | Flag for a recurring deal template. If set to `Y`, it is a template, not a deal | Yes | Yes ||
    || **IS_RETURN_CUSTOMER**
    [`char`][1] | Indicator of a repeat lead | Yes | Yes ||
    || **IS_REPEATED_APPROACH**
    [`char`][1] | Repeated approach | Yes | Yes ||
    || **PROBABILITY**
    [`integer`][1] | Probability | Yes | Yes ||
    || **CURRENCY_ID**
    [`crm_currency`][2] | Identifier of the deal currency | Yes | Yes ||
    || **OPPORTUNITY**
    [`double`][1] | Amount | Yes | Yes ||
    || **IS_MANUAL_OPPORTUNITY**
    [`char`][1] | Indicator of manual opportunity calculation | Yes | Yes ||
    || **TAX_VALUE**
    [`double`][1] | Tax rate | Yes | Yes ||
    || **COMPANY_ID**
    [`crm_company`][2] | Identifier of the linked company | Yes | Yes ||
    || **CONTACT_ID**
    [`crm_contact`][2] | Identifier of the linked contact. Deprecated. Kept for compatibility | Yes | Yes ||
    || **CONTACT_IDS**
    [`crm_contact`][2] | Identifier of the linked contact. Multiple.

    When using [crm.deal.update](./deals/crm-deal-update.md) and [crm.deal.add](./deals/crm-deal-add.md), an array of contacts can be submitted. 
    
    In the methods [crm.deal.list](./deals/crm-deal-list.md) and [crm.deal.get](./deals/crm-deal-get.md), this field does not exist, and [crm.deal.contact.items.get](./deals/contacts/crm-deal-contact-items-get.md) should be used to get the list of contacts. 
    
    To clear the field, use [crm.deal.contact.items.delete](./deals/contacts/crm-deal-contact-items-delete.md), to replace the value use [crm.deal.contact.items.set](./deals/contacts/crm-deal-contact-items-set.md) | Yes | Yes ||
    || **QUOTE_ID**
    [`crm_quote`][2] | Identifier of the quote. Read-only. Deprecated. Use the method [crm.quote.list](./quote/crm-quote-list.md) with a filter by deal | Yes | No ||
    || **BEGINDATE**
    [`date`][1] | Start date | Yes | Yes ||
    || **CLOSEDATE**
    [`date`][1] | End date | Yes | Yes ||
    || **OPENED**
    [`char`][1] | Available to everyone | Yes | Yes ||
    || **CLOSED**
    [`char`][1] | Whether the deal is closed | Yes | Yes ||
    || **COMMENTS**
    [`string`][1] | Comments | Yes | Yes ||
    || **ASSIGNED_BY_ID**
    [`user`][1] | Linked to user by ID | Yes | Yes ||
    || **CREATED_BY_ID**
    [`user`][1] | Created by user | Yes | No ||
    || **MODIFY_BY_ID**
    [`user`][1] | Identifier of the author of the last change | Yes | No ||
    || **MOVED_BY_ID**
    [`user`][1] | Identifier of the author who moved the item to the current stage | Yes | No ||
    || **DATE_CREATE**
    [`datetime`][1] | Creation date | Yes | No ||
    || **DATE_MODIFY**
    [`datetime`][1] | Modification date | Yes | No ||
    || **MOVED_TIME**
    [`datetime`][1] | Date of moving the item to the current stage | Yes | No ||
    || **SOURCE_ID**
    [`string`][1] | Identifier of the source. Defines the source of the deal (callback, advertisement, e-mail, etc.). 
    
    A list of possible identifiers can be obtained using the method [crm.status.list](./status/crm-status-list.md) with the filter `filter[ENTITY_ID]=SOURCE` | Yes | Yes ||
    || **SOURCE_DESCRIPTION**
    [`string`][1] | Additional information about the source. Text field | Yes | Yes ||
    || **ADDITIONAL_INFO**
    [`string`][1] | Additional information | Yes | Yes ||
    || **LEAD_ID**
    [`crm_lead`][2] | Identifier of the linked lead | Yes | No ||
    || **LOCATION_ID**
    [`location`][1] | Client's location. Service field, not recommended for use | Yes | Yes ||
    || **ORIGINATOR_ID**
    [`string`][1] | Identifier of the data source. Used only for linking to external sources | Yes | Yes ||
    || **ORIGIN_ID**
    [`string`][1] | Identifier of the item in the data source. Used only for linking to external sources | Yes | Yes ||
    || **UTM_SOURCE**
    [`string`][1] | Advertising system (Google-Adwords, etc.) | Yes | Yes ||
    || **UTM_MEDIUM**
    [`string`][1] | Type of traffic: CPC (ads), CPM (banners) | Yes | Yes ||
    || **UTM_CAMPAIGN**
    [`string`][1] | Designation of the advertising campaign | Yes | Yes ||
    || **UTM_CONTENT**
    [`string`][1] | Content of the campaign. For example, for contextual ads | Yes | Yes ||
    || **UTM_TERM**
    [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising | Yes | Yes ||
    || **PARENT_ID_xxx**
    [`crm_entity`][2] | Relationship fields. 
    
    If there are SPAs related to contacts on the account, then for each such SPA there is a field that stores the relationship between this SPA and the contact. The field itself stores the identifier of the item of that SPA. 
    
    For example, the field `PARENT_ID_153` — relationship with the SPA `entityTypeId=153`, stores the identifier of the item of this SPA related to the current contact | Yes | Yes ||
    || **LAST_ACTIVITY_BY**
    [`string`][1] | Identifier of the user responsible for the last activity in this lead (for example, who created a new deal in the lead) | Yes | Yes ||
    || **LAST_ACTIVITY_TIME**
    [`datetime`][1] | Time of the last activity | Yes | Yes ||
    || **UF_CRM_xxx** | [User-defined fields](./deals/user-defined-fields/index.md) | Yes | Yes ||
    |#

- Leads

    The field description is returned by the method [crm.lead.fields](./leads/crm-lead-fields.md)

    #| 
    || **Name**
    `type` | **Description** | **Read** | **Write** ||
    || **ID**
    [`integer`][1] | Integer identifier of the lead | Yes | No ||
    || **TITLE**
    [`string`][1] | Title of the lead | Yes | Yes ||
    || **HONORIFIC**
    [`crm_status`][2] | Form of address. Status from the directory. 
    
    A list of possible identifiers can be obtained using the method [crm.status.list](./status/crm-status-list.md) with the filter `filter[ENTITY_ID]=HONORIFIC` | Yes | Yes ||
    || **NAME**
    [`string`][1] | Contact's first name | Yes | Yes ||
    || **SECOND_NAME**
    [`string`][1] | Contact's middle name | Yes | Yes ||
    || **LAST_NAME**
    [`string`][1] | Contact's last name | Yes | Yes ||
    || **BIRTHDATE**
    [`date`][1] | Date of birth | Yes | Yes ||
    || **COMPANY_TITLE**
    [`string`][1] | Title of the company linked to the lead | Yes | Yes ||
    || **SOURCE_ID**
    [`crm_status`][2] | Identifier of the source. Status from the directory. 
    
    A list of possible identifiers can be obtained using the method [crm.status.list](./status/crm-status-list.md) with the filter `filter[ENTITY_ID]=SOURCE` | Yes | Yes ||
    || **SOURCE_DESCRIPTION**
    [`string`][1] | Description of the source | Yes | Yes ||
    || **STATUS_ID**
    [`crm_status`][2] | Identifier of the lead stage. Status from the directory. 
    
    A list of possible identifiers can be obtained using the method [crm.status.list](./status/crm-status-list.md) with the filter `filter[ENTITY_ID]=STATUS` | Yes | Yes ||
    || **STATUS_DESCRIPTION**
    [`string`][1] | Additional information about the stage | Yes | Yes ||
    || **STATUS_SEMANTIC_ID**
    [`string`][1] | Status. Possible values:
    - `F` (failed) — processed unsuccessfully
    - `S` (success) — processed successfully
    - `P` (processing) — lead in processing | Yes | No ||
    || **POST**
    [`string`][1] | Position | Yes | Yes ||
    || **ADDRESS**
    [`string`][1] | Contact's address | Yes | Yes ||
    || **ADDRESS_2**
    [`string`][1] | Second line of the address. In some countries, it is customary to split the address into 2 parts | Yes | Yes ||
    || **ADDRESS_CITY**
    [`string`][1] | City | Yes | Yes ||
    || **ADDRESS_POSTAL_CODE**
    [`string`][1] | Postal code | Yes | Yes ||
    || **ADDRESS_REGION**
    [`string`][1] | Region | Yes | Yes ||
    || **ADDRESS_PROVINCE**
    [`string`][1] | Province | Yes | Yes ||
    || **ADDRESS_COUNTRY**
    [`string`][1] | Country | Yes | Yes ||
    || **ADDRESS_COUNTRY_CODE**
    [`string`][1] | Country code | Yes | Yes ||
    || **ADDRESS_LOC_ADDR_ID**
    [`integer`][1] | Identifier of the address from the location module | Yes | Yes ||
    || **CURRENCY_ID**
    [`crm_currency`][2] | Identifier of the currency | Yes | Yes ||
    || **OPPORTUNITY**
    [`double`][1] | Estimated amount | Yes | Yes ||
    || **IS_MANUAL_OPPORTUNITY**
    [`char`][1] | Indicator of manual calculation of the amount. Allowed values `Y` or `N` | Yes | Yes ||
    || **OPENED**
    [`char`][1] | Available to everyone. Allowed values `Y` or `N` | Yes | Yes ||
    || **COMMENTS**
    [`string`][1] | Comments | Yes | Yes ||
    || **HAS_PHONE**
    [`char`][1] | Indicator of the phone field being filled. Allowed values `Y` or `N` | Yes | No ||
    || **HAS_EMAIL**
    [`char`][1] | Indicator of the e-mail field being filled. Allowed values `Y` or `N` | Yes | No ||
    || **HAS_IMOL**
    [`char`][1] | Indicator of the presence of a linked open line. Allowed values `Y` or `N` | Yes | No ||
    || **ASSIGNED_BY_ID**
    [`user`][1] | Identifier of the user responsible for the lead | Yes | Yes ||
    || **CREATED_BY_ID**
    [`user`][1] | Identifier of the user who created the lead | Yes | No ||
    || **MODIFY_BY_ID**
    [`user`][1] | Identifier of the author of the last change | Yes | No ||
    || **MOVED_BY_ID**
    [`user`][1] | Identifier of the author who moved the item to the current stage | Yes | No ||
    || **DATE_CREATE**
    [`datetime`][1] | Creation date | Yes | No ||
    || **DATE_MODIFY**
    [`datetime`][1] | Modification date | Yes | No ||
    || **MOVED_TIME**
    [`datetime`][1] | Date of moving the item to the current stage | Yes | No ||
    || **COMPANY_ID**
    [`crm_company`][2] | Linking the lead to a company (Field Client->Company) | Yes | Yes ||
    || **CONTACT_ID**
    [`crm_contact`][2] | Linking the lead to a contact. Deprecated field, currently not used. Kept for backward compatibility | Yes | Yes ||
    || **CONTACT_IDS**
    [`crm_contact`][2] | Identifier of the linked contact. Multiple. 
    
    When using [crm.lead.update](./leads/crm-lead-update.md) and [crm.lead.add](./leads/crm-lead-add.md), an array of contacts can be submitted | Yes | Yes ||
    || **IS_RETURN_CUSTOMER**
    [`char`][1] | Indicator of a repeat lead. Allowed values `Y` or `N` | Yes | No ||
    || **DATE_CLOSED**
    [`datetime`][1] | Closing date | Yes | No ||
    || **ORIGINATOR_ID**
    [`string`][1] | Identifier of the data source. Used only for linking to external sources | Yes | Yes ||
    || **ORIGIN_ID**
    [`string`][1] | Identifier of the item in the data source. Used only for linking to external sources | Yes | Yes ||
    || **UTM_SOURCE**
    [`string`][1] | Advertising system (Google-Adwords, etc.) | Yes | Yes ||
    || **UTM_MEDIUM**
    [`string`][1] | Type of traffic: CPC (ads), CPM (banners) | Yes | Yes ||
    || **UTM_CAMPAIGN**
    [`string`][1] | Designation of the advertising campaign | Yes | Yes ||
    || **UTM_CONTENT**
    [`string`][1] | Content of the campaign. For example, for contextual ads | Yes | Yes ||
    || **UTM_TERM**
    [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising | Yes | Yes ||
    || **LAST_ACTIVITY_TIME**
    [`datetime`][1] | Time of the last activity | Yes | No ||
    || **LAST_ACTIVITY_BY**
    [`string`][1] | Identifier of the user responsible for the last activity in this lead (for example, who created a new deal in the lead) | Yes | No ||
    || **PHONE**
    [`crm_multifield`][2] | Contact's phone. Multiple | Yes | Yes ||
    || **EMAIL**
    [`crm_multifield`][2] | E-mail address. Multiple | Yes | Yes ||
    || **WEB**
    [`crm_multifield`][2] | URLs of the lead. Multiple | Yes | Yes ||
    || **IM**
    [`crm_multifield`][2] | Messengers. Multiple | Yes | Yes ||
    || **LINK**
    [`crm_multifield`][2] | Links. Multiple. Service | Yes | Yes ||
    || **UF_CRM_xxx** | [User-defined fields](./leads/userfield/index.md) | Yes | Yes ||
    |#

- Companies

    The field description is returned by the method [crm.company.fields](./companies/crm-company-fields.md)

    #| 
    || **Name**
    `type` | **Description** | **Read** | **Write** ||
    || **ID**
    [`integer`][1] | Identifier of the company | Yes | No ||
    || **TITLE**
    [`string`][1] | Name. Required | Yes | Yes ||
    || **COMPANY_TYPE**
    [`crm_status`][2] | Type of company | Yes | Yes ||
    || **LOGO**
    [`file`][1] | Logo | Yes | Yes ||
    || **ADDRESS**
    [`string`][1] | Address of the company | Yes | Yes ||
    || **ADDRESS_2**
    [`string`][1] | Second line of the address. In some countries, it is customary to split the address into 2 parts | Yes | Yes ||
    || **ADDRESS_CITY**
    [`string`][1] | City | Yes | Yes ||
    || **ADDRESS_POSTAL_CODE**
    [`string`][1] | Postal code | Yes | Yes ||
    || **ADDRESS_REGION**
    [`string`][1] | Region | Yes | Yes ||
    || **ADDRESS_PROVINCE**
    [`string`][1] | Province | Yes | Yes ||
    || **ADDRESS_COUNTRY**
    [`string`][1] | Country | Yes | Yes ||
    || **ADDRESS_COUNTRY_CODE**
    [`string`][1] | Country code | Yes | Yes ||
    || **ADDRESS_LOC_ADDR_ID**
    [`integer`][1] | Identifier of the location address | Yes | Yes ||
    || **ADDRESS_LEGAL**
    [`string`][1] | Legal address | Yes | Yes ||
    || **REG_ADDRESS**
    [`string`][1] | Legal address of the company. Deprecated, used for compatibility | Yes | Yes ||
    || **REG_ADDRESS_2**
    [`string`][1] | Second line of the legal address. In some countries, it is customary to split the address into 2 parts. 
    Deprecated, used for compatibility | Yes | Yes ||
    || **REG_ADDRESS_CITY**
    [`string`][1] | City of the legal address. Deprecated, used for compatibility | Yes | Yes ||
    || **REG_ADDRESS_POSTAL_CODE**
    [`string`][1] | Postal code of the legal address. Deprecated, used for compatibility | Yes | Yes ||
    || **REG_ADDRESS_REGION**
    [`string`][1] | Region of the legal address. Deprecated, used for compatibility | Yes | Yes ||
    || **REG_ADDRESS_PROVINCE**
    [`string`][1] | Province of the legal address. Deprecated, used for compatibility | Yes | Yes ||
    || **REG_ADDRESS_COUNTRY**
    [`string`][1] | Country of the legal address. Deprecated, used for compatibility | Yes | Yes ||
    || **REG_ADDRESS_COUNTRY_CODE**
    [`string`][1] | Country code of the legal address. Deprecated, used for compatibility | Yes | Yes ||
    || **REG_ADDRESS_LOC_ADDR_ID**
    [`integer`][1] | Legal address identifier of the location address. Deprecated, used for compatibility | Yes | Yes ||
    || **BANKING_DETAILS**
    [`string`][1] | Banking details | Yes | Yes ||
    || **INDUSTRY**
    [`crm_status`][2] | Industry | Yes | Yes ||
    || **EMPLOYEES**
    [`crm_status`][2] | Number of employees | Yes | Yes ||
    || **CURRENCY_ID**
    [`crm_currency`][2] | Currency | Yes | Yes ||
    || **REVENUE**
    [`double`][1] | Annual revenue | Yes | Yes ||
    || **OPENED**
    [`char`][1] | Available to everyone | Yes | Yes ||
    || **COMMENTS**
    [`string`][1] | Comments | Yes | Yes ||
    || **HAS_PHONE**
    [`char`][1] | Check for the phone field being filled | Yes | No ||
    || **HAS_EMAIL**
    [`char`][1] | Check for the e-mail field being filled | Yes | No ||
    || **HAS_IMOL**
    [`char`][1] | Is there an open line | Yes | No ||
    || **IS_MY_COMPANY**
    [`char`][1] | My company | Yes | Yes ||
    || **ASSIGNED_BY_ID**
    [`user`][1] | Linked to user by ID | Yes | Yes ||
    || **CREATED_BY_ID**
    [`user`][1] | Who created the company | Yes | No ||
    || **MODIFY_BY_ID**
    [`user`][1] | Identifier of the author of the last change | Yes | No ||
    || **DATE_CREATE**
    [`datetime`][1] | Creation date | Yes | No ||
    || **DATE_MODIFY**
    [`datetime`][1] | Modification date | Yes | No ||
    || **CONTACT_ID**
    [`string`][1] | Contact. Used only for linking to external sources. | Yes | Yes ||
    || **LEAD_ID**
    [`crm_lead`][2] | Identifier of the lead associated with the company | Yes | No ||
    || **ORIGINATOR_ID**
    [`string`][1] | Identifier of the data source. Used only for linking to external sources | Yes | Yes ||
    || **ORIGIN_ID**
    [`string`][1] | Identifier of the item in the data source. Used only for linking to external sources | Yes | Yes ||
    || **ORIGIN_VERSION**
    [`string`][1] | Original version. Used to protect data from accidental overwriting by an external system. 
    
    If the data was imported and not changed in the external system, such data can be edited in CRM without fear that the next export will overwrite the data | Yes | Yes ||
    || **UTM_SOURCE**
    [`string`][1] | Advertising system (Google-Adwords, etc.) | Yes | Yes ||
    || **UTM_MEDIUM**
    [`string`][1] | Type of traffic: CPC (ads), CPM (banners) | Yes | Yes ||
    || **UTM_CAMPAIGN**
    [`string`][1] | Designation of the advertising campaign | Yes | Yes ||
    || **UTM_CONTENT**
    [`string`][1] | Content of the campaign. For example, for contextual ads | Yes | Yes ||
    || **UTM_TERM**
    [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising | Yes | Yes ||
    || **PARENT_ID_xxx**
    [`crm_entity`][2] | Relationship fields. 
    
    If there are SPAs related to contacts on the account, for each such SPA there is a field that stores the relationship between this SPA and the contact. The field itself stores the identifier of the item of that SPA. 
    
    For example, the field `PARENT_ID_153` — relationship with the SPA `entityTypeId=153`, stores the identifier of the item of this SPA related to the current contact | Yes | Yes ||
    || **LAST_ACTIVITY_TIME**
    [`datetime`][1] | Time of the last activity. | Yes | No ||
    || **LAST_ACTIVITY_BY**
    [`string`][1] | Identifier of the user responsible for the last activity in this lead (for example, who created a new deal in the lead) | Yes | No ||
    || **PHONE**
    [`crm_multifield`][2] | Phone of the company. Multiple | Yes | Yes ||
    || **EMAIL**
    [`crm_multifield`][2] | E-mail address. Multiple | Yes | Yes ||
    || **WEB**
    [`crm_multifield`][2] | URLs of the company. Multiple | Yes | Yes ||
    || **IM**
    [`crm_multifield`][2] | Messengers. Multiple | Yes | Yes ||
    || **LINK**
    [`crm_multifield`][2] | Links. Multiple. Service | Yes | Yes ||
    || **UF_CRM_xxx** | [User-defined fields](./companies/userfields/index.md) | Yes | Yes ||
    |#

- Contacts

    The field description is returned by the method [crm.contact.fields](./contacts/crm-contact-fields.md)

    #| 
    || **Name**
    `type` | **Description** | **Read** | **Write** ||
    ||**ID**
    [`integer`][1] | Identifier of the contact | Yes | No ||
    ||**HONORIFIC**
    [`crm_status`][2] | Addressing.

    Values from the directory can be obtained using the method [crm.status.list](./status/crm-status-list.md) with the filter by `ENTITY_ID=HONORIFIC` | Yes | Yes ||
    ||**NAME**
    [`string`][1] | First name | Yes | Yes ||
    ||**SECOND_NAME**
    [`string`][1] | Middle name | Yes | Yes ||
    ||**LAST_NAME**
    [`string`][1] | Last name | Yes | Yes ||
    ||**PHOTO**
    [`file`][1] | Photo | Yes | Yes ||
    ||**BIRTHDATE**
    [`date`][1] | Date of birth | Yes | Yes ||
    ||**TYPE_ID**
    [`crm_status`][2]| Type of contact.
    
    Values from the directory can be obtained using the method [crm.status.list](./status/crm-status-list.md) with the filter by `ENTITY_ID=CONTACT_TYPE` | Yes | Yes ||
    ||**SOURCE_ID**
    [`crm_status`][2] | Source.
    
    Values from the directory can be obtained using the method [crm.status.list](./status/crm-status-list.md) with the filter by `ENTITY_ID=SOURCE`| Yes | Yes ||
    ||**SOURCE_DESCRIPTION**
    [`string`][1] | Additional information about the source | Yes | Yes ||
    ||**POST**
    [`string`][1] | Position | Yes | Yes ||
    || | {% note tip "Deprecated fields" %}

    Address fields in the contact are deprecated and are only used in compatibility mode. To work with the address, use [requisites](./requisites/index.md).

    {% endnote %}
    | | ||
    ||**ADDRESS**
    [`string`][1] | Address (deprecated) | Yes | Yes ||
    ||**ADDRESS_2**
    [`string`][1] | Second line of the address (deprecated) | Yes | Yes ||
    ||**ADDRESS_CITY**
    [`string`][1] | City (deprecated) | Yes | Yes ||
    ||**ADDRESS_POSTAL_CODE**
    [`string`][1] | Postal code (deprecated) | Yes | Yes ||
    ||**ADDRESS_REGION**
    [`string`][1] | Region (deprecated) | Yes | Yes ||
    ||**ADDRESS_PROVINCE**
    [`string`][1] | Province (deprecated) | Yes | Yes ||
    ||**ADDRESS_COUNTRY**
    [`string`][1] | Country (deprecated) | Yes | Yes ||
    ||**ADDRESS_COUNTRY_CODE**
    [`string`][1] | Country code (deprecated) | Yes | Yes ||
    ||**ADDRESS_LOC_ADDR_ID**
    [`location`][1] | Identifier of the location address (deprecated) | Yes | Yes ||
    ||**COMMENTS**
    [`string`][1] | Comment. Supports bb-codes | Yes | Yes ||
    ||**OPENED**
    [`char`][1] | Available to everyone. Can take values `Y` or `N`. Considered in the work of access permissions for roles with the access level "All open" | Yes | Yes ||
    ||**EXPORT**
    [`char`][1] | Participates in the export of contacts. Can take values `Y` or `N`  | Yes | Yes ||
    ||**HAS_PHONE**
    [`char`][1] | Phone is set. Can take values `Y` or `N` | Yes | No ||
    ||**HAS_EMAIL**
    [`char`][1] | E-mail is set. Can take values `Y` or `N` | Yes | No ||
    ||**HAS_IMOL**
    [`char`][1] | Open line is set. Can take values `Y` or `N` | Yes | No ||
    ||**ASSIGNED_BY_ID**
    [`user`][1] | Responsible | Yes | Yes ||
    ||**CREATED_BY_ID**
    [`user`][1] | Who created | Yes | No ||
    ||**MODIFY_BY_ID**
    [`user`][1] | Who modified | Yes | No ||
    ||**DATE_CREATE**
    [`datetime`][1] | Creation date | Yes | No ||
    ||**DATE_MODIFY**
    [`datetime`][1] | Modification date | Yes | No ||
    ||**COMPANY_ID**
    [`crm_company`][2] | Main company of the contact | Yes | Yes ||
    ||**COMPANY_IDS**
    [`crm_company`][2] | Linking the contact to companies. Multiple. 
    
    In the methods [crm.contact.update](./contacts/crm-contact-update.md) and [crm.contact.add](./contacts/crm-contact-add.md), it is used to submit an array of companies. 
    
    In the methods [crm.contact.list](./contacts/crm-contact-list.md) and [crm.contact.get](./contacts/crm-contact-get.md), this field does not exist, and [crm.contact.company.items.get](./contacts/company/crm-contact-company-items-get.md) should be used to get the list of companies  | Yes | Yes ||
    ||**LEAD_ID**
    [`crm_lead`][2] | Identifier of the lead associated with the contact | Yes | No ||
    || | {% note tip "Fields for linking to external data sources" %}

    If the contact was created by an external system, then:
    - the field `ORIGINATOR_ID` stores the string identifier of that system
    - the field `ORIGIN_ID` stores the string identifier of the contact in that external system
    - the field `ORIGIN_VERSION` stores the version of the contact data in that external system

    {% endnote %} | | ||
    ||**ORIGINATOR_ID**
    [`string`][1] | Identifier of the external system that is the source of data about this contact | Yes | Yes ||
    ||**ORIGIN_ID**
    [`string`][1] | Identifier of the contact in the external system | Yes | Yes ||
    ||**ORIGIN_VERSION**
    [`string`][1] | Version of the contact data in the external system. Used to protect data from accidental overwriting by an external system. 
    
    If the data was imported and not changed in the external system, such data can be edited in CRM without fear that the next export will overwrite the data. | Yes | Yes ||
    ||**FACE_ID**
    [`integer`][1] | Link to faces from the `faceid` module | Yes | No ||
    ||**UTM_SOURCE**
    [`string`][1] | Advertising system (Google-Adwords, etc.) | Yes | Yes ||
    ||**UTM_MEDIUM**
    [`string`][1] | Type of traffic: CPC (ads), CPM (banners) | Yes | Yes ||
    ||**UTM_CAMPAIGN**
    [`string`][1] | Designation of the advertising campaign | Yes | Yes ||
    ||**UTM_CONTENT**
    [`string`][1] | Content of the campaign. For example, for contextual ads | Yes | Yes ||
    ||**UTM_TERM**
    [`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising | Yes | Yes ||
    ||**PARENT_ID_...** | Relationship fields. 
    
    If there are SPAs related to contacts on the account, for each such SPA there is a field that stores the relationship between this SPA and the contact. The field itself stores the identifier of the item of that SPA. 
    
    For example, the field `PARENT_ID_153` — relationship with the SPA `entityTypeId=153`, stores the identifier of the item of this SPA related to the current contact | Yes | Yes ||
    ||**LAST_ACTIVITY_TIME**
    [`datetime`][1] | Date of the last activity in the timeline | Yes | No ||
    ||**LAST_ACTIVITY_BY**
    [`user`][1] | Author of the last activity in the timeline | Yes | No ||
    ||**PHONE**
    [`crm_multifield`][2] | Phones. Multiple | Yes | Yes ||
    ||**EMAIL**
    [`crm_multifield`][2] | E-mail. Multiple | Yes | Yes ||
    ||**WEB**
    [`crm_multifield`][2] | Websites. Multiple | Yes | Yes ||
    ||**IM**
    [`crm_multifield`][2] | Messengers. Multiple | Yes | Yes ||
    ||**LINK**
    [`crm_multifield`][2] | Links. Multiple. Service | Yes | Yes ||
    ||**UF_CRM_xxx**  | User-defined fields. For example, `UF_CRM_25534736`. 
    
    Depending on the portal settings, contacts may have a set of user-defined fields of specific types. A user-defined field can be added to a contact using the method [crm.contact.userfield.add](./contacts/userfield/crm-contact-userfield-add.md)  ||
    |#

- Requisites

    ### General Requisites 
    
    The field description is returned by the method [crm.requisite.fields](./requisites/universal/crm-requisite-fields.md)

    #| 
    || **Name**
    `type` | **Description** | **Read** | **Write** ||
    || **ID**
    [`integer`][1] | Identifier of the requisite. 
    
    Can be obtained using the method [crm.requisite.list](./requisites/universal/crm-requisite-list.md). 
    
    Created automatically and unique within the portal | Yes | No ||
    || **ENTITY_TYPE_ID**
    [`integer`][1] | Identifier of the parent entity type. Currently, this can only be:
    - `3` — contact
    - `4` — company

    Identifiers of all CRM entity types are returned by the method [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) | Yes | Yes ||
    || **ENTITY_ID**
    [`integer`][1] | Identifier of the parent entity (contact or company).

    The identifier can be obtained using the method [crm.company.list](./companies/crm-company-list.md) for a company and the method [crm.contact.list](./contacts/crm-contact-list.md) for a contact | Yes | Yes ||
    || **PRESET_ID**
    [`integer`][1] | Identifier of the requisites template.

    Template identifiers can be obtained using the method [crm.requisite.preset.list](./requisites/presets/crm-requisite-preset-list.md) | Yes | Yes ||
    || **DATE_CREATE**
    [`datetime`][1] | Creation date | Yes | No ||
    || **DATE_MODIFY**
    [`datetime`][1] | Modification date | Yes | No ||
    || **CREATED_BY_ID**
    [`user`][1] | Identifier of the user who created the requisite | Yes | No ||
    || **MODIFY_BY_ID**
    [`user`][1] | Identifier of the user who modified the requisite | Yes | No ||
    || **NAME**
    [`string`][1] | Name of the requisite | Yes | Yes ||
    || **CODE**
    [`string`][1] | Symbolic code of the requisite | Yes | Yes ||
    || **XML_ID**
    [`string`][1] | External key, used for exchange operations.

    Identifier of the object in the external information database.

    The purpose of the field may change by the end developer | Yes | Yes ||
    || **ORIGINATOR_ID**
    [`string`][1] | Identifier of the external information database.

    The purpose of the field may change by the end developer | Yes | Yes ||
    || **ACTIVE**
    [`char`][1] | Activity indicator.

    Values `Y` or `N` are used.

    Currently, the field does not actually affect anything | Yes | Yes ||
    || **ADDRESS_ONLY**
    [`char`][1] | Indicator of the state when the requisite is used only for storing the address.

    Values `Y` or `N` are used. When set to `Y`, the requisites are not displayed in the entity card, but the address is displayed | Yes | Yes ||
    || **SORT**
    [`integer`][1] | Sorting. Order in the list of entity requisites when there are multiple | Yes | Yes ||
    || **RQ_NAME**
    [`string`][1] | Full name | Yes | Yes ||
    || **RQ_FIRST_NAME**
    [`string`][1] | First name | Yes | Yes ||
    || **RQ_LAST_NAME**
    [`string`][1] | Last name | Yes | Yes ||
    || **RQ_SECOND_NAME**
    [`string`][1] | Middle name | Yes | Yes ||
    || **RQ_COMPANY_ID**
    [`string`][1] | Identifier of the organization | Yes | Yes ||
    || **RQ_COMPANY_NAME**
    [`string`][1] | Short name of the organization | Yes | Yes ||
    || **RQ_COMPANY_FULL_NAME**
    [`string`][1] | Full name of the organization | Yes | Yes ||
    || **RQ_COMPANY_REG_DATE**
    [`string`][1] | Date of state registration | Yes | Yes ||
    || **RQ_DIRECTOR**
    [`string`][1] | General director | Yes | Yes ||
    || **RQ_ACCOUNTANT**
    [`string`][1] | Chief accountant | Yes | Yes ||
    || **RQ_CEO_NAME**
    [`string`][1] | Full name of the first leader | Yes | Yes ||
    || **RQ_CEO_WORK_POS**
    [`string`][1] | Position of the first leader | Yes | Yes ||
    || **RQ_CONTACT**
    [`string`][1] | Contact person | Yes | Yes ||
    || **RQ_EMAIL**
    [`string`][1] | E-Mail | Yes | Yes ||
    || **RQ_PHONE**
    [`string`][1] | Phone | Yes | Yes ||
    || **RQ_FAX**
    [`string`][1] | Fax | Yes | Yes ||
    || **RQ_IDENT_TYPE**
    [`crm_status`][2] | Method of identification | Yes | Yes ||
    || **RQ_IDENT_DOC**
    [`string`][1] | Type of document | Yes | Yes ||
    || **RQ_IDENT_DOC_SER**
    [`string`][1] | Series | Yes | Yes ||
    || **RQ_IDENT_DOC_NUM**
    [`string`][1] | Number | Yes | Yes ||
    || **RQ_IDENT_DOC_PERS_NUM**
    [`string`][1] | Personal number | Yes | Yes ||
    || **RQ_IDENT_DOC_DATE**
    [`string`][1] | Date of issue | Yes | Yes ||
    || **RQ_IDENT_DOC_ISSUED_BY**
    [`string`][1] | Issued by | Yes | Yes ||
    || **RQ_IDENT_DOC_DEP_CODE**
    [`string`][1] | Department code | Yes | Yes ||
    || **RQ_INN**
    [`string`][1] | Taxpayer identification number | Yes | Yes ||
    || **RQ_KPP**
    [`string`][1] | Tax registration reason code | Yes | Yes ||
    || **RQ_USRLE**
    [`string`][1] | Commercial register number (for country DE) | Yes | Yes ||
    || **RQ_IFNS**
    [`string`][1] | Tax authority | Yes | Yes ||
    || **RQ_OGRN**
    [`string`][1] | Primary state registration number | Yes | Yes ||
    || **RQ_OGRNIP**
    [`string`][1] | Primary state registration number for individual entrepreneurs | Yes | Yes ||
    || **RQ_OKPO**
    [`string`][1] | National classification of enterprises and organizations | Yes | Yes ||
    || **RQ_OKTMO**
    [`string`][1] | Municipal classification of territories | Yes | Yes ||
    || **RQ_OKVED**
    [`string`][1] | All-Russian classifier of economic activities | Yes | Yes ||
    || **RQ_EDRPOU**
    [`string`][1] | Unified State Register of Enterprises and Organizations | Yes | Yes ||
    || **RQ_DRFO**
    [`string`][1] | Taxpayer registration number | Yes | Yes ||
    || **RQ_KBE**
    [`string`][1] | Classification of economic activities | Yes | Yes ||
    || **RQ_IIN**
    [`string`][1] | Individual identification number | Yes | Yes ||
    || **RQ_BIN**
    [`string`][1] | Business identification number | Yes | Yes ||
    || **RQ_ST_CERT_SER**
    [`string`][1] | Series of the state registration certificate | Yes | Yes ||
    || **RQ_ST_CERT_NUM**
    [`string`][1] | Number of the state registration certificate | Yes | Yes ||
    || **RQ_ST_CERT_DATE**
    [`string`][1] | Date of the state registration certificate | Yes | Yes ||
    || **RQ_VAT_PAYER**
    [`char`][1] | VAT payer (for country UA).

    Values `Y` or `N` are used | Yes | Yes ||
    || **RQ_VAT_ID**
    [`string`][1] | VAT ID (identification number of the VAT payer) | Yes | Yes ||
    || **RQ_VAT_CERT_SER**
    [`string`][1] | Series of the VAT certificate | Yes | Yes ||
    || **RQ_VAT_CERT_NUM**
    [`string`][1] | Number of the VAT certificate | Yes | Yes ||
    || **RQ_VAT_CERT_DATE**
    [`string`][1] | Date of the VAT certificate | Yes | Yes ||
    || **RQ_RESIDENCE_COUNTRY**
    [`string`][1] | Country of residence | Yes | Yes ||
    || **RQ_BASE_DOC**
    [`string`][1] | Basis for action | Yes | Yes ||
    || **RQ_REGON**
    [`string`][1] | REGON (for country PL) | Yes | Yes ||
    || **RQ_KRS**
    [`string`][1] | KRS (for country PL) | Yes | Yes ||
    || **RQ_PESEL**
    [`string`][1] | PESEL (for country PL) | Yes | Yes ||
    || **RQ_LEGAL_FORM**
    [`string`][1] | Legal form (for country FR) | Yes | Yes ||
    || **RQ_SIRET**
    [`string`][1] | SIRET number (for country FR) | Yes | Yes ||
    || **RQ_SIREN**
    [`string`][1] | SIREN number (for country FR) | Yes | Yes ||
    || **RQ_CAPITAL**
    [`string`][1] | Share capital (for country FR) | Yes | Yes ||
    || **RQ_RCS**
    [`string`][1] | RCS (for country FR) | Yes | Yes ||
    || **RQ_CNPJ**
    [`string`][1] | CNPJ (for country BR) | Yes | Yes ||
    || **RQ_STATE_REG**
    [`string`][1] | State registration (IE) (for country BR) | Yes | Yes ||
    || **RQ_MNPL_REG**
    [`string`][1] | Municipal registration (IM) (for country BR) | Yes | Yes ||
    || **RQ_CPF**
    [`string`][1] | CPF (for country BR) | Yes | Yes ||
    || **UF_CRM_...** | User-defined fields. For example, `UF_CRM_1694526604`.

    Requisites may have a set of user-defined fields with types: `string`, `boolean`, `double`, `datetime`.

    A user-defined field can be added to requisites using the method [crm.requisite.userfield.add](./requisites/user-fields/crm-requisite-userfield-add.md) | Yes | Yes ||
    |#

    ### Bank Details 
    
    The field description is returned by the method [crm.requisite.bankdetail.fields](./requisites/bank-detail/crm-requisite-bank-detail-fields.md)

    #| 
    || **Name**
    `type` | **Description** | **Read** | **Write** ||
    || **ID**
    [`integer`][1] | Identifier of the bank detail. Created automatically and unique within the portal | Yes | No ||
    || **ENTITY_TYPE_ID**
    [`integer`][1] | Identifier of the parent object type. Can only be `Requisite` (value `8`).

    Identifiers of object types are returned by the method [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) | Yes | No ||
    || **ENTITY_ID**
    [`integer`][1] | Identifier of the parent object | Yes | Yes ||
    || **COUNTRY_ID**
    [`integer`][1] | Identifier of the country corresponding to the set of bank detail fields (see method [crm.requisite.preset.countries](./requisites/presets/crm-requisite-preset-countries.md) for available values).

    The country code of the bank detail matches the country code in the linked requisites template, the identifier of which is specified in the field `ENTITY_ID` | Yes | Yes ||
    || **DATE_CREATE**
    [`datetime`][1] | Creation date | Yes | No ||
    || **DATE_MODIFY**
    [`datetime`][1] | Modification date | Yes | No ||
    || **CREATED_BY_ID**
    [`user`][1] | Identifier of the user who created the requisite | Yes | No ||
    || **MODIFY_BY_ID**
    [`user`][1] | Identifier of the user who modified the requisite | Yes | No ||
    || **NAME^*^**
    [`string`][1] | Name of the bank detail | Yes | Yes ||
    || **CODE**
    [`string`][1] | Symbolic code of the requisite | Yes | Yes ||
    || **XML_ID**
    [`string`][1] | External key. Used for exchange operations. Identifier of the object in the external information database.

    The purpose of the field may change by the end developer. Each application ensures the uniqueness of values in this field.

    It is recommended to use a unique prefix to avoid collisions with other applications | Yes | Yes ||
    || **ACTIVE**
    [`char`][1] | Activity indicator. Values `Y` or `N` are used.

    Currently, the field does not actually affect anything | Yes | Yes ||
    || **SORT**
    [`integer`][1] | Sorting | Yes | Yes ||
    || **RQ_BANK_NAME**
    [`string`][1] | Name of the bank | Yes | Yes ||
    || **RQ_BANK_ADDR**
    [`string`][1] | Address of the bank | Yes | Yes ||
    || **RQ_BANK_CODE**
    [`string`][1] | Bank code (for country BR) | Yes | Yes ||
    || **RQ_BANK_ROUTE_NUM**
    [`string`][1] | Bank Routing Number | Yes | Yes ||
    || **RQ_BIK**
    [`string`][1] | BIK | Yes | Yes ||
    || **RQ_CODEB**
    [`string`][1] | Bank Code (for country FR) | Yes | Yes ||
    || **RQ_CODEG**
    [`string`][1] | Branch Code (for country FR) | Yes | Yes ||
    || **RQ_RIB**
    [`string`][1] | RIB Key (for country FR) | Yes | Yes ||
    || **RQ_MFO**
    [`string`][1] | MFO | Yes | Yes ||
    || **RQ_ACC_NAME**
    [`string`][1] | Bank Account Holder Name | Yes | Yes ||
    || **RQ_ACC_NUM**
    [`string`][1] | Bank Account Number | Yes | Yes ||
    || **RQ_ACC_TYPE**
    [`string`][1] | Account Type (for country BR) | Yes | Yes ||
    || **RQ_AGENCY_NAME**
    [`string`][1] | Agency (for country BR) | Yes | Yes ||
    || **RQ_IIK**
    [`string`][1] | IIK | Yes | Yes ||
    || **RQ_ACC_CURRENCY**
    [`string`][1] | Currency of the account | Yes | Yes ||
    || **RQ_COR_ACC_NUM**
    [`string`][1] | Correspondent account | Yes | Yes ||
    || **RQ_IBAN**
    [`string`][1] | IBAN | Yes | Yes ||
    || **RQ_SWIFT**
    [`string`][1] | SWIFT | Yes | Yes ||
    || **RQ_BIC**
    [`string`][1] | BIC | Yes | Yes ||
    || **COMMENTS**
    [`string`][1] | Comment | Yes | Yes ||
    || **ORIGINATOR_ID**
    [`string`][1] | Identifier of the external information database. The purpose of the field may change by the end developer | Yes | Yes ||
    |#

    ### Templates of Requisites 
    
    The field description is returned by the method [crm.requisite.preset.fields](./requisites/presets/crm-requisite-preset-fields.md)

    #| 
    || **Name** | **Description** | **Read** | **Write** ||
    || **ID**
    [`integer`][1] | Identifier of the requisite. Created automatically and unique within the portal | Yes | No ||
    || **ENTITY_TYPE_ID**
    [`integer`][1] | Identifier of the parent object type.

    Identifiers of CRM object types are returned by the method [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) | Yes | Yes ||
    || **COUNTRY_ID**
    [`integer`][1] | Identifier of the country corresponding to the set of fields of the requisites template (for available values see method [crm.requisite.preset.countries](./requisites/presets/crm-requisite-preset-countries.md)) | Yes | Yes ||
    || **DATE_CREATE**
    [`datetime`][1] | Creation date | Yes | No ||
    || **DATE_MODIFY**
    [`datetime`][1] | Modification date. Contains an empty string if the template has not been changed since creation | Yes | No ||
    || **CREATED_BY_ID**
    [`user`][1] | Identifier of the user who created the requisite | Yes | No ||
    || **MODIFY_BY_ID**
    [`user`][1] | Identifier of the user who modified the requisite | Yes | No ||
    || **NAME**
    [`string`][1] | Name of the requisite | Yes | Yes ||
    || **XML_ID**
    [`string`][1] | External key. Used for exchange operations. Identifier of the object in the external information database.

    The purpose of the field may change by the end developer.

    Each application ensures the uniqueness of values in this field. It is recommended to use a unique prefix to avoid collisions with other applications.

    Values of the form `#CRM_REQUISITE_PRESET_DEF_...` are reserved in CRM for identifying templates that are used by default. These identifiers should not be used for your purposes, as this may lead to a violation of logic | Yes | Yes ||
    || **ACTIVE**
    [`char`][1] | Activity indicator. Values `Y` or `N` are used. Determines the availability of the template in the selection list when adding requisites | Yes | Yes ||
    || **SORT**
    [`integer`][1] | Sorting | Yes | Yes ||
    |#

    ### Fields of Requisites Templates 
    
    The field description is returned by the method [crm.requisite.preset.field.fields](./requisites/presets/fields/crm-requisite-preset-field-fields.md)

    #| 
    ||  **Name**
    `type` | **Description** | **Read** | **Write** ||
    || **ID**
    [`integer`][1] | Identifier of the field. Created automatically and unique within the template | Yes | No ||
    || **FIELD_NAME**
    [`string`][1] | Name of the field | Yes | Yes ||
    || **FIELD_TITLE**
    [`string`][1] | Alternative name of the field for the requisite.

    The alternative name is displayed in various forms for filling requisites. Depending on the specific form, the alternative name may or may not be used | Yes | Yes ||
    || **SORT**
    [`integer`][1] | Sorting. Order in the list of template fields | Yes | Yes ||
    || **IN_SHORT_LIST**
    [`char`][1] | Show in the short list. Deprecated field, currently not used. Kept for backward compatibility. Can take values `Y` or `N` | Yes | Yes ||
    |#

    ### Addresses of Requisites 
    
    The field description is returned by the method [crm.address.fields](./requisites/addresses/crm-address-fields.md)

    #| 
    || **Name** | **Description** | **Read** | **Write** ||
    || **TYPE_ID**
    [`integer`][1] | Identifier of the address type. Element of the enumeration "Address Type".

    Elements of the enumeration "Address Type" can be obtained using the method [crm.enum.addresstype](./auxiliary/enum/crm-enum-address-type.md) | Yes | Yes  ||
    || **ENTITY_TYPE_ID**
    [`integer`][1] | Identifier of the parent object type.

    Identifiers of object types can be obtained using the method [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md).
    
    {% note tip "" %}

    Addresses can only be linked to Requisites (and requisites to companies or contacts) or Leads. For backward compatibility, the ability to link Addresses to Contacts or Companies has been left. However, this link is only possible on some old portals where the old address handling mode was specifically enabled by technical support.

    {% endnote %} | Yes | Yes ||
    || **ENTITY_ID**
    [`string`][1] | Identifier of the parent object | Yes | Yes ||
    || **ADDRESS_1**
    [`string`][1] | Street, house, building | Yes | Yes ||
    || **ADDRESS_2**
    [`string`][1] | Apartment / office | Yes | Yes ||
    || **CITY**
    [`string`][1] | City | Yes | Yes ||
    || **POSTAL_CODE**
    [`string`][1] | Postal code | Yes | Yes ||
    || **REGION**
    [`string`][1] | Region | Yes | Yes ||
    || **PROVINCE**
    [`string`][1] | Province | Yes | Yes ||
    || **COUNTRY**
    [`string`][1] | Country | Yes | Yes ||
    || **COUNTRY_CODE**
    [`string`][1] | Country code | Yes | Yes ||
    || **LOC_ADDR_ID**
    [`integer`][1] | Identifier of the location address.

    This field contains the identifier of the address object in the `Location` module, linked to the CRM address object. Each CRM address corresponds to an address object in the `location` module. This can be used to copy an existing address in CRM with location information that is not in the CRM address fields.

    If the identifier of the `location` module address is specified when creating an address, a copy of the `location` address is created and linked to the created CRM address. If in this case no values are specified for the string address fields, they will be filled from the location address.

    If at least one string field was specified, then only the specified fields will be saved in the CRM address, and their values will overwrite the corresponding values in the location address object. The same behavior will occur when updating the address | Yes | Yes ||
    || **ANCHOR_TYPE_ID**
    [`integer`][1] | Identifier of the type of the main parent object.

    This field is for service use. The value is automatically filled when adding an address.

    Identifiers of object types can be obtained using the method [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md).

    This field contains the identifier of the type of the parent requisite object (company or contact) if the address is linked to the requisite. If the address is linked to a lead, then this value will be the identifier of the lead | Yes | No ||
    || **ANCHOR_ID**
    [`integer`][1] | This field is for service use. The value is automatically filled when adding an address.

    This field contains the identifier of the parent requisite object (company or contact) if the address is linked to the requisite. If the address is linked to a lead, then this value will be the identifier of the lead | Yes | No ||
    |#

- Activities

    The field description is returned by the method [crm.activity.fields](./timeline/activities/crm-activity-fields.md)

    #| 
    || **Name** | **Description** | **Read** | **Write** ||
    || **ID**
    [`integer`][1] | Identifier of the activity | Yes | No ||
    || **OWNER_ID**
    [`integer`][1] | Identifier of the owner, immutable | Yes | Yes ||
    || **OWNER_TYPE_ID**
    [`crm.enum.ownertype`][2] | Type of owner, immutable | Yes | Yes ||
    || **TYPE_ID**
    [`crm_enum_activitytype`][2] | Type, immutable | Yes | Yes ||
    || **PROVIDER_ID**
    [`string`][1] | Identifier of the provider | Yes | Yes ||
    || **PROVIDER_TYPE_ID**
    [`string`][1] | Identifier of the provider type | Yes | Yes ||
    || **PROVIDER_GROUP_ID**
    [`string`][1] | Type of connector | Yes | Yes ||
    || **ASSOCIATED_ENTITY_ID**
    [`integer`][1] | Identifier of the entity associated with the activity | Yes | No ||
    || **SUBJECT**
    [`string`][1] | Subject, title of the activity | Yes | Yes ||
    || **START_TIME**
    [`datetime`][1] | Start time of execution | Yes | Yes ||
    || **END_TIME**
    [`datetime`][1] | End time | Yes | Yes ||
    || **DEADLINE**
    [`datetime`][1] | Deadline. The field is not set directly; the value is taken from `START_TIME` for calls and meetings and from `END_TIME` for tasks | Yes | Yes ||
    || **COMPLETED**
    [`char`][1] | Completed | Yes | Yes ||
    || **STATUS**
    [`crm_enum_activitystatus`][2] | Status | Yes | Yes ||
    || **RESPONSIBLE_ID**
    [`user`][1] | Responsible | Yes | Yes ||
    || **PRIORITY**
    [`crm.enum.activitypriority`][2] | Importance | Yes | Yes ||
    || **NOTIFY_TYPE**
    [`crm.enum.activitynotifytype`][2] | Type of notifications | Yes | Yes ||
    || **NOTIFY_VALUE**
    [`integer`][1] | Notification parameter | Yes | Yes ||
    || **DESCRIPTION**
    [`string`][1] | Description | Yes | Yes ||
    || **DESCRIPTION_TYPE**
    [`crm.enum.contenttype`][2] | Type of description | Yes | Yes ||
    || **DIRECTION**
    [`crm.enum.activitydirection`][2] | Direction of the activity: incoming/outgoing. Relevant for calls and emails, not used for meetings | Yes | Yes ||
    || **LOCATION**
    [`string`][1] | Location | Yes | Yes ||
    || **CREATED**
    [`datetime`][1] | Creation date | Yes | No ||
    || **AUTHOR_ID**
    [`user`][1] | Creator of the activity | Yes | Yes ||
    || **LAST_UPDATED**
    [`datetime`][1] | Date of the last update | Yes | No ||
    || **EDITOR_ID**
    [`user`][1] | Who modified | Yes | No ||
    || **SETTINGS**
    [`object`][1] | Settings | Yes | Yes ||
    || **ORIGIN_ID**
    [`string`][1] | Identifier of the item in the data source. Used only for linking to external sources | Yes | Yes ||
    || **ORIGINATOR_ID**
    [`string`][1] | Identifier of the data source. Used only for linking to external sources | Yes | Yes ||
    || **RESULT_STATUS**
    [`integer`][1] | Unused field, remains for compatibility | Yes | Yes ||
    || **RESULT_STREAM**
    [`integer`][1] | Report statistics | Yes | Yes ||
    || **RESULT_SOURCE_ID**
    [`string`][1] | Unused field, remains for compatibility | Yes | Yes ||
    || **PROVIDER_PARAMS**
    [`object`][1] | Provider parameters | Yes | Yes ||
    || **PROVIDER_DATA**
    [`string`][1] | Provider data | Yes | Yes ||
    || **RESULT_MARK**
    [`integer`][1] | Unused field, remains for compatibility | Yes | Yes ||
    || **RESULT_VALUE**
    [`double`][1] | Unused field, remains for compatibility | Yes | Yes ||
    || **RESULT_SUM**
    [`double`][1] | Unused field, remains for compatibility | Yes | Yes ||
    || **RESULT_CURRENCY_ID**
    [`string`][1] | Unused field, remains for compatibility | Yes | Yes ||
    || **AUTOCOMPLETE_RULE**
    [`integer`][1] | Autocomplete | Yes | Yes ||
    || **BINDINGS**
    [`crm_activity_binding`][2] | Bindings | Yes | No ||
    || **COMMUNICATIONS**
    [`crm_activity_communication`][2] | Communication channel. Multiple, required | Yes | Yes ||
    || **FILES**
    [`diskfile`][1] | Added files. Multiple | Yes | Yes ||
    || **WEBDAV_ELEMENTS**
    [`diskfile`][1] | Added files. Multiple. Deprecated, kept for compatibility | Yes | Yes ||
    || **IS_INCOMING_CHANNEL**
    [`char`][1] | Is the activity incoming, that is, created as a result of an incoming client request in the communication channel | Yes | No ||
    |#

{% endlist %}

[1]: ../data-types.md
[2]: ./data-types.md