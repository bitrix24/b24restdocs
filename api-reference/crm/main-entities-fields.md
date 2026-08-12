# Fields of Main CRM Objects

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> **Attention!** A more complete list of fields can be found on the pages of methods that return the description of object fields. Such methods are named **crm.object_name.fields**.

## Deals

The field description is returned by the method [crm.deal.fields](./deals/crm-deal-fields.md)

#|
|| **Name**
`type` | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`][1] | Deal identifier | Yes | No ||
|| **TITLE**
[`string`][1] | Name | Yes | Yes ||
|| **TYPE_ID**
[`crm_status`][2] | Deal type. Used only for linking to an external source. | Yes | Yes ||
|| **CATEGORY_ID**
[`crm_category`][1] | Direction identifier. Immutable. If this field is not passed when creating a deal, the deal will be created in the general direction. | Yes | Yes ||
|| **STAGE_ID**
[`crm_status`][2] | Stage identifier. Possible values:
- `NEW` — new deal
- `PREPARATION` — document preparation
- `PREPAYMENT_INVOICE` — invoice sending
- `EXECUTING` — in progress
- `FINAL_INVOICE` — final invoice
- `WON` — won
- `>LOSE` — lost, no reason analysis required
- `APOLOGY` — lost, reason analysis required | Yes | Yes ||
|| **STAGE_SEMANTIC_ID**
[`string`][1] | Name. Read-only. In a sense, generalizes the values of the deal identifier `STAGE_ID`:
- `P` — for stages with identifiers `NEW`, `PREPARATION`, `PREPAYMENT_INVOICE`, `EXECUTING`, and `FINAL_INVOICE`
- `S` — for the stage with identifier `WON`
- `F` — for stages with identifiers `>LOSE` and `APOLOGY` | Yes | No ||
|| **IS_NEW**
[`char`][1] | New deal flag (deals in the first stage) | Yes | No ||
|| **IS_RECURRING**
[`char`][1] | Recurring deal template flag. If `Y` is set, then it is a template, not a deal | Yes | Yes ||
|| **IS_RETURN_CUSTOMER**
[`char`][1] | Repeat lead indicator | Yes | Yes ||
|| **IS_REPEATED_APPROACH**
[`char`][1] | Repeat Salutation | Yes | Yes ||
|| **PROBABILITY**
[`integer`][1] | Probability | Yes | Yes ||
|| **CURRENCY_ID**
[`crm_currency`][2] | Deal currency identifier | Yes | Yes ||
|| **OPPORTUNITY**
[`double`][1] | Amount | Yes | Yes ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`][1] | Repeat Salutation | Yes | Yes ||
|| **TAX_VALUE**
[`double`][1] | Tax rate | Yes | Yes ||
|| **COMPANY_ID**
[`crm_company`][2] | Linked company identifier | Yes | Yes ||
|| **CONTACT_ID**
[`crm_contact`][2] | Linked contact identifier. Deprecated. Retained for compatibility | Yes | Yes ||
|| **CONTACT_IDS**
[`crm_contact`][2] | Linked contact identifier. Multiple.

When using [crm.deal.update](./deals/crm-deal-update.md) and [crm.deal.add](./deals/crm-deal-add.md), you can pass an array of contacts.

In the [crm.deal.list](./deals/crm-deal-list.md) and [crm.deal.get](./deals/crm-deal-get.md) methods, this field does not exist, and you must use [crm.deal.contact.items.get](./deals/contacts/crm-deal-contact-items-get.md) to retrieve the list of contacts.

To clear the field, use [crm.deal.contact.items.delete](./deals/contacts/crm-deal-contact-items-delete.md); to replace the value, use [crm.deal.contact.items.set](./deals/contacts/crm-deal-contact-items-set.md) | Yes | Yes ||
|| **QUOTE_ID**
[`crm_quote`][2] | Quote identifier. Read-only. Deprecated. Use the [crm.quote.list](./quote/crm-quote-list.md) method with a filter by deal | Yes | No ||
|| **BEGINDATE**
[`date`][1] | Start date | Yes | Yes ||
|| **CLOSEDATE**
[`date`][1] | End date | Yes | Yes ||
|| **OPENED**
[`char`][1] | Available to all | Yes | Yes ||
|| **CLOSED**
[`char`][1] | Is the deal completed | Yes | Yes ||
|| **COMMENTS**
[`string`][1] | Comments | Yes | Yes ||
|| **ASSIGNED_BY_ID**
[`user`][1] | Linked to user by ID | Yes | Yes ||
|| **CREATED_BY_ID**
[`user`][1] | Created by user | Yes | No ||
|| **MODIFY_BY_ID**
[`user`][1] | Last change author identifier | Yes | No ||
|| **MOVED_BY_ID**
[`user`][1] | Author identifier who moved the item to the current stage | Yes | No ||
|| **DATE_CREATE**
[`datetime`][1] | Create date | Yes | No ||
|| **DATE_MODIFY**
[`datetime`][1] | Change date | Yes | No ||
|| **MOVED_TIME**
[`datetime`][1] | Date the item was moved to the current stage | Yes | No ||
|| **SOURCE_ID**
[`string`][1] | Source identifier. Determines the source of the deal (callback, advertisement, Email, etc.).

A list of possible identifiers can be obtained using the [crm.status.list](./status/crm-status-list.md) method with the `filter[ENTITY_ID]=SOURCE` filter. | Yes | Yes ||
|| **SOURCE_DESCRIPTION**
[`string`][1] | Additional information about the source. Text field. | Yes | Yes ||
|| **ADDITIONAL_INFO**
[`string`][1] | Additional information. | Yes | Yes ||
|| **LEAD_ID**
[`crm_lead`][2] | Linked lead identifier. | Yes | No ||
|| **LOCATION_ID**
[`location`][1] | Customer location. Service field, not recommended for use. | Yes | Yes ||
|| **ORIGINATOR_ID**
[`string`][1] | Data source identifier. Used only for linking to an external source. | Yes | Yes ||
|| **ORIGIN_ID**
[`string`][1] | Item identifier in the data source. Used only for linking to an external source. | Yes | Yes ||
|| **UTM_SOURCE**
[`string`][1] | Ad system (Search Ads, Display Ads, and others). | Yes | Yes ||
|| **UTM_MEDIUM**
[`string`][1] | Traffic type: CPC (ads), CPM (banners). | Yes | Yes ||
|| **UTM_CAMPAIGN**
[`string`][1] | Advertising campaign designation. | Yes | Yes ||
|| **UTM_CONTENT**
[`string`][1] | Campaign contents. For example, for contextual ads. | Yes | Yes ||
|| **UTM_TERM**
[`string`][1] | Campaign search condition. For example, contextual advertising keywords. | Yes | Yes ||
|| **PARENT_ID_xxx**
[`crm_entity`][2] | Relation fields.

If there are SPAs on the portal linked to contacts, then for each such SPA, there is a field that stores the connection between this SPA and the contact. The field itself stores the item identifier of that SPA.

For example, the `PARENT_ID_153` field — a connection with SPA `entityTypeId=153`, stores the item identifier of this SPA linked to the current contact. | Yes | Yes ||
|| **LAST_ACTIVITY_BY**
[`string`][1] | Identifier of the user assigned to the last activity in this lead (for example, the one who created a new activity in the lead). | Yes | Yes ||
|| **LAST_ACTIVITY_TIME**
[`datetime`][1] | Last activity time. | Yes | Yes ||
|| **UF_CRM_xxx** | [Custom fields](./deals/user-defined-fields/index.md) | Yes | Yes ||
|#

## Leads

The field description is returned by the method [crm.lead.fields](./leads/crm-lead-fields.md)

#|
|| **Name**
`type` | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`][1] | Integer lead identifier. | Yes | No ||
|| **TITLE**
[`string`][1] | Lead name. | Yes | Yes ||
|| **HONORIFIC**
[`crm_status`][2] | Salutation. Status from the directory.

A list of possible identifiers can be obtained using the [crm.status.list](./status/crm-status-list.md) method with the `filter[ENTITY_ID]=HONORIFIC` filter. | Yes | Yes ||
|| **NAME**
[`string`][1] |  Contact first name. | Yes | Yes ||
|| **SECOND_NAME**
[`string`][1] |  Contact middle name. | Yes | Yes ||
|| **LAST_NAME**
[`string`][1] |  Contact last name. | Yes | Yes ||
|| **BIRTHDATE**
[`date`][1] | Date of birth. | Yes | Yes ||
|| **COMPANY_TITLE**
[`string`][1] | Name of the company linked to the lead. | Yes | Yes ||
|| **SOURCE_ID**
[`crm_status`][2] | Source identifier. Status from the directory.

A list of possible identifiers can be obtained using the [crm.status.list](./status/crm-status-list.md) method with the `filter[ENTITY_ID]=SOURCE` filter. | Yes | Yes ||
|| **SOURCE_DESCRIPTION**
[`string`][1] | Source description. | Yes | Yes ||
|| **STATUS_ID**
[`crm_status`][2] | Lead stage identifier. Status from the directory.

A list of possible identifiers can be obtained using the [crm.status.list](./status/crm-status-list.md) method with the `filter[ENTITY_ID]=STATUS` filter. | Yes | Yes ||
|| **STATUS_DESCRIPTION**
[`string`][1] | Additional information about the stage. | Yes | Yes ||
|| **STATUS_SEMANTIC_ID**
[`string`][1] | Status. Possible values:
- `F` (failed) — processed unsuccessfully
- `S` (success) — processed successfully
- `P` (processing) — lead is being processed. | Yes | No ||
|| **POST**
[`string`][1] | Job title. | Yes | Yes ||
|| **ADDRESS**
[`string`][1] | Contact address. | Yes | Yes ||
|| **ADDRESS_2**
[`string`][1] | Address line 2. In some countries, it is common to split the address into 2 parts. | Yes | Yes ||
|| **ADDRESS_CITY**
[`string`][1] | City | Yes | Yes ||
|| **ADDRESS_POSTAL_CODE**
[`string`][1] | Postal code | Yes | Yes ||
|| **ADDRESS_REGION**
[`string`][1] | District | Yes | Yes ||
|| **ADDRESS_PROVINCE**
[`string`][1] | Region | Yes | Yes ||
|| **ADDRESS_COUNTRY**
[`string`][1] | Country | Yes | Yes ||
|| **ADDRESS_COUNTRY_CODE**
[`string`][1] | Country code | Yes | Yes ||
|| **ADDRESS_LOC_ADDR_ID**
[`integer`][1] | Address identifier from the locations module | Yes | Yes ||
|| **CURRENCY_ID**
[`crm_currency`][2] | Currency identifier | Yes | Yes ||
|| **OPPORTUNITY**
[`double`][1] | Estimated amount | Yes | Yes ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`][1] | Manual amount calculation flag. Allowed values `Y` or `N` | Yes | Yes ||
|| **OPENED**
[`char`][1] | Available to all. Allowed values `Y` or `N` | Yes | Yes ||
|| **COMMENTS**
[`string`][1] | Comments | Yes | Yes ||
|| **HAS_PHONE**
[`char`][1] | `phone` field completion flag. Allowed values `Y` or `N` | Yes | No ||
|| **HAS_EMAIL**
[`char`][1] | Email field completion flag. Allowed values `Y` or `N` | Yes | No ||
|| **HAS_IMOL**
[`char`][1] | Linked Open Channel presence flag. Allowed values `Y` or `N` | Yes | No ||
|| **ASSIGNED_BY_ID**
[`user`][1] | Identifier of the user assigned to the lead | Yes | Yes ||
|| **CREATED_BY_ID**
[`user`][1] | Identifier of the user who created the lead | Yes | No ||
|| **MODIFY_BY_ID**
[`user`][1] | Last change author identifier | Yes | No ||
|| **MOVED_BY_ID**
[`user`][1] | Identifier of the author who moved the item to the current stage | Yes | No ||
|| **DATE_CREATE**
[`datetime`][1] | Create date | Yes | No ||
|| **DATE_MODIFY**
[`datetime`][1] | Change date | Yes | No ||
|| **MOVED_TIME**
[`datetime`][1] | Date the item was moved to the current stage | Yes | No ||
|| **COMPANY_ID**
[`crm_company`][2] | Linking the lead to a company (Client->Company field) | Yes | Yes ||
|| **CONTACT_ID**
[`crm_contact`][2] | Linking the lead to a contact. Obsolete field, currently not used. Kept for backward compatibility | Yes | Yes ||
|| **CONTACT_IDS**
[`crm_contact`][2] |  Linked contact identifier. Multiple.

When using [crm.lead.update](./leads/crm-lead-update.md) and [crm.lead.add](./leads/crm-lead-add.md), you can pass an array of contacts  | Yes | Yes ||
|| **IS_RETURN_CUSTOMER**
[`char`][1] | Repeat lead indicator. Allowed values `Y` or `N` | Yes | No ||
|| **DATE_CLOSED**
[`datetime`][1] | Closing date | Yes | No ||
|| **ORIGINATOR_ID**
[`string`][1] | Data source identifier. Used only for linking to an external source. | Yes | Yes ||
|| **ORIGIN_ID**
[`string`][1] | Item identifier in the data source. Used only for linking to an external source. | Yes | Yes ||
|| **UTM_SOURCE**
[`string`][1] | Ad system (Search Ads, Display Ads, and others). | Yes | Yes ||
|| **UTM_MEDIUM**
[`string`][1] | Traffic type: CPC (ads), CPM (banners). | Yes | Yes ||
|| **UTM_CAMPAIGN**
[`string`][1] | Advertising campaign designation. | Yes | Yes ||
|| **UTM_CONTENT**
[`string`][1] | Campaign contents. For example, for contextual ads. | Yes | Yes ||
|| **UTM_TERM**
[`string`][1] | Campaign search condition. For example, contextual advertising keywords. | Yes | Yes ||
|| **LAST_ACTIVITY_TIME**
[`datetime`][1] | Last activity time. | Yes | No ||
|| **LAST_ACTIVITY_BY**
[`string`][1] | Identifier of the user assigned to the last activity in this lead (for example, the one who created a new activity in the lead). | Yes | No ||
|| **PHONE**
[`crm_multifield`][2] | Contact phone. Multiple | Yes | Yes ||
|| **EMAIL**
[`crm_multifield`][2] | Email address. Multiple | Yes | Yes ||
|| **WEB**
[`crm_multifield`][2] | Lead URL resources. Multiple | Yes | Yes ||
|| **IM**
[`crm_multifield`][2] | Messengers. Multiple | Yes | Yes ||
|| **LINK**
[`crm_multifield`][2] |  Links. Multiple. Service | Yes | Yes ||
|| **UF_CRM_xxx** | [Custom fields](./leads/userfield/index.md) | Yes | Yes ||
|#

## Companies

The field description is returned by the method [crm.company.fields](./companies/crm-company-fields.md)

#|
|| **Name**
`type` | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`][1] | Company identifier | Yes | No ||
|| **TITLE**
[`string`][1] | Name. Required | Yes | Yes ||
|| **COMPANY_TYPE**
[`crm_status`][2] | Company type | Yes | Yes ||
|| **LOGO**
[`file`][1] | Logo | Yes | Yes ||
|| **ADDRESS**
[`string`][1] | Company address | Yes | Yes ||
|| **ADDRESS_2**
[`string`][1] | Address line 2. In some countries, it is common to split the address into 2 parts. | Yes | Yes ||
|| **ADDRESS_CITY**
[`string`][1] | City | Yes | Yes ||
|| **ADDRESS_POSTAL_CODE**
[`string`][1] | Postal code | Yes | Yes ||
|| **ADDRESS_REGION**
[`string`][1] | District | Yes | Yes ||
|| **ADDRESS_PROVINCE**
[`string`][1] | Region | Yes | Yes ||
|| **ADDRESS_COUNTRY**
[`string`][1] | Country | Yes | Yes ||
|| **ADDRESS_COUNTRY_CODE**
[`string`][1] | Country code | Yes | Yes ||
|| **ADDRESS_LOC_ADDR_ID**
[`integer`][1] | Location address identifier | Yes | Yes ||
|| **ADDRESS_LEGAL**
[`string`][1] | Legal address | Yes | Yes ||
|| **REG_ADDRESS**
[`string`][1] | Company legal address. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_2**
[`string`][1] | Legal address line 2. In some countries, it is common to split the address into 2 parts.

Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_CITY**
[`string`][1] | Legal address city. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_POSTAL_CODE**
[`string`][1] | Legal address postal code. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_REGION**
[`string`][1] | Legal address district. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_PROVINCE**
[`string`][1] | Legal address region. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_COUNTRY**
[`string`][1] | Legal address country. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_COUNTRY_CODE**
[`string`][1] | Legal address country code. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_LOC_ADDR_ID**
[`integer`][1] | Legal address location identifier. Obsolete, used for compatibility | Yes | Yes ||
|| **BANKING_DETAILS**
[`string`][1] | Bank Company details | Yes | Yes ||
|| **INDUSTRY**
[`crm_status`][2] | Industry | Yes | Yes ||
|| **EMPLOYEES**
[`crm_status`][2] | Number of employees | Yes | Yes ||
|| **CURRENCY_ID**
[`crm_currency`][2] | Currency | Yes | Yes ||
|| **REVENUE**
[`double`][1] | Annual turnover | Yes | Yes ||
|| **OPENED**
[`char`][1] | Available to all | Yes | Yes ||
|| **COMMENTS**
[`string`][1] | Comments | Yes | Yes ||
|| **HAS_PHONE**
[`char`][1] | Phone field completion check | Yes | No ||
|| **HAS_EMAIL**
[`char`][1] | Email field completion check | Yes | No ||
|| **HAS_IMOL**
[`char`][1] | Is Open Channel set | Yes | No ||
|| **IS_MY_COMPANY**
[`char`][1] | My company | Yes | Yes ||
|| **ASSIGNED_BY_ID**
[`user`][1] | Linked to user by ID | Yes | Yes ||
|| **CREATED_BY_ID**
[`user`][1] | Created by | Yes | No ||
|| **MODIFY_BY_ID**
[`user`][1] | Last change author identifier | Yes | No ||
|| **DATE_CREATE**
[`datetime`][1] | Create date | Yes | No ||
|| **DATE_MODIFY**
[`datetime`][1] | Change date | Yes | No ||
|| **CONTACT_ID**
[`string`][1] | Contact. Used only for linking to an external source. | Yes | Yes ||
|| **LEAD_ID**
[`crm_lead`][2] | Identifier of the lead linked to the company | Yes | No ||
|| **ORIGINATOR_ID**
[`string`][1] | Data source identifier. Used only for linking to an external source. | Yes | Yes ||
|| **ORIGIN_ID**
[`string`][1] | Item identifier in the data source. Used only for linking to an external source. | Yes | Yes ||
|| **ORIGIN_VERSION**
[`string`][1] | Original version. Used to protect data from accidental overwriting by an external system.

If the data was imported and has not changed in the external system, then such data can be edited in the CRM without fear that the next upload will lead to overwriting the data | Yes | Yes ||
|| **UTM_SOURCE**
[`string`][1] | Ad system (Search Ads, Display Ads, and others). | Yes | Yes ||
|| **UTM_MEDIUM**
[`string`][1] | Traffic type: CPC (ads), CPM (banners). | Yes | Yes ||
|| **UTM_CAMPAIGN**
[`string`][1] | Advertising campaign designation. | Yes | Yes ||
|| **UTM_CONTENT**
[`string`][1] | Campaign contents. For example, for contextual ads. | Yes | Yes ||
|| **UTM_TERM**
[`string`][1] | Campaign search condition. For example, contextual advertising keywords. | Yes | Yes ||
|| **PARENT_ID_xxx**
[`crm_entity`][2] | Relation fields.

If there are SPAs on the portal linked to contacts, for each such SPA there is a field storing the link between this SPA and the contact. The field itself stores the item identifier of such an SPA.

For example, the field `PARENT_ID_153` — link to SPA `entityTypeId=153`, stores the item identifier of this SPA linked to the current contact | Yes | Yes ||
|| **LAST_ACTIVITY_TIME**
[`datetime`][1] | Last activity time. | Yes | No ||
|| **LAST_ACTIVITY_BY**
[`string`][1] | Identifier of the user assigned to the last activity in this lead (for example, the one who created a new activity in the lead). | Yes | No ||
|| **PHONE**
[`crm_multifield`][2] | Company phone. Multiple | Yes | Yes ||
|| **EMAIL**
[`crm_multifield`][2] | Email address. Multiple | Yes | Yes ||
|| **WEB**
[`crm_multifield`][2] | Company resource URLs. Multiple | Yes | Yes ||
|| **IM**
[`crm_multifield`][2] | Messengers. Multiple | Yes | Yes ||
|| **LINK**
[`crm_multifield`][2] |  Links. Multiple. Service | Yes | Yes ||
|#

## Contacts

The field description is returned by the method [crm.contact.fields](./contacts/crm-contact-fields.md)

#|
|| **Name**
`type` | **Description** | **Read** | **Write** ||
||**ID**
[`integer`][1] | Contact identifier | Yes | No ||
||**HONORIFIC**
[`crm_status`][2] | Salutation.

You can obtain the dictionary values using the [crm.status.list](./status/crm-status-list.md) method with a filter by `ENTITY_ID=HONORIFIC` | Yes | Yes ||
||**NAME**
[`string`][1] | First name | Yes | Yes ||
||**SECOND_NAME**
[`string`][1] | Middle name | Yes | Yes ||
||**LAST_NAME**
[`string`][1] | Last name | Yes | Yes ||
||**PHOTO**
[`file`][1] | Photo | Yes | Yes ||
||**BIRTHDATE**
[`date`][1] | Date of birth. | Yes | Yes ||
||**TYPE_ID**
[`crm_status`][2]| Contact type.

You can obtain the dictionary values using the [crm.status.list](./status/crm-status-list.md) method with a filter by `ENTITY_ID=CONTACT_TYPE` | Yes | Yes ||
||**SOURCE_ID**
[`crm_status`][2] | Source.

You can obtain the dictionary values using the [crm.status.list](./status/crm-status-list.md) method with a filter by `ENTITY_ID=SOURCE`| Yes | Yes ||
||**SOURCE_DESCRIPTION**
[`string`][1] | Additional info about the source | Yes | Yes ||
||**POST**
[`string`][1] | Job title. | Yes | Yes ||
|| {% note tip "Deprecated fields" %}

Address fields in the contact are deprecated and used only for backward compatibility. To work with addresses, use [Company details](./requisites/index.md).

{% endnote %}| > | > | > ||
||**ADDRESS**
[`string`][1] | Address (deprecated) | Yes | Yes ||
||**ADDRESS_2**
[`string`][1] | Address line 2 (deprecated) | Yes | Yes ||
||**ADDRESS_CITY**
[`string`][1] | City (deprecated) | Yes | Yes ||
||**ADDRESS_POSTAL_CODE**
[`string`][1] | Postal code (deprecated) | Yes | Yes ||
||**ADDRESS_REGION**
[`string`][1] | District (deprecated) | Yes | Yes ||
||**ADDRESS_PROVINCE**
[`string`][1] | Region (deprecated) | Yes | Yes ||
||**ADDRESS_COUNTRY**
[`string`][1] | Country (deprecated) | Yes | Yes ||
||**ADDRESS_COUNTRY_CODE**
[`string`][1] | Country code (deprecated) | Yes | Yes ||
||**ADDRESS_LOC_ADDR_ID**
[`location`][1] | Location address identifier (deprecated) | Yes | Yes ||
||**COMMENTS**
[`string`][1] | Comment. Supports bb-codes | Yes | Yes ||
||**OPENED**
[`char`][1] | Available to all. Can take values `Y` or `N`. Taken into account in access rights for roles with "All open" access level | Yes | Yes ||
||**EXPORT**
[`char`][1] | Include in contact export. Can take values `Y` or `N`  | Yes | Yes ||
||**HAS_PHONE**
[`char`][1] | Phone is set. Can take values `Y` or `N` | Yes | No ||
||**HAS_EMAIL**
[`char`][1] | E-mail is set. Can take values `Y` or `N` | Yes | No ||
||**HAS_IMOL**
[`char`][1] | Open Channel is set. Can take values `Y` or `N` | Yes | No ||
||**ASSIGNED_BY_ID**
[`user`][1] | assigned user | Yes | Yes ||
||**CREATED_BY_ID**
[`user`][1] | Created by | Yes | No ||
||**MODIFY_BY_ID**
[`user`][1] | Changed by | Yes | No ||
||**DATE_CREATE**
[`datetime`][1] | Create date | Yes | No ||
||**DATE_MODIFY**
[`datetime`][1] | Change date | Yes | No ||
||**COMPANY_ID**
[`crm_company`][2] | Primary company of the contact | Yes | Yes ||
||**COMPANY_IDS**
[`crm_company`][2] | Linking a contact to companies. Multiple.

In methods [crm.contact.update](./contacts/crm-contact-update.md) and [crm.contact.add](./contacts/crm-contact-add.md), it is used to provide an array of companies.

In methods [crm.contact.list](./contacts/crm-contact-list.md) and [crm.contact.get](./contacts/crm-contact-get.md), this field does not exist, and you must use [crm.contact.company.items.get](./contacts/company/crm-contact-company-items-get.md) to retrieve the list of companies  | Yes | Yes ||
||**LEAD_ID**
[`crm_lead`][2] | Identifier of the lead associated with the contact | Yes | No ||
|| {% note tip "Connection fields with external data sources" %}

If the contact was created by an external system, then:
- the `ORIGINATOR_ID` field stores the string identifier of that system
- the `ORIGIN_ID` field stores the string identifier of the contact in that external system
- the `ORIGIN_VERSION` field stores the version of the contact data in that external system

{% endnote %} | > | > | > ||
||**ORIGINATOR_ID**
[`string`][1] | Identifier of the external system that is the source of data about this contact | Yes | Yes ||
||**ORIGIN_ID**
[`string`][1] | Identifier of the contact in the external system | Yes | Yes ||
||**ORIGIN_VERSION**
[`string`][1] | Version of the contact data in the external system. Used to protect data from accidental overwriting by an external system.

If the data was imported and has not changed in the external system, such data can be edited in the CRM without fear that the next export will lead to overwriting the data. | Yes | Yes ||
||**FACE_ID**
[`integer`][1] | Link to persons from the `faceid` module | Yes | No ||
||**UTM_SOURCE**
[`string`][1] | Ad system (Search Ads, Display Ads, and others). | Yes | Yes ||
||**UTM_MEDIUM**
[`string`][1] | Traffic type: CPC (ads), CPM (banners). | Yes | Yes ||
||**UTM_CAMPAIGN**
[`string`][1] | Advertising campaign designation. | Yes | Yes ||
||**UTM_CONTENT**
[`string`][1] | Campaign contents. For example, for contextual ads. | Yes | Yes ||
||**UTM_TERM**
[`string`][1] | Campaign search condition. For example, contextual advertising keywords. | Yes | Yes ||
||**PARENT_ID_...** | Relation fields.

If there are SPAs on the portal linked to contacts, for each such SPA there is a field storing the link between this SPA and the contact. The field itself stores the item identifier of such an SPA.

For example, the field `PARENT_ID_153` — link to SPA `entityTypeId=153`, stores the item identifier of this SPA linked to the current contact ||
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
||**UF_CRM_xxx**  | custom fields. For example, `UF_CRM_25534736`.

Depending on the portal settings, contacts may have a set of custom fields of certain types. You can add a custom field to a contact using the [crm.contact.userfield.add](./contacts/userfield/crm-contact-userfield-add.md) method  ||
|#

## Company Details

### General Requisites

The field description is returned by the method [crm.requisite.fields](./requisites/universal/crm-requisite-fields.md)

#|
|| **Name**
`type` | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`][1] | Identifier of the requisite.

Can be obtained using the [crm.requisite.list](./requisites/universal/crm-requisite-list.md) method.

Created automatically and is unique within the portal | Yes | No ||
|| **ENTITY_TYPE_ID**
[`integer`][1] | Identifier of the parent entity type. Currently, this can only be:
- `3` — contact
- `4` — company

The [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) method returns identifiers for all CRM entity types| Yes | Yes ||
|| **ENTITY_ID**
[`integer`][1] | Identifier of the parent entity (contact or company).

The identifier can be obtained using the [crm.company.list](./companies/crm-company-list.md) method for a company and the [crm.contact.list](./contacts/crm-contact-list.md) method for a contact. | Yes | Yes ||
|| **PRESET_ID**
[`integer`][1] | Requisite template identifier.

Template identifiers can be obtained using the [crm.requisite.preset.list](./requisites/presets/crm-requisite-preset-list.md) method. | Yes | Yes ||
|| **DATE_CREATE**
[`datetime`][1] | Create date | Yes | No ||
|| **DATE_MODIFY**
[`datetime`][1] | Change date | Yes | No ||
|| **CREATED_BY_ID**
[`user`][1] | Identifier of the user who created the requisite. | Yes | No ||
|| **MODIFY_BY_ID**
[`user`][1] | Identifier of the user who changed the requisite. | Yes | No ||
|| **NAME**
[`string`][1] | Requisite name. | Yes | Yes ||
|| **CODE**
[`string`][1] | Character code of the requisite. | Yes | Yes ||
|| **XML_ID**
[`string`][1] | Foreign key, used for exchange operations.

Identifier of the external information base object.

The purpose of the field may be changed by the end developer. | Yes | Yes ||
|| **ORIGINATOR_ID**
[`string`][1] | External information base identifier.

The purpose of the field may be changed by the end developer. | Yes | Yes ||
|| **ACTIVE**
[`char`][1] | Activity flag.

Uses values `Y` or `N`.

Currently, the field does not actually affect anything. | Yes | Yes ||
|| **ADDRESS_ONLY**
[`char`][1] | Status flag, when the requisite is used only for storing an address.

Uses values `Y` or `N`. When the value is `Y`, Company details are not shown in the entity card, but the address is displayed. | Yes | Yes ||
|| **SORT**
[`integer`][1] | Sorting. The order in the entity's requisite list when there are multiple requisites. | Yes | Yes ||
|| **RQ_NAME**
[`string`][1] | Full Name | Yes | Yes ||
|| **RQ_FIRST_NAME**
[`string`][1] | First name | Yes | Yes ||
|| **RQ_LAST_NAME**
[`string`][1] | Last name | Yes | Yes ||
|| **RQ_SECOND_NAME**
[`string`][1] | Middle name | Yes | Yes ||
|| **RQ_COMPANY_ID**
[`string`][1] | Organization identifier | Yes | Yes ||
|| **RQ_COMPANY_NAME**
[`string`][1] | Abbreviated organization name | Yes | Yes ||
|| **RQ_COMPANY_FULL_NAME**
[`string`][1] | Full organization name | Yes | Yes ||
|| **RQ_COMPANY_REG_DATE**
[`string`][1] | State registration date | Yes | Yes ||
|| **RQ_DIRECTOR**
[`string`][1] | General Director | Yes | Yes ||
|| **RQ_ACCOUNTANT**
[`string`][1] | Chief Accountant | Yes | Yes ||
|| **RQ_CEO_NAME**
[`string`][1] | Full Name of the first manager | Yes | Yes ||
|| **RQ_CEO_WORK_POS**
[`string`][1] | Position of the first manager | Yes | Yes ||
|| **RQ_CONTACT**
[`string`][1] | Contact person | Yes | Yes ||
|| **RQ_EMAIL**
[`string`][1] | E-Mail | Yes | Yes ||
|| **RQ_PHONE**
[`string`][1] | Phone | Yes | Yes ||
|| **RQ_FAX**
[`string`][1] | Fax | Yes | Yes ||
|| **RQ_IDENT_TYPE**
[`crm_status`][2] | Identification method | Yes | Yes ||
|| **RQ_IDENT_DOC**
[`string`][1] | Document type | Yes | Yes ||
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
[`string`][1] | TIN | Yes | Yes ||
|| **RQ_KPP**
[`string`][1] | KPP | Yes | Yes ||
|| **RQ_USRLE**
[`string`][1] | Handelsregisternummer (for country DE) | Yes | Yes ||
|| **RQ_IFNS**
[`string`][1] | IFNS | Yes | Yes ||
|| **RQ_OGRN**
[`string`][1] | OGRN | Yes | Yes ||
|| **RQ_OGRNIP**
[`string`][1] | OGRNIP | Yes | Yes ||
|| **RQ_OKPO**
[`string`][1] | OKPO | Yes | Yes ||
|| **RQ_OKTMO**
[`string`][1] | OKTMO | Yes | Yes ||
|| **RQ_OKVED**
[`string`][1] | OKVED | Yes | Yes ||
|| **RQ_EDRPOU**
[`string`][1] | EDRPOU | Yes | Yes ||
|| **RQ_DRFO**
[`string`][1] | DRFO | Yes | Yes ||
|| **RQ_KBE**
[`string`][1] | KBE | Yes | Yes ||
|| **RQ_IIN**
[`string`][1] | IIN | Yes | Yes ||
|| **RQ_BIN**
[`string`][1] | BIN | Yes | Yes ||
|| **RQ_ST_CERT_SER**
[`string`][1] | State registration certificate series | Yes | Yes ||
|| **RQ_ST_CERT_NUM**
[`string`][1] | State registration certificate number | Yes | Yes ||
|| **RQ_ST_CERT_DATE**
[`string`][1] | State registration certificate date | Yes | Yes ||
|| **RQ_VAT_PAYER**
[`char`][1] | VAT Payer (for country UA).

Uses values `Y` or `N` | Yes | Yes ||
|| **RQ_VAT_ID**
[`string`][1] | VAT ID (Value Added Tax identification number) | Yes | Yes ||
|| **RQ_VAT_CERT_SER**
[`string`][1] | VAT certificate series | Yes | Yes ||
|| **RQ_VAT_CERT_NUM**
[`string`][1] | VAT certificate number | Yes | Yes ||
|| **RQ_VAT_CERT_DATE**
[`string`][1] | VAT certificate date | Yes | Yes ||
|| **RQ_RESIDENCE_COUNTRY**
[`string`][1] | Country of residence | Yes | Yes ||
|| **RQ_BASE_DOC**
[`string`][1] | Basis of action | Yes | Yes ||
|| **RQ_REGON**
[`string`][1] | REGON (for country PL) | Yes | Yes ||
|| **RQ_KRS**
[`string`][1] | KRS (for country PL) | Yes | Yes ||
|| **RQ_PESEL**
[`string`][1] | PESEL (for country PL) | Yes | Yes ||
|| **RQ_LEGAL_FORM**
[`string`][1] | Legal form (for country FR) | Yes | Yes ||
|| **RQ_SIRET**
[`string`][1] | Siret Number (for country FR) | Yes | Yes ||
|| **RQ_SIREN**
[`string`][1] | Siren Number (for country FR) | Yes | Yes ||
|| **RQ_CAPITAL**
[`string`][1] | Share capital (for country FR) | Yes | Yes ||
|| **RQ_RCS**
[`string`][1] | RCS (for country FR) | Yes | Yes ||
|| **RQ_CNPJ**
[`string`][1] | CNPJ (for country BR) | Yes | Yes ||
|| **RQ_STATE_REG**
[`string`][1] | State Registration (IE) (for country BR) | Yes | Yes ||
|| **RQ_MNPL_REG**
[`string`][1] | Municipal Registration (IM) (for country BR) | Yes | Yes ||
|| **RQ_CPF**
[`string`][1] | CPF (for country BR) | Yes | Yes ||
|| **UF_CRM_...** | Custom fields. For example, `UF_CRM_1694526604`.

Requisites can have a set of custom fields with types: `string`, `boolean`, `double`, `datetime`.

You can add a custom field to requisites using the [crm.requisite.userfield.add](./requisites/user-fields/crm-requisite-userfield-add.md) method | Yes | Yes ||
|#

### Bank Details

The field description is returned by the method [crm.requisite.bankdetail.fields](./requisites/bank-detail/crm-requisite-bank-detail-fields.md)

#|
|| **Name**
`type` | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`][1] | Bank requisite identifier. Created automatically and is unique within the portal | Yes | No ||
|| **ENTITY_TYPE_ID**
[`integer`][1] | Parent object type identifier. Can only be `Attribute` (value `8`).

The method [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) returns object type identifiers | Yes | No ||
|| **ENTITY_ID**
[`integer`][1] | Parent object identifier | Yes | Yes ||
|| **COUNTRY_ID**
[`integer`][1] | Country identifier that corresponds to the set of bank requisite fields (see method [crm.requisite.preset.countries](./requisites/presets/crm-requisite-preset-countries.md) to get available values).

The bank requisite country code matches the country code in the linked requisite template, whose identifier is specified in the `ENTITY_ID` field| Yes | Yes ||
|| **DATE_CREATE**
[`datetime`][1] | Create date | Yes | No ||
|| **DATE_MODIFY**
[`datetime`][1] | Change date | Yes | No ||
|| **CREATED_BY_ID**
[`user`][1] | Identifier of the user who created the requisite | Yes | No ||
|| **MODIFY_BY_ID**
[`user`][1] | Identifier of the user who changed the requisite | Yes | No ||
|| **NAME^*^**
[`string`][1] | Bank requisite name | Yes | Yes ||
|| **CODE**
[`string`][1] | Character code of the requisite. | Yes | Yes ||
|| **XML_ID**
[`string`][1] | External key. Used for exchange operations. Identifier of the external information base object.

The purpose of the field may be changed by the end developer. Each application ensures the uniqueness of values in this field.

It is recommended to use a unique prefix to avoid collisions with other applications | Yes | Yes ||
|| **ACTIVE**
[`char`][1] | Activity flag. Uses values `Y` or `N`.

Currently, the field does not actually affect anything | Yes | Yes ||
|| **SORT**
[`integer`][1] | Sorting | Yes | Yes ||
|| **RQ_BANK_NAME**
[`string`][1] | Bank name | Yes | Yes ||
|| **RQ_BANK_ADDR**
[`string`][1] | Bank address | Yes | Yes ||
|| **RQ_BANK_CODE**
[`string`][1] | Bank Code (for country BR) | Yes | Yes ||
|| **RQ_BANK_ROUTE_NUM**
[`string`][1] | Bank Routing Number | Yes | Yes ||
|| **RQ_BIK**
[`string`][1] | BIC | Yes | Yes ||
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
[`string`][1] | Account currency | Yes | Yes ||
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
[`string`][1] | External information base identifier. The purpose of the field may be changed by the end developer | Yes | Yes ||
|#

### Templates of Requisites

The field description is returned by the method [crm.requisite.preset.fields](./requisites/presets/crm-requisite-preset-fields.md)

#|
|| **Name** | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`][1] | Requisite identifier. Created automatically and is unique within the portal | Yes | No ||
|| **ENTITY_TYPE_ID**
[`integer`][1] | Parent object type identifier.

The method [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) returns CRM object type identifiers | Yes | Yes ||
|| **COUNTRY_ID**
[`integer`][1] | Country identifier that corresponds to the set of fields in the requisite template (to get available values, see method [crm.requisite.preset.countries](./requisites/presets/crm-requisite-preset-countries.md)) | Yes | Yes ||
|| **DATE_CREATE**
[`datetime`][1] | Create date | Yes | No ||
|| **DATE_MODIFY**
[`datetime`][1] | Modification date. Contains an empty string if the template has not changed since creation. | Yes | No ||
|| **CREATED_BY_ID**
[`user`][1] | Identifier of the user who created the requisite | Yes | No ||
|| **MODIFY_BY_ID**
[`user`][1] | Identifier of the user who changed the requisite | Yes | No ||
|| **NAME**
[`string`][1] | Requisite name. | Yes | Yes ||
|| **XML_ID**
[`string`][1] | Foreign key. Used for exchange operations. Identifier of an object in an external information base.

The purpose of the field may change depending on the end developer.

Each application ensures the uniqueness of values in this field. It is recommended to use a unique prefix to avoid collisions with other applications.

In CRM, values like `#CRM_REQUISITE_PRESET_DEF_...` are reserved for identifying default templates. These identifiers should not be used for your own purposes, as this may lead to logic violations. | Yes | Yes ||
|| **ACTIVE**
[`char`][1] | Activity flag. Uses values `Y` or `N`. Determines the availability of the template in the selection list when adding requisites. | Yes | Yes ||
|| **SORT**
[`integer`][1] | Sorting | Yes | Yes ||
|#

### Fields of Requisites Templates

The field description is returned by the method [crm.requisite.preset.field.fields](./requisites/presets/fields/crm-requisite-preset-field-fields.md)

#|
||  **Name**
`type` | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`][1] | Field identifier. Created automatically and is unique within the template. | Yes | No ||
|| **FIELD_NAME**
[`string`][1] | Field name | Yes | Yes ||
|| **FIELD_TITLE**
[`string`][1] | Alternative field name for a requisite.

The alternative name is displayed in various forms for filling out requisites. Depending on the specific form, the alternative name may or may not be used. | Yes | Yes ||
|| **SORT**
[`integer`][1] | Sorting. The order in the template field list. | Yes | Yes ||
|| **IN_SHORT_LIST**
[`char`][1] | Show in short list. Deprecated field, currently not used. Kept for backward compatibility. Can take values `Y` or `N`. | Yes | Yes ||
|#

### Addresses of Requisites

The field description is returned by the method [crm.address.fields](./requisites/addresses/crm-address-fields.md)

#|
|| **Name** | **Description** | **Read** | **Write** ||
|| **TYPE_ID**
[`integer`][1] | Address type identifier. "Address type" enumeration item.

"Address type" enumeration items can be obtained using the [crm.enum.addresstype](./auxiliary/enum/crm-enum-address-type.md) method. | Yes | Yes  ||
|| **ENTITY_TYPE_ID**
[`integer`][1] | Parent object type identifier.

Object type identifiers can be obtained using the [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) method.

{% note tip "" %}

Addresses can only be linked to Requisites (whereas Company details are already linked to companies or contacts) or Leads. For backward compatibility, the ability to link Addresses to Contacts or Companies has been kept. However, this connection is only possible on some old portals where the old address mode was specifically enabled by technical support.

{% endnote %} | Yes | Yes ||
|| **ENTITY_ID**
[`string`][1] | Parent object identifier | Yes | Yes ||
|| **ADDRESS_1**
[`string`][1] | Street, house, building, structure | Yes | Yes ||
|| **ADDRESS_2**
[`string`][1] | Apartment / office | Yes | Yes ||
|| **CITY**
[`string`][1] | City | Yes | Yes ||
|| **POSTAL_CODE**
[`string`][1] | Postal code | Yes | Yes ||
|| **REGION**
[`string`][1] | District | Yes | Yes ||
|| **PROVINCE**
[`string`][1] | Region | Yes | Yes ||
|| **COUNTRY**
[`string`][1] | Country | Yes | Yes ||
|| **COUNTRY_CODE**
[`string`][1] | Country code | Yes | Yes ||
|| **LOC_ADDR_ID**
[`integer`][1] | Location identifier.

This field contains the identifier of the address object in the `Location` module, associated with the CRM address object. Each CRM address corresponds to an address object in the module `location`. This can be used to copy an existing CRM address with location information that is not present in the CRM address fields.

If a `location` module address identifier is specified when creating an address, a copy of the address is created `location` and linked to the created CRM address. If, in this case, no values are specified for the string address fields, they will be filled from the location address.

However, if at least one string field was specified, only the specified fields will be saved in the CRM address, and their values will overwrite the corresponding values in the location address object. The same behavior applies when updating an address. | Yes | Yes ||
|| **ANCHOR_TYPE_ID**
[`integer`][1] | Main parent object type identifier.

This field is for internal use. The value is filled automatically when an address is added.

Object type identifiers can be obtained using the [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) method.

This field contains the identifier of the attribute's parent object type (company or contact) if the address is linked to an attribute. If the address is linked to a lead, this value will be the lead type identifier. | Yes | No ||
|| **ANCHOR_ID**
[`integer`][1] | This field is for internal use. The value is filled automatically when an address is added.

This field contains the identifier of the attribute's parent object (company or contact) if the address is linked to an attribute. If the address is linked to a lead, this value will be the lead identifier. | Yes | No ||
|#

## Activities

The field description is returned by the method [crm.activity.fields](./timeline/activities/activity-base/crm-activity-fields.md)

#|
|| **Name** | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`][1] | Activity identifier | Yes | No ||
|| **OWNER_ID**
[`integer`][1] | Owner identifier, immutable | Yes | Yes ||
|| **OWNER_TYPE_ID**
[`crm.enum.ownertype`][2] | Owner type, immutable | Yes | Yes ||
|| **TYPE_ID**
[`crm_enum_activitytype`][2] | Type, immutable | Yes | Yes ||
|| **PROVIDER_ID**
[`string`][1] | Provider identifier | Yes | Yes ||
|| **PROVIDER_TYPE_ID**
[`string`][1] | Provider type identifier | Yes | Yes ||
|| **PROVIDER_GROUP_ID**
[`string`][1] | Connector type | Yes | Yes ||
|| **ASSOCIATED_ENTITY_ID**
[`integer`][1] | Entity identifier related to the activity | Yes | No ||
|| **SUBJECT**
[`string`][1] | Subject, activity title | Yes | Yes ||
|| **START_TIME**
[`datetime`][1] | Start time | Yes | Yes ||
|| **END_TIME**
[`datetime`][1] | Completion time | Yes | Yes ||
|| **DEADLINE**
[`datetime`][1] | Due date. This field is not set directly; the value is taken from `START_TIME` for calls and meetings and from `END_TIME` for tasks. | Yes | Yes ||
|| **COMPLETED**
[`char`][1] | Completed | Yes | Yes ||
|| **STATUS**
[`crm_enum_activitystatus`][2] | Status | Yes | Yes ||
|| **RESPONSIBLE_ID**
[`user`][1] | assigned user | Yes | Yes ||
|| **PRIORITY**
[`crm.enum.activitypriority`][2] | Importance | Yes | Yes ||
|| **NOTIFY_TYPE**
[`crm.enum.activitynotifytype`][2] | Notification type | Yes | Yes ||
|| **NOTIFY_VALUE**
[`integer`][1] | Notification parameter | Yes | Yes ||
|| **DESCRIPTION**
[`string`][1] | Description | Yes | Yes ||
|| **DESCRIPTION_TYPE**
[`crm.enum.contenttype`][2] | Description type | Yes | Yes ||
|| **DIRECTION**
[`crm.enum.activitydirection`][2] | Activity direction: inbound/outbound. Relevant for calls and emails, not used for meetings. | Yes | Yes ||
|| **LOCATION**
[`string`][1] | Location | Yes | Yes ||
|| **CREATED**
[`datetime`][1] | Create date | Yes | No ||
|| **AUTHOR_ID**
[`user`][1] | Activity creator | Yes | Yes ||
|| **LAST_UPDATED**
[`datetime`][1] | Last update date | Yes | No ||
|| **EDITOR_ID**
[`user`][1] | Changed by | Yes | No ||
|| **SETTINGS**
[`object`][1] | Settings | Yes | Yes ||
|| **ORIGIN_ID**
[`string`][1] | Item identifier in the data source. Used only for linking to an external source. | Yes | Yes ||
|| **ORIGINATOR_ID**
[`string`][1] | Data source identifier. Used only for linking to an external source. | Yes | Yes ||
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
[`integer`][1] | Autofill | Yes | Yes ||
|| **BINDINGS**
[`crm_activity_binding`][2] | Links | Yes | No ||
|| **COMMUNICATIONS**
[`crm_activity_communication`][2] | Communication channel. Multiple, mandatory | Yes | Yes ||
|| **FILES**
[`diskfile`][1] | Added files. Multiple | Yes | Yes ||
|| **WEBDAV_ELEMENTS**
[`diskfile`][1] | Added files. Multiple. Deprecated, kept for compatibility | Yes | Yes ||
|| **IS_INCOMING_CHANNEL**
[`char`][1] | Whether the activity is inbound, i.e., created as a result of an incoming client Salutation to a communication channel | Yes | No ||
|#

[1]: ../data-types.md
[2]: ./data-types.md
