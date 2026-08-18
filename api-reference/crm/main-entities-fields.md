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
[`integer`](../data-types.md) | Deal identifier | Yes | No ||
|| **TITLE**
[`string`](../data-types.md) | Name | Yes | Yes ||
|| **TYPE_ID**
[`crm_status`](./data-types.md) | Deal type. Used only for linking to an external source. | Yes | Yes ||
|| **CATEGORY_ID**
[`crm_category`](./data-types.md) | Direction identifier. Immutable. If this field is not passed when creating a deal, the deal will be created in the general direction. | Yes | Yes ||
|| **STAGE_ID**
[`crm_status`](./data-types.md) | Stage identifier. Possible values:
- `NEW` — new deal
- `PREPARATION` — document preparation
- `PREPAYMENT_INVOICE` — invoice sending
- `EXECUTING` — in progress
- `FINAL_INVOICE` — final invoice
- `WON` — won
- `>LOSE` — lost, no reason analysis required
- `APOLOGY` — lost, reason analysis required | Yes | Yes ||
|| **STAGE_SEMANTIC_ID**
[`string`](../data-types.md) | Name. Read-only. In a sense, generalizes the values of the deal identifier `STAGE_ID`:
- `P` — for stages with identifiers `NEW`, `PREPARATION`, `PREPAYMENT_INVOICE`, `EXECUTING`, and `FINAL_INVOICE`
- `S` — for the stage with identifier `WON`
- `F` — for stages with identifiers `>LOSE` and `APOLOGY` | Yes | No ||
|| **IS_NEW**
[`char`](../data-types.md) | New deal flag (deals in the first stage) | Yes | No ||
|| **IS_RECURRING**
[`char`](../data-types.md) | Recurring deal template flag. If `Y` is set, then it is a template, not a deal | Yes | Yes ||
|| **IS_RETURN_CUSTOMER**
[`char`](../data-types.md) | Repeat lead indicator | Yes | Yes ||
|| **IS_REPEATED_APPROACH**
[`char`](../data-types.md) | Repeat Salutation | Yes | Yes ||
|| **PROBABILITY**
[`integer`](../data-types.md) | Probability | Yes | Yes ||
|| **CURRENCY_ID**
[`crm_currency`](./data-types.md) | Deal currency identifier | Yes | Yes ||
|| **OPPORTUNITY**
[`double`](../data-types.md) | Amount | Yes | Yes ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../data-types.md) | Repeat Salutation | Yes | Yes ||
|| **TAX_VALUE**
[`double`](../data-types.md) | Tax rate | Yes | Yes ||
|| **COMPANY_ID**
[`crm_company`](./data-types.md) | Linked company identifier | Yes | Yes ||
|| **CONTACT_ID**
[`crm_contact`](./data-types.md) | Linked contact identifier. Deprecated. Retained for compatibility | Yes | Yes ||
|| **CONTACT_IDS**
[`crm_contact`](./data-types.md) | Linked contact identifier. Multiple.

When using [crm.deal.update](./deals/crm-deal-update.md) and [crm.deal.add](./deals/crm-deal-add.md), you can pass an array of contacts.

In the [crm.deal.list](./deals/crm-deal-list.md) and [crm.deal.get](./deals/crm-deal-get.md) methods, this field does not exist, and you must use [crm.deal.contact.items.get](./deals/contacts/crm-deal-contact-items-get.md) to retrieve the list of contacts.

To clear the field, use [crm.deal.contact.items.delete](./deals/contacts/crm-deal-contact-items-delete.md); to replace the value, use [crm.deal.contact.items.set](./deals/contacts/crm-deal-contact-items-set.md) | Yes | Yes ||
|| **QUOTE_ID**
[`crm_quote`](./data-types.md) | Quote identifier. Read-only. Deprecated. Use the [crm.quote.list](./quote/crm-quote-list.md) method with a filter by deal | Yes | No ||
|| **BEGINDATE**
[`date`](../data-types.md) | Start date | Yes | Yes ||
|| **CLOSEDATE**
[`date`](../data-types.md) | End date | Yes | Yes ||
|| **OPENED**
[`char`](../data-types.md) | Available to all | Yes | Yes ||
|| **CLOSED**
[`char`](../data-types.md) | Is the deal completed | Yes | Yes ||
|| **COMMENTS**
[`string`](../data-types.md) | Comments | Yes | Yes ||
|| **ASSIGNED_BY_ID**
[`user`](../data-types.md) | Linked to user by ID | Yes | Yes ||
|| **CREATED_BY_ID**
[`user`](../data-types.md) | Created by user | Yes | No ||
|| **MODIFY_BY_ID**
[`user`](../data-types.md) | Last change author identifier | Yes | No ||
|| **MOVED_BY_ID**
[`user`](../data-types.md) | Author identifier who moved the item to the current stage | Yes | No ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Create date | Yes | No ||
|| **DATE_MODIFY**
[`datetime`](../data-types.md) | Change date | Yes | No ||
|| **MOVED_TIME**
[`datetime`](../data-types.md) | Date the item was moved to the current stage | Yes | No ||
|| **SOURCE_ID**
[`string`](../data-types.md) | Source identifier. Determines the source of the deal (callback, advertisement, Email, etc.).

A list of possible identifiers can be obtained using the [crm.status.list](./status/crm-status-list.md) method with the `filter[ENTITY_ID]=SOURCE` filter. | Yes | Yes ||
|| **SOURCE_DESCRIPTION**
[`string`](../data-types.md) | Additional information about the source. Text field. | Yes | Yes ||
|| **ADDITIONAL_INFO**
[`string`](../data-types.md) | Additional information. | Yes | Yes ||
|| **LEAD_ID**
[`crm_lead`](./data-types.md) | Linked lead identifier. | Yes | No ||
|| **LOCATION_ID**
[`location`](./data-types.md) | Customer location. Service field, not recommended for use. | Yes | Yes ||
|| **ORIGINATOR_ID**
[`string`](../data-types.md) | Data source identifier. Used only for linking to an external source. | Yes | Yes ||
|| **ORIGIN_ID**
[`string`](../data-types.md) | Item identifier in the data source. Used only for linking to an external source. | Yes | Yes ||
|| **UTM_SOURCE**
[`string`](../data-types.md) | Ad system (Search Ads, Display Ads, and others). | Yes | Yes ||
|| **UTM_MEDIUM**
[`string`](../data-types.md) | Traffic type: CPC (ads), CPM (banners). | Yes | Yes ||
|| **UTM_CAMPAIGN**
[`string`](../data-types.md) | Advertising campaign designation. | Yes | Yes ||
|| **UTM_CONTENT**
[`string`](../data-types.md) | Campaign contents. For example, for contextual ads. | Yes | Yes ||
|| **UTM_TERM**
[`string`](../data-types.md) | Campaign search condition. For example, contextual advertising keywords. | Yes | Yes ||
|| **PARENT_ID_xxx**
[`crm_entity`](./data-types.md) | Relation fields.

If there are SPAs on the portal linked to contacts, then for each such SPA, there is a field that stores the connection between this SPA and the contact. The field itself stores the item identifier of that SPA.

For example, the `PARENT_ID_153` field — a connection with SPA `entityTypeId=153`, stores the item identifier of this SPA linked to the current contact. | Yes | Yes ||
|| **LAST_ACTIVITY_BY**
[`string`](../data-types.md) | Identifier of the user assigned to the last activity in this lead (for example, the one who created a new activity in the lead). | Yes | Yes ||
|| **LAST_ACTIVITY_TIME**
[`datetime`](../data-types.md) | Last activity time. | Yes | Yes ||
|| **UF_CRM_xxx** | [Custom fields](./deals/user-defined-fields/index.md) | Yes | Yes ||
|#

## Leads

The field description is returned by the method [crm.lead.fields](./leads/crm-lead-fields.md)

#|
|| **Name**
`type` | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`](../data-types.md) | Integer lead identifier. | Yes | No ||
|| **TITLE**
[`string`](../data-types.md) | Lead name. | Yes | Yes ||
|| **HONORIFIC**
[`crm_status`](./data-types.md) | Salutation. Status from the directory.

A list of possible identifiers can be obtained using the [crm.status.list](./status/crm-status-list.md) method with the `filter[ENTITY_ID]=HONORIFIC` filter. | Yes | Yes ||
|| **NAME**
[`string`](../data-types.md) |  Contact first name. | Yes | Yes ||
|| **SECOND_NAME**
[`string`](../data-types.md) |  Contact middle name. | Yes | Yes ||
|| **LAST_NAME**
[`string`](../data-types.md) |  Contact last name. | Yes | Yes ||
|| **BIRTHDATE**
[`date`](../data-types.md) | Date of birth. | Yes | Yes ||
|| **COMPANY_TITLE**
[`string`](../data-types.md) | Name of the company linked to the lead. | Yes | Yes ||
|| **SOURCE_ID**
[`crm_status`](./data-types.md) | Source identifier. Status from the directory.

A list of possible identifiers can be obtained using the [crm.status.list](./status/crm-status-list.md) method with the `filter[ENTITY_ID]=SOURCE` filter. | Yes | Yes ||
|| **SOURCE_DESCRIPTION**
[`string`](../data-types.md) | Source description. | Yes | Yes ||
|| **STATUS_ID**
[`crm_status`](./data-types.md) | Lead stage identifier. Status from the directory.

A list of possible identifiers can be obtained using the [crm.status.list](./status/crm-status-list.md) method with the `filter[ENTITY_ID]=STATUS` filter. | Yes | Yes ||
|| **STATUS_DESCRIPTION**
[`string`](../data-types.md) | Additional information about the stage. | Yes | Yes ||
|| **STATUS_SEMANTIC_ID**
[`string`](../data-types.md) | Status. Possible values:
- `F` (failed) — processed unsuccessfully
- `S` (success) — processed successfully
- `P` (processing) — lead is being processed. | Yes | No ||
|| **POST**
[`string`](../data-types.md) | Job title. | Yes | Yes ||
|| **ADDRESS**
[`string`](../data-types.md) | Contact address. | Yes | Yes ||
|| **ADDRESS_2**
[`string`](../data-types.md) | Address line 2. In some countries, it is common to split the address into 2 parts. | Yes | Yes ||
|| **ADDRESS_CITY**
[`string`](../data-types.md) | City | Yes | Yes ||
|| **ADDRESS_POSTAL_CODE**
[`string`](../data-types.md) | Postal code | Yes | Yes ||
|| **ADDRESS_REGION**
[`string`](../data-types.md) | District | Yes | Yes ||
|| **ADDRESS_PROVINCE**
[`string`](../data-types.md) | Region | Yes | Yes ||
|| **ADDRESS_COUNTRY**
[`string`](../data-types.md) | Country | Yes | Yes ||
|| **ADDRESS_COUNTRY_CODE**
[`string`](../data-types.md) | Country code | Yes | Yes ||
|| **ADDRESS_LOC_ADDR_ID**
[`integer`](../data-types.md) | Address identifier from the locations module | Yes | Yes ||
|| **CURRENCY_ID**
[`crm_currency`](./data-types.md) | Currency identifier | Yes | Yes ||
|| **OPPORTUNITY**
[`double`](../data-types.md) | Estimated amount | Yes | Yes ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../data-types.md) | Manual amount calculation flag. Allowed values `Y` or `N` | Yes | Yes ||
|| **OPENED**
[`char`](../data-types.md) | Available to all. Allowed values `Y` or `N` | Yes | Yes ||
|| **COMMENTS**
[`string`](../data-types.md) | Comments | Yes | Yes ||
|| **HAS_PHONE**
[`char`](../data-types.md) | `phone` field completion flag. Allowed values `Y` or `N` | Yes | No ||
|| **HAS_EMAIL**
[`char`](../data-types.md) | Email field completion flag. Allowed values `Y` or `N` | Yes | No ||
|| **HAS_IMOL**
[`char`](../data-types.md) | Linked Open Channel presence flag. Allowed values `Y` or `N` | Yes | No ||
|| **ASSIGNED_BY_ID**
[`user`](../data-types.md) | Identifier of the user assigned to the lead | Yes | Yes ||
|| **CREATED_BY_ID**
[`user`](../data-types.md) | Identifier of the user who created the lead | Yes | No ||
|| **MODIFY_BY_ID**
[`user`](../data-types.md) | Last change author identifier | Yes | No ||
|| **MOVED_BY_ID**
[`user`](../data-types.md) | Identifier of the author who moved the item to the current stage | Yes | No ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Create date | Yes | No ||
|| **DATE_MODIFY**
[`datetime`](../data-types.md) | Change date | Yes | No ||
|| **MOVED_TIME**
[`datetime`](../data-types.md) | Date the item was moved to the current stage | Yes | No ||
|| **COMPANY_ID**
[`crm_company`](./data-types.md) | Linking the lead to a company (Client->Company field) | Yes | Yes ||
|| **CONTACT_ID**
[`crm_contact`](./data-types.md) | Linking the lead to a contact. Obsolete field, currently not used. Kept for backward compatibility | Yes | Yes ||
|| **CONTACT_IDS**
[`crm_contact`](./data-types.md) |  Linked contact identifier. Multiple.

When using [crm.lead.update](./leads/crm-lead-update.md) and [crm.lead.add](./leads/crm-lead-add.md), you can pass an array of contacts  | Yes | Yes ||
|| **IS_RETURN_CUSTOMER**
[`char`](../data-types.md) | Repeat lead indicator. Allowed values `Y` or `N` | Yes | No ||
|| **DATE_CLOSED**
[`datetime`](../data-types.md) | Closing date | Yes | No ||
|| **ORIGINATOR_ID**
[`string`](../data-types.md) | Data source identifier. Used only for linking to an external source. | Yes | Yes ||
|| **ORIGIN_ID**
[`string`](../data-types.md) | Item identifier in the data source. Used only for linking to an external source. | Yes | Yes ||
|| **UTM_SOURCE**
[`string`](../data-types.md) | Ad system (Search Ads, Display Ads, and others). | Yes | Yes ||
|| **UTM_MEDIUM**
[`string`](../data-types.md) | Traffic type: CPC (ads), CPM (banners). | Yes | Yes ||
|| **UTM_CAMPAIGN**
[`string`](../data-types.md) | Advertising campaign designation. | Yes | Yes ||
|| **UTM_CONTENT**
[`string`](../data-types.md) | Campaign contents. For example, for contextual ads. | Yes | Yes ||
|| **UTM_TERM**
[`string`](../data-types.md) | Campaign search condition. For example, contextual advertising keywords. | Yes | Yes ||
|| **LAST_ACTIVITY_TIME**
[`datetime`](../data-types.md) | Last activity time. | Yes | No ||
|| **LAST_ACTIVITY_BY**
[`string`](../data-types.md) | Identifier of the user assigned to the last activity in this lead (for example, the one who created a new activity in the lead). | Yes | No ||
|| **PHONE**
[`crm_multifield`](./data-types.md) | Contact phone. Multiple | Yes | Yes ||
|| **EMAIL**
[`crm_multifield`](./data-types.md) | Email address. Multiple | Yes | Yes ||
|| **WEB**
[`crm_multifield`](./data-types.md) | Lead URL resources. Multiple | Yes | Yes ||
|| **IM**
[`crm_multifield`](./data-types.md) | Messengers. Multiple | Yes | Yes ||
|| **LINK**
[`crm_multifield`](./data-types.md) |  Links. Multiple. Service | Yes | Yes ||
|| **UF_CRM_xxx** | [Custom fields](./leads/userfield/index.md) | Yes | Yes ||
|#

## Companies

The field description is returned by the method [crm.company.fields](./companies/crm-company-fields.md)

#|
|| **Name**
`type` | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`](../data-types.md) | Company identifier | Yes | No ||
|| **TITLE**
[`string`](../data-types.md) | Name. Required | Yes | Yes ||
|| **COMPANY_TYPE**
[`crm_status`](./data-types.md) | Company type | Yes | Yes ||
|| **LOGO**
[`file`](../data-types.md) | Logo | Yes | Yes ||
|| **ADDRESS**
[`string`](../data-types.md) | Company address | Yes | Yes ||
|| **ADDRESS_2**
[`string`](../data-types.md) | Address line 2. In some countries, it is common to split the address into 2 parts. | Yes | Yes ||
|| **ADDRESS_CITY**
[`string`](../data-types.md) | City | Yes | Yes ||
|| **ADDRESS_POSTAL_CODE**
[`string`](../data-types.md) | Postal code | Yes | Yes ||
|| **ADDRESS_REGION**
[`string`](../data-types.md) | District | Yes | Yes ||
|| **ADDRESS_PROVINCE**
[`string`](../data-types.md) | Region | Yes | Yes ||
|| **ADDRESS_COUNTRY**
[`string`](../data-types.md) | Country | Yes | Yes ||
|| **ADDRESS_COUNTRY_CODE**
[`string`](../data-types.md) | Country code | Yes | Yes ||
|| **ADDRESS_LOC_ADDR_ID**
[`integer`](../data-types.md) | Location address identifier | Yes | Yes ||
|| **ADDRESS_LEGAL**
[`string`](../data-types.md) | Legal address | Yes | Yes ||
|| **REG_ADDRESS**
[`string`](../data-types.md) | Company legal address. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_2**
[`string`](../data-types.md) | Legal address line 2. In some countries, it is common to split the address into 2 parts.

Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_CITY**
[`string`](../data-types.md) | Legal address city. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_POSTAL_CODE**
[`string`](../data-types.md) | Legal address postal code. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_REGION**
[`string`](../data-types.md) | Legal address district. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_PROVINCE**
[`string`](../data-types.md) | Legal address region. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_COUNTRY**
[`string`](../data-types.md) | Legal address country. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_COUNTRY_CODE**
[`string`](../data-types.md) | Legal address country code. Obsolete, used for compatibility | Yes | Yes ||
|| **REG_ADDRESS_LOC_ADDR_ID**
[`integer`](../data-types.md) | Legal address location identifier. Obsolete, used for compatibility | Yes | Yes ||
|| **BANKING_DETAILS**
[`string`](../data-types.md) | Bank Company details | Yes | Yes ||
|| **INDUSTRY**
[`crm_status`](./data-types.md) | Industry | Yes | Yes ||
|| **EMPLOYEES**
[`crm_status`](./data-types.md) | Number of employees | Yes | Yes ||
|| **CURRENCY_ID**
[`crm_currency`](./data-types.md) | Currency | Yes | Yes ||
|| **REVENUE**
[`double`](../data-types.md) | Annual turnover | Yes | Yes ||
|| **OPENED**
[`char`](../data-types.md) | Available to all | Yes | Yes ||
|| **COMMENTS**
[`string`](../data-types.md) | Comments | Yes | Yes ||
|| **HAS_PHONE**
[`char`](../data-types.md) | Phone field completion check | Yes | No ||
|| **HAS_EMAIL**
[`char`](../data-types.md) | Email field completion check | Yes | No ||
|| **HAS_IMOL**
[`char`](../data-types.md) | Is Open Channel set | Yes | No ||
|| **IS_MY_COMPANY**
[`char`](../data-types.md) | My company | Yes | Yes ||
|| **ASSIGNED_BY_ID**
[`user`](../data-types.md) | Linked to user by ID | Yes | Yes ||
|| **CREATED_BY_ID**
[`user`](../data-types.md) | Created by | Yes | No ||
|| **MODIFY_BY_ID**
[`user`](../data-types.md) | Last change author identifier | Yes | No ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Create date | Yes | No ||
|| **DATE_MODIFY**
[`datetime`](../data-types.md) | Change date | Yes | No ||
|| **CONTACT_ID**
[`string`](../data-types.md) | Contact. Used only for linking to an external source. | Yes | Yes ||
|| **LEAD_ID**
[`crm_lead`](./data-types.md) | Identifier of the lead linked to the company | Yes | No ||
|| **ORIGINATOR_ID**
[`string`](../data-types.md) | Data source identifier. Used only for linking to an external source. | Yes | Yes ||
|| **ORIGIN_ID**
[`string`](../data-types.md) | Item identifier in the data source. Used only for linking to an external source. | Yes | Yes ||
|| **ORIGIN_VERSION**
[`string`](../data-types.md) | Original version. Used to protect data from accidental overwriting by an external system.

If the data was imported and has not changed in the external system, then such data can be edited in the CRM without fear that the next upload will lead to overwriting the data | Yes | Yes ||
|| **UTM_SOURCE**
[`string`](../data-types.md) | Ad system (Search Ads, Display Ads, and others). | Yes | Yes ||
|| **UTM_MEDIUM**
[`string`](../data-types.md) | Traffic type: CPC (ads), CPM (banners). | Yes | Yes ||
|| **UTM_CAMPAIGN**
[`string`](../data-types.md) | Advertising campaign designation. | Yes | Yes ||
|| **UTM_CONTENT**
[`string`](../data-types.md) | Campaign contents. For example, for contextual ads. | Yes | Yes ||
|| **UTM_TERM**
[`string`](../data-types.md) | Campaign search condition. For example, contextual advertising keywords. | Yes | Yes ||
|| **PARENT_ID_xxx**
[`crm_entity`](./data-types.md) | Relation fields.

If there are SPAs on the portal linked to contacts, for each such SPA there is a field storing the link between this SPA and the contact. The field itself stores the item identifier of such an SPA.

For example, the field `PARENT_ID_153` — link to SPA `entityTypeId=153`, stores the item identifier of this SPA linked to the current contact | Yes | Yes ||
|| **LAST_ACTIVITY_TIME**
[`datetime`](../data-types.md) | Last activity time. | Yes | No ||
|| **LAST_ACTIVITY_BY**
[`string`](../data-types.md) | Identifier of the user assigned to the last activity in this lead (for example, the one who created a new activity in the lead). | Yes | No ||
|| **PHONE**
[`crm_multifield`](./data-types.md) | Company phone. Multiple | Yes | Yes ||
|| **EMAIL**
[`crm_multifield`](./data-types.md) | Email address. Multiple | Yes | Yes ||
|| **WEB**
[`crm_multifield`](./data-types.md) | Company resource URLs. Multiple | Yes | Yes ||
|| **IM**
[`crm_multifield`](./data-types.md) | Messengers. Multiple | Yes | Yes ||
|| **LINK**
[`crm_multifield`](./data-types.md) |  Links. Multiple. Service | Yes | Yes ||
|#

## Contacts

The field description is returned by the method [crm.contact.fields](./contacts/crm-contact-fields.md)

#|
|| **Name**
`type` | **Description** | **Read** | **Write** ||
||**ID**
[`integer`](../data-types.md) | Contact identifier | Yes | No ||
||**HONORIFIC**
[`crm_status`](./data-types.md) | Salutation.

You can obtain the dictionary values using the [crm.status.list](./status/crm-status-list.md) method with a filter by `ENTITY_ID=HONORIFIC` | Yes | Yes ||
||**NAME**
[`string`](../data-types.md) | First name | Yes | Yes ||
||**SECOND_NAME**
[`string`](../data-types.md) | Middle name | Yes | Yes ||
||**LAST_NAME**
[`string`](../data-types.md) | Last name | Yes | Yes ||
||**PHOTO**
[`file`](../data-types.md) | Photo | Yes | Yes ||
||**BIRTHDATE**
[`date`](../data-types.md) | Date of birth. | Yes | Yes ||
||**TYPE_ID**
[`crm_status`](./data-types.md)| Contact type.

You can obtain the dictionary values using the [crm.status.list](./status/crm-status-list.md) method with a filter by `ENTITY_ID=CONTACT_TYPE` | Yes | Yes ||
||**SOURCE_ID**
[`crm_status`](./data-types.md) | Source.

You can obtain the dictionary values using the [crm.status.list](./status/crm-status-list.md) method with a filter by `ENTITY_ID=SOURCE`| Yes | Yes ||
||**SOURCE_DESCRIPTION**
[`string`](../data-types.md) | Additional info about the source | Yes | Yes ||
||**POST**
[`string`](../data-types.md) | Job title. | Yes | Yes ||
|| {% note tip "Deprecated fields" %}

Address fields in the contact are deprecated and used only for backward compatibility. To work with addresses, use [Company details](./requisites/index.md).

{% endnote %}| > | > | > ||
||**ADDRESS**
[`string`](../data-types.md) | Address (deprecated) | Yes | Yes ||
||**ADDRESS_2**
[`string`](../data-types.md) | Address line 2 (deprecated) | Yes | Yes ||
||**ADDRESS_CITY**
[`string`](../data-types.md) | City (deprecated) | Yes | Yes ||
||**ADDRESS_POSTAL_CODE**
[`string`](../data-types.md) | Postal code (deprecated) | Yes | Yes ||
||**ADDRESS_REGION**
[`string`](../data-types.md) | District (deprecated) | Yes | Yes ||
||**ADDRESS_PROVINCE**
[`string`](../data-types.md) | Region (deprecated) | Yes | Yes ||
||**ADDRESS_COUNTRY**
[`string`](../data-types.md) | Country (deprecated) | Yes | Yes ||
||**ADDRESS_COUNTRY_CODE**
[`string`](../data-types.md) | Country code (deprecated) | Yes | Yes ||
||**ADDRESS_LOC_ADDR_ID**
[`location`](./data-types.md) | Location address identifier (deprecated) | Yes | Yes ||
||**COMMENTS**
[`string`](../data-types.md) | Comment. Supports bb-codes | Yes | Yes ||
||**OPENED**
[`char`](../data-types.md) | Available to all. Can take values `Y` or `N`. Taken into account in access rights for roles with "All open" access level | Yes | Yes ||
||**EXPORT**
[`char`](../data-types.md) | Include in contact export. Can take values `Y` or `N`  | Yes | Yes ||
||**HAS_PHONE**
[`char`](../data-types.md) | Phone is set. Can take values `Y` or `N` | Yes | No ||
||**HAS_EMAIL**
[`char`](../data-types.md) | E-mail is set. Can take values `Y` or `N` | Yes | No ||
||**HAS_IMOL**
[`char`](../data-types.md) | Open Channel is set. Can take values `Y` or `N` | Yes | No ||
||**ASSIGNED_BY_ID**
[`user`](../data-types.md) | assigned user | Yes | Yes ||
||**CREATED_BY_ID**
[`user`](../data-types.md) | Created by | Yes | No ||
||**MODIFY_BY_ID**
[`user`](../data-types.md) | Changed by | Yes | No ||
||**DATE_CREATE**
[`datetime`](../data-types.md) | Create date | Yes | No ||
||**DATE_MODIFY**
[`datetime`](../data-types.md) | Change date | Yes | No ||
||**COMPANY_ID**
[`crm_company`](./data-types.md) | Primary company of the contact | Yes | Yes ||
||**COMPANY_IDS**
[`crm_company`](./data-types.md) | Linking a contact to companies. Multiple.

In methods [crm.contact.update](./contacts/crm-contact-update.md) and [crm.contact.add](./contacts/crm-contact-add.md), it is used to provide an array of companies.

In methods [crm.contact.list](./contacts/crm-contact-list.md) and [crm.contact.get](./contacts/crm-contact-get.md), this field does not exist, and you must use [crm.contact.company.items.get](./contacts/company/crm-contact-company-items-get.md) to retrieve the list of companies  | Yes | Yes ||
||**LEAD_ID**
[`crm_lead`](./data-types.md) | Identifier of the lead associated with the contact | Yes | No ||
|| {% note tip "Connection fields with external data sources" %}

If the contact was created by an external system, then:
- the `ORIGINATOR_ID` field stores the string identifier of that system
- the `ORIGIN_ID` field stores the string identifier of the contact in that external system
- the `ORIGIN_VERSION` field stores the version of the contact data in that external system

{% endnote %} | > | > | > ||
||**ORIGINATOR_ID**
[`string`](../data-types.md) | Identifier of the external system that is the source of data about this contact | Yes | Yes ||
||**ORIGIN_ID**
[`string`](../data-types.md) | Identifier of the contact in the external system | Yes | Yes ||
||**ORIGIN_VERSION**
[`string`](../data-types.md) | Version of the contact data in the external system. Used to protect data from accidental overwriting by an external system.

If the data was imported and has not changed in the external system, such data can be edited in the CRM without fear that the next export will lead to overwriting the data. | Yes | Yes ||
||**FACE_ID**
[`integer`](../data-types.md) | Link to persons from the `faceid` module | Yes | No ||
||**UTM_SOURCE**
[`string`](../data-types.md) | Ad system (Search Ads, Display Ads, and others). | Yes | Yes ||
||**UTM_MEDIUM**
[`string`](../data-types.md) | Traffic type: CPC (ads), CPM (banners). | Yes | Yes ||
||**UTM_CAMPAIGN**
[`string`](../data-types.md) | Advertising campaign designation. | Yes | Yes ||
||**UTM_CONTENT**
[`string`](../data-types.md) | Campaign contents. For example, for contextual ads. | Yes | Yes ||
||**UTM_TERM**
[`string`](../data-types.md) | Campaign search condition. For example, contextual advertising keywords. | Yes | Yes ||
||**PARENT_ID_...** | Relation fields.

If there are SPAs on the portal linked to contacts, for each such SPA there is a field storing the link between this SPA and the contact. The field itself stores the item identifier of such an SPA.

For example, the field `PARENT_ID_153` — link to SPA `entityTypeId=153`, stores the item identifier of this SPA linked to the current contact ||
||**LAST_ACTIVITY_TIME**
[`datetime`](../data-types.md) | Date of the last activity in the timeline | Yes | No ||
||**LAST_ACTIVITY_BY**
[`user`](../data-types.md) | Author of the last activity in the timeline | Yes | No ||
||**PHONE**
[`crm_multifield`](./data-types.md) | Phones. Multiple | Yes | Yes ||
||**EMAIL**
[`crm_multifield`](./data-types.md) | E-mail. Multiple | Yes | Yes ||
||**WEB**
[`crm_multifield`](./data-types.md) | Websites. Multiple | Yes | Yes ||
||**IM**
[`crm_multifield`](./data-types.md) | Messengers. Multiple | Yes | Yes ||
||**LINK**
[`crm_multifield`](./data-types.md) | Links. Multiple. Service | Yes | Yes ||
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
[`integer`](../data-types.md) | Identifier of the requisite.

Can be obtained using the [crm.requisite.list](./requisites/universal/crm-requisite-list.md) method.

Created automatically and is unique within the portal | Yes | No ||
|| **ENTITY_TYPE_ID**
[`integer`](../data-types.md) | Identifier of the parent entity type. Currently, this can only be:
- `3` — contact
- `4` — company

The [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) method returns identifiers for all CRM entity types| Yes | Yes ||
|| **ENTITY_ID**
[`integer`](../data-types.md) | Identifier of the parent entity (contact or company).

The identifier can be obtained using the [crm.company.list](./companies/crm-company-list.md) method for a company and the [crm.contact.list](./contacts/crm-contact-list.md) method for a contact. | Yes | Yes ||
|| **PRESET_ID**
[`integer`](../data-types.md) | Requisite template identifier.

Template identifiers can be obtained using the [crm.requisite.preset.list](./requisites/presets/crm-requisite-preset-list.md) method. | Yes | Yes ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Create date | Yes | No ||
|| **DATE_MODIFY**
[`datetime`](../data-types.md) | Change date | Yes | No ||
|| **CREATED_BY_ID**
[`user`](../data-types.md) | Identifier of the user who created the requisite. | Yes | No ||
|| **MODIFY_BY_ID**
[`user`](../data-types.md) | Identifier of the user who changed the requisite. | Yes | No ||
|| **NAME**
[`string`](../data-types.md) | Requisite name. | Yes | Yes ||
|| **CODE**
[`string`](../data-types.md) | Character code of the requisite. | Yes | Yes ||
|| **XML_ID**
[`string`](../data-types.md) | Foreign key, used for exchange operations.

Identifier of the external information base object.

The purpose of the field may be changed by the end developer. | Yes | Yes ||
|| **ORIGINATOR_ID**
[`string`](../data-types.md) | External information base identifier.

The purpose of the field may be changed by the end developer. | Yes | Yes ||
|| **ACTIVE**
[`char`](../data-types.md) | Activity flag.

Uses values `Y` or `N`.

Currently, the field does not actually affect anything. | Yes | Yes ||
|| **ADDRESS_ONLY**
[`char`](../data-types.md) | Status flag, when the requisite is used only for storing an address.

Uses values `Y` or `N`. When the value is `Y`, Company details are not shown in the entity card, but the address is displayed. | Yes | Yes ||
|| **SORT**
[`integer`](../data-types.md) | Sorting. The order in the entity's requisite list when there are multiple requisites. | Yes | Yes ||
|| **RQ_NAME**
[`string`](../data-types.md) | Full Name | Yes | Yes ||
|| **RQ_FIRST_NAME**
[`string`](../data-types.md) | First name | Yes | Yes ||
|| **RQ_LAST_NAME**
[`string`](../data-types.md) | Last name | Yes | Yes ||
|| **RQ_SECOND_NAME**
[`string`](../data-types.md) | Middle name | Yes | Yes ||
|| **RQ_COMPANY_ID**
[`string`](../data-types.md) | Organization identifier | Yes | Yes ||
|| **RQ_COMPANY_NAME**
[`string`](../data-types.md) | Abbreviated organization name | Yes | Yes ||
|| **RQ_COMPANY_FULL_NAME**
[`string`](../data-types.md) | Full organization name | Yes | Yes ||
|| **RQ_COMPANY_REG_DATE**
[`string`](../data-types.md) | State registration date | Yes | Yes ||
|| **RQ_DIRECTOR**
[`string`](../data-types.md) | General Director | Yes | Yes ||
|| **RQ_ACCOUNTANT**
[`string`](../data-types.md) | Chief Accountant | Yes | Yes ||
|| **RQ_CEO_NAME**
[`string`](../data-types.md) | Full Name of the first manager | Yes | Yes ||
|| **RQ_CEO_WORK_POS**
[`string`](../data-types.md) | Position of the first manager | Yes | Yes ||
|| **RQ_CONTACT**
[`string`](../data-types.md) | Contact person | Yes | Yes ||
|| **RQ_EMAIL**
[`string`](../data-types.md) | E-Mail | Yes | Yes ||
|| **RQ_PHONE**
[`string`](../data-types.md) | Phone | Yes | Yes ||
|| **RQ_FAX**
[`string`](../data-types.md) | Fax | Yes | Yes ||
|| **RQ_IDENT_TYPE**
[`crm_status`](./data-types.md) | Identification method | Yes | Yes ||
|| **RQ_IDENT_DOC**
[`string`](../data-types.md) | Document type | Yes | Yes ||
|| **RQ_IDENT_DOC_SER**
[`string`](../data-types.md) | Series | Yes | Yes ||
|| **RQ_IDENT_DOC_NUM**
[`string`](../data-types.md) | Number | Yes | Yes ||
|| **RQ_IDENT_DOC_PERS_NUM**
[`string`](../data-types.md) | Personal number | Yes | Yes ||
|| **RQ_IDENT_DOC_DATE**
[`string`](../data-types.md) | Date of issue | Yes | Yes ||
|| **RQ_IDENT_DOC_ISSUED_BY**
[`string`](../data-types.md) | Issued by | Yes | Yes ||
|| **RQ_IDENT_DOC_DEP_CODE**
[`string`](../data-types.md) | Department code | Yes | Yes ||
|| **RQ_INN**
[`string`](../data-types.md) | TIN | Yes | Yes ||
|| **RQ_KPP**
[`string`](../data-types.md) | KPP | Yes | Yes ||
|| **RQ_USRLE**
[`string`](../data-types.md) | Handelsregisternummer (for country DE) | Yes | Yes ||
|| **RQ_IFNS**
[`string`](../data-types.md) | IFNS | Yes | Yes ||
|| **RQ_OGRN**
[`string`](../data-types.md) | OGRN | Yes | Yes ||
|| **RQ_OGRNIP**
[`string`](../data-types.md) | OGRNIP | Yes | Yes ||
|| **RQ_OKPO**
[`string`](../data-types.md) | OKPO | Yes | Yes ||
|| **RQ_OKTMO**
[`string`](../data-types.md) | OKTMO | Yes | Yes ||
|| **RQ_OKVED**
[`string`](../data-types.md) | OKVED | Yes | Yes ||
|| **RQ_EDRPOU**
[`string`](../data-types.md) | EDRPOU | Yes | Yes ||
|| **RQ_DRFO**
[`string`](../data-types.md) | DRFO | Yes | Yes ||
|| **RQ_KBE**
[`string`](../data-types.md) | KBE | Yes | Yes ||
|| **RQ_IIN**
[`string`](../data-types.md) | IIN | Yes | Yes ||
|| **RQ_BIN**
[`string`](../data-types.md) | BIN | Yes | Yes ||
|| **RQ_ST_CERT_SER**
[`string`](../data-types.md) | State registration certificate series | Yes | Yes ||
|| **RQ_ST_CERT_NUM**
[`string`](../data-types.md) | State registration certificate number | Yes | Yes ||
|| **RQ_ST_CERT_DATE**
[`string`](../data-types.md) | State registration certificate date | Yes | Yes ||
|| **RQ_VAT_PAYER**
[`char`](../data-types.md) | VAT Payer (for country UA).

Uses values `Y` or `N` | Yes | Yes ||
|| **RQ_VAT_ID**
[`string`](../data-types.md) | VAT ID (Value Added Tax identification number) | Yes | Yes ||
|| **RQ_VAT_CERT_SER**
[`string`](../data-types.md) | VAT certificate series | Yes | Yes ||
|| **RQ_VAT_CERT_NUM**
[`string`](../data-types.md) | VAT certificate number | Yes | Yes ||
|| **RQ_VAT_CERT_DATE**
[`string`](../data-types.md) | VAT certificate date | Yes | Yes ||
|| **RQ_RESIDENCE_COUNTRY**
[`string`](../data-types.md) | Country of residence | Yes | Yes ||
|| **RQ_BASE_DOC**
[`string`](../data-types.md) | Basis of action | Yes | Yes ||
|| **RQ_REGON**
[`string`](../data-types.md) | REGON (for country PL) | Yes | Yes ||
|| **RQ_KRS**
[`string`](../data-types.md) | KRS (for country PL) | Yes | Yes ||
|| **RQ_PESEL**
[`string`](../data-types.md) | PESEL (for country PL) | Yes | Yes ||
|| **RQ_LEGAL_FORM**
[`string`](../data-types.md) | Legal form (for country FR) | Yes | Yes ||
|| **RQ_SIRET**
[`string`](../data-types.md) | Siret Number (for country FR) | Yes | Yes ||
|| **RQ_SIREN**
[`string`](../data-types.md) | Siren Number (for country FR) | Yes | Yes ||
|| **RQ_CAPITAL**
[`string`](../data-types.md) | Share capital (for country FR) | Yes | Yes ||
|| **RQ_RCS**
[`string`](../data-types.md) | RCS (for country FR) | Yes | Yes ||
|| **RQ_CNPJ**
[`string`](../data-types.md) | CNPJ (for country BR) | Yes | Yes ||
|| **RQ_STATE_REG**
[`string`](../data-types.md) | State Registration (IE) (for country BR) | Yes | Yes ||
|| **RQ_MNPL_REG**
[`string`](../data-types.md) | Municipal Registration (IM) (for country BR) | Yes | Yes ||
|| **RQ_CPF**
[`string`](../data-types.md) | CPF (for country BR) | Yes | Yes ||
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
[`integer`](../data-types.md) | Bank requisite identifier. Created automatically and is unique within the portal | Yes | No ||
|| **ENTITY_TYPE_ID**
[`integer`](../data-types.md) | Parent object type identifier. Can only be `Attribute` (value `8`).

The method [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) returns object type identifiers | Yes | No ||
|| **ENTITY_ID**
[`integer`](../data-types.md) | Parent object identifier | Yes | Yes ||
|| **COUNTRY_ID**
[`integer`](../data-types.md) | Country identifier that corresponds to the set of bank requisite fields (see method [crm.requisite.preset.countries](./requisites/presets/crm-requisite-preset-countries.md) to get available values).

The bank requisite country code matches the country code in the linked requisite template, whose identifier is specified in the `ENTITY_ID` field| Yes | Yes ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Create date | Yes | No ||
|| **DATE_MODIFY**
[`datetime`](../data-types.md) | Change date | Yes | No ||
|| **CREATED_BY_ID**
[`user`](../data-types.md) | Identifier of the user who created the requisite | Yes | No ||
|| **MODIFY_BY_ID**
[`user`](../data-types.md) | Identifier of the user who changed the requisite | Yes | No ||
|| **NAME^*^**
[`string`](../data-types.md) | Bank requisite name | Yes | Yes ||
|| **CODE**
[`string`](../data-types.md) | Character code of the requisite. | Yes | Yes ||
|| **XML_ID**
[`string`](../data-types.md) | External key. Used for exchange operations. Identifier of the external information base object.

The purpose of the field may be changed by the end developer. Each application ensures the uniqueness of values in this field.

It is recommended to use a unique prefix to avoid collisions with other applications | Yes | Yes ||
|| **ACTIVE**
[`char`](../data-types.md) | Activity flag. Uses values `Y` or `N`.

Currently, the field does not actually affect anything | Yes | Yes ||
|| **SORT**
[`integer`](../data-types.md) | Sorting | Yes | Yes ||
|| **RQ_BANK_NAME**
[`string`](../data-types.md) | Bank name | Yes | Yes ||
|| **RQ_BANK_ADDR**
[`string`](../data-types.md) | Bank address | Yes | Yes ||
|| **RQ_BANK_CODE**
[`string`](../data-types.md) | Bank Code (for country BR) | Yes | Yes ||
|| **RQ_BANK_ROUTE_NUM**
[`string`](../data-types.md) | Bank Routing Number | Yes | Yes ||
|| **RQ_BIK**
[`string`](../data-types.md) | BIC | Yes | Yes ||
|| **RQ_CODEB**
[`string`](../data-types.md) | Bank Code (for country FR) | Yes | Yes ||
|| **RQ_CODEG**
[`string`](../data-types.md) | Branch Code (for country FR) | Yes | Yes ||
|| **RQ_RIB**
[`string`](../data-types.md) | RIB Key (for country FR) | Yes | Yes ||
|| **RQ_MFO**
[`string`](../data-types.md) | MFO | Yes | Yes ||
|| **RQ_ACC_NAME**
[`string`](../data-types.md) | Bank Account Holder Name | Yes | Yes ||
|| **RQ_ACC_NUM**
[`string`](../data-types.md) | Bank Account Number | Yes | Yes ||
|| **RQ_ACC_TYPE**
[`string`](../data-types.md) | Account Type (for country BR) | Yes | Yes ||
|| **RQ_AGENCY_NAME**
[`string`](../data-types.md) | Agency (for country BR) | Yes | Yes ||
|| **RQ_IIK**
[`string`](../data-types.md) | IIK | Yes | Yes ||
|| **RQ_ACC_CURRENCY**
[`string`](../data-types.md) | Account currency | Yes | Yes ||
|| **RQ_COR_ACC_NUM**
[`string`](../data-types.md) | Correspondent account | Yes | Yes ||
|| **RQ_IBAN**
[`string`](../data-types.md) | IBAN | Yes | Yes ||
|| **RQ_SWIFT**
[`string`](../data-types.md) | SWIFT | Yes | Yes ||
|| **RQ_BIC**
[`string`](../data-types.md) | BIC | Yes | Yes ||
|| **COMMENTS**
[`string`](../data-types.md) | Comment | Yes | Yes ||
|| **ORIGINATOR_ID**
[`string`](../data-types.md) | External information base identifier. The purpose of the field may be changed by the end developer | Yes | Yes ||
|#

### Templates of Requisites

The field description is returned by the method [crm.requisite.preset.fields](./requisites/presets/crm-requisite-preset-fields.md)

#|
|| **Name** | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`](../data-types.md) | Requisite identifier. Created automatically and is unique within the portal | Yes | No ||
|| **ENTITY_TYPE_ID**
[`integer`](../data-types.md) | Parent object type identifier.

The method [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) returns CRM object type identifiers | Yes | Yes ||
|| **COUNTRY_ID**
[`integer`](../data-types.md) | Country identifier that corresponds to the set of fields in the requisite template (to get available values, see method [crm.requisite.preset.countries](./requisites/presets/crm-requisite-preset-countries.md)) | Yes | Yes ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Create date | Yes | No ||
|| **DATE_MODIFY**
[`datetime`](../data-types.md) | Modification date. Contains an empty string if the template has not changed since creation. | Yes | No ||
|| **CREATED_BY_ID**
[`user`](../data-types.md) | Identifier of the user who created the requisite | Yes | No ||
|| **MODIFY_BY_ID**
[`user`](../data-types.md) | Identifier of the user who changed the requisite | Yes | No ||
|| **NAME**
[`string`](../data-types.md) | Requisite name. | Yes | Yes ||
|| **XML_ID**
[`string`](../data-types.md) | Foreign key. Used for exchange operations. Identifier of an object in an external information base.

The purpose of the field may change depending on the end developer.

Each application ensures the uniqueness of values in this field. It is recommended to use a unique prefix to avoid collisions with other applications.

In CRM, values like `#CRM_REQUISITE_PRESET_DEF_...` are reserved for identifying default templates. These identifiers should not be used for your own purposes, as this may lead to logic violations. | Yes | Yes ||
|| **ACTIVE**
[`char`](../data-types.md) | Activity flag. Uses values `Y` or `N`. Determines the availability of the template in the selection list when adding requisites. | Yes | Yes ||
|| **SORT**
[`integer`](../data-types.md) | Sorting | Yes | Yes ||
|#

### Fields of Requisites Templates

The field description is returned by the method [crm.requisite.preset.field.fields](./requisites/presets/fields/crm-requisite-preset-field-fields.md)

#|
||  **Name**
`type` | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`](../data-types.md) | Field identifier. Created automatically and is unique within the template. | Yes | No ||
|| **FIELD_NAME**
[`string`](../data-types.md) | Field name | Yes | Yes ||
|| **FIELD_TITLE**
[`string`](../data-types.md) | Alternative field name for a requisite.

The alternative name is displayed in various forms for filling out requisites. Depending on the specific form, the alternative name may or may not be used. | Yes | Yes ||
|| **SORT**
[`integer`](../data-types.md) | Sorting. The order in the template field list. | Yes | Yes ||
|| **IN_SHORT_LIST**
[`char`](../data-types.md) | Show in short list. Deprecated field, currently not used. Kept for backward compatibility. Can take values `Y` or `N`. | Yes | Yes ||
|#

### Addresses of Requisites

The field description is returned by the method [crm.address.fields](./requisites/addresses/crm-address-fields.md)

#|
|| **Name** | **Description** | **Read** | **Write** ||
|| **TYPE_ID**
[`integer`](../data-types.md) | Address type identifier. "Address type" enumeration item.

"Address type" enumeration items can be obtained using the [crm.enum.addresstype](./auxiliary/enum/crm-enum-address-type.md) method. | Yes | Yes  ||
|| **ENTITY_TYPE_ID**
[`integer`](../data-types.md) | Parent object type identifier.

Object type identifiers can be obtained using the [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) method.

{% note tip "" %}

Addresses can only be linked to Requisites (whereas Company details are already linked to companies or contacts) or Leads. For backward compatibility, the ability to link Addresses to Contacts or Companies has been kept. However, this connection is only possible on some old portals where the old address mode was specifically enabled by technical support.

{% endnote %} | Yes | Yes ||
|| **ENTITY_ID**
[`string`](../data-types.md) | Parent object identifier | Yes | Yes ||
|| **ADDRESS_1**
[`string`](../data-types.md) | Street, house, building, structure | Yes | Yes ||
|| **ADDRESS_2**
[`string`](../data-types.md) | Apartment / office | Yes | Yes ||
|| **CITY**
[`string`](../data-types.md) | City | Yes | Yes ||
|| **POSTAL_CODE**
[`string`](../data-types.md) | Postal code | Yes | Yes ||
|| **REGION**
[`string`](../data-types.md) | District | Yes | Yes ||
|| **PROVINCE**
[`string`](../data-types.md) | Region | Yes | Yes ||
|| **COUNTRY**
[`string`](../data-types.md) | Country | Yes | Yes ||
|| **COUNTRY_CODE**
[`string`](../data-types.md) | Country code | Yes | Yes ||
|| **LOC_ADDR_ID**
[`integer`](../data-types.md) | Location identifier.

This field contains the identifier of the address object in the `Location` module, associated with the CRM address object. Each CRM address corresponds to an address object in the module `location`. This can be used to copy an existing CRM address with location information that is not present in the CRM address fields.

If a `location` module address identifier is specified when creating an address, a copy of the address is created `location` and linked to the created CRM address. If, in this case, no values are specified for the string address fields, they will be filled from the location address.

However, if at least one string field was specified, only the specified fields will be saved in the CRM address, and their values will overwrite the corresponding values in the location address object. The same behavior applies when updating an address. | Yes | Yes ||
|| **ANCHOR_TYPE_ID**
[`integer`](../data-types.md) | Main parent object type identifier.

This field is for internal use. The value is filled automatically when an address is added.

Object type identifiers can be obtained using the [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) method.

This field contains the identifier of the attribute's parent object type (company or contact) if the address is linked to an attribute. If the address is linked to a lead, this value will be the lead type identifier. | Yes | No ||
|| **ANCHOR_ID**
[`integer`](../data-types.md) | This field is for internal use. The value is filled automatically when an address is added.

This field contains the identifier of the attribute's parent object (company or contact) if the address is linked to an attribute. If the address is linked to a lead, this value will be the lead identifier. | Yes | No ||
|#

## Activities

The field description is returned by the method [crm.activity.fields](./timeline/activities/activity-base/crm-activity-fields.md)

#|
|| **Name** | **Description** | **Read** | **Write** ||
|| **ID**
[`integer`](../data-types.md) | Activity identifier | Yes | No ||
|| **OWNER_ID**
[`integer`](../data-types.md) | Owner identifier, immutable | Yes | Yes ||
|| **OWNER_TYPE_ID**
[`crm_enum_ownertype`](./data-types.md#activity-enums) | Owner type, immutable | Yes | Yes ||
|| **TYPE_ID**
[`crm_enum_activitytype`](./data-types.md#activity-enums) | Type, immutable | Yes | Yes ||
|| **PROVIDER_ID**
[`string`](../data-types.md) | Provider identifier | Yes | Yes ||
|| **PROVIDER_TYPE_ID**
[`string`](../data-types.md) | Provider type identifier | Yes | Yes ||
|| **PROVIDER_GROUP_ID**
[`string`](../data-types.md) | Connector type | Yes | Yes ||
|| **ASSOCIATED_ENTITY_ID**
[`integer`](../data-types.md) | Entity identifier related to the activity | Yes | No ||
|| **SUBJECT**
[`string`](../data-types.md) | Subject, activity title | Yes | Yes ||
|| **START_TIME**
[`datetime`](../data-types.md) | Start time | Yes | Yes ||
|| **END_TIME**
[`datetime`](../data-types.md) | Completion time | Yes | Yes ||
|| **DEADLINE**
[`datetime`](../data-types.md) | Due date. This field is not set directly; the value is taken from `START_TIME` for calls and meetings and from `END_TIME` for tasks. | Yes | Yes ||
|| **COMPLETED**
[`char`](../data-types.md) | Completed | Yes | Yes ||
|| **STATUS**
[`crm_enum_activitystatus`](./data-types.md#activity-enums) | Status | Yes | Yes ||
|| **RESPONSIBLE_ID**
[`user`](../data-types.md) | assigned user | Yes | Yes ||
|| **PRIORITY**
[`crm_enum_activitypriority`](./data-types.md#activity-enums) | Importance | Yes | Yes ||
|| **NOTIFY_TYPE**
[`crm_enum_activitynotifytype`](./data-types.md#activity-enums) | Notification type | Yes | Yes ||
|| **NOTIFY_VALUE**
[`integer`](../data-types.md) | Notification parameter | Yes | Yes ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description | Yes | Yes ||
|| **DESCRIPTION_TYPE**
[`crm_enum_contenttype`](./data-types.md#activity-enums) | Description type | Yes | Yes ||
|| **DIRECTION**
[`crm_enum_activitydirection`](./data-types.md#activity-enums) | Activity direction: inbound/outbound. Relevant for calls and emails, not used for meetings. | Yes | Yes ||
|| **LOCATION**
[`string`](../data-types.md) | Location | Yes | Yes ||
|| **CREATED**
[`datetime`](../data-types.md) | Create date | Yes | No ||
|| **AUTHOR_ID**
[`user`](../data-types.md) | Activity creator | Yes | Yes ||
|| **LAST_UPDATED**
[`datetime`](../data-types.md) | Last update date | Yes | No ||
|| **EDITOR_ID**
[`user`](../data-types.md) | Changed by | Yes | No ||
|| **SETTINGS**
[`object`](../data-types.md) | Settings | Yes | Yes ||
|| **ORIGIN_ID**
[`string`](../data-types.md) | Item identifier in the data source. Used only for linking to an external source. | Yes | Yes ||
|| **ORIGINATOR_ID**
[`string`](../data-types.md) | Data source identifier. Used only for linking to an external source. | Yes | Yes ||
|| **RESULT_STATUS**
[`integer`](../data-types.md) | Unused field, remains for compatibility | Yes | Yes ||
|| **RESULT_STREAM**
[`integer`](../data-types.md) | Report statistics | Yes | Yes ||
|| **RESULT_SOURCE_ID**
[`string`](../data-types.md) | Unused field, remains for compatibility | Yes | Yes ||
|| **PROVIDER_PARAMS**
[`object`](../data-types.md) | Provider parameters | Yes | Yes ||
|| **PROVIDER_DATA**
[`string`](../data-types.md) | Provider data | Yes | Yes ||
|| **RESULT_MARK**
[`integer`](../data-types.md) | Unused field, remains for compatibility | Yes | Yes ||
|| **RESULT_VALUE**
[`double`](../data-types.md) | Unused field, remains for compatibility | Yes | Yes ||
|| **RESULT_SUM**
[`double`](../data-types.md) | Unused field, remains for compatibility | Yes | Yes ||
|| **RESULT_CURRENCY_ID**
[`string`](../data-types.md) | Unused field, remains for compatibility | Yes | Yes ||
|| **AUTOCOMPLETE_RULE**
[`integer`](../data-types.md) | Autofill | Yes | Yes ||
|| **BINDINGS**
[`crm_activity_binding`](./data-types.md#crm_activity_binding) | Links | Yes | No ||
|| **COMMUNICATIONS**
[`crm_activity_communication`](./data-types.md) | Communication channel. Multiple, mandatory | Yes | Yes ||
|| **FILES**
[`diskfile`](./data-types.md#diskfile) | Added files. Multiple | Yes | Yes ||
|| **WEBDAV_ELEMENTS**
[`diskfile`](./data-types.md#diskfile) | Added files. Multiple. Deprecated, kept for compatibility | Yes | Yes ||
|| **IS_INCOMING_CHANNEL**
[`char`](../data-types.md) | Whether the activity is inbound, i.e., created as a result of an incoming client Salutation to a communication channel | Yes | No ||
|#
