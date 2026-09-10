# Fields of Main CRM Objects

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This page explains the purpose and type of standard fields for the main CRM objects. Refer to it when building `select`, `filter`, and `fields` for CRM methods.

The available fields and their settings may differ between Bitrix24 accounts. Before creating an integration, retrieve the current field description using the corresponding `crm.object_name.fields` method, for example, [crm.deal.fields](./deals/crm-deal-fields.md) for a deal. The tables below provide a quick reference and do not replace the method response.

The methods return the current availability attributes for each field:

- `isReadOnly: true`—the field is read-only
- `isImmutable: true`—the field value can be set when creating an object but cannot be changed after it is retained
- `isRequired: true`—the field is required
- `isMultiple: true`—the field accepts multiple values
- `isDynamic: true`—the field was created in a specific Bitrix24 account and is not part of the standard set

To prepare a request:

1. Select an object and call its `.fields` method
2. Find the field by its identifier and check its type and availability attributes
3. Retrieve allowed values for dynamic directories, such as stages and custom fields
4. Pass the field to a create, update, retrieve, or list method according to the returned attributes

Field length limits are described in the article [CRM Field Length Limits](./field-length-limits.md).

## Deals

The field description is returned by the method [crm.deal.fields](./deals/crm-deal-fields.md)

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Deal identifier ||
|| **TITLE**
[`string`](../data-types.md) | Name ||
|| **TYPE_ID**
[`crm_status`](./data-types.md) | Deal type. Used only for linking to an external source. ||
|| **CATEGORY_ID**
[`crm_category`](./data-types.md) | Pipeline identifier. Immutable. If this field is not passed when creating a deal, the deal is created in the default pipeline ||
|| **STAGE_ID**
[`crm_status`](./data-types.md) | Deal stage identifier. The set of stages depends on the pipeline. Retrieve stages using [crm.status.list](./status/crm-status-list.md) with an `ENTITY_ID` filter: `DEAL_STAGE` for the default pipeline or `DEAL_STAGE_{CATEGORY_ID}` for another pipeline ||
|| **STAGE_SEMANTIC_ID**
[`string`](../data-types.md) | Deal stage group. Possible values:
- `P` — stage in progress
- `S` — successful stage
- `F` — unsuccessful stage ||
|| **IS_NEW**
[`char`](../data-types.md) | New deal flag (deals in the first stage) ||
|| **IS_RECURRING**
[`char`](../data-types.md) | Recurring deal template flag. If `Y` is set, then it is a template, not a deal ||
|| **IS_RETURN_CUSTOMER**
[`char`](../data-types.md) | Repeat deal indicator. Allowed values are `Y` and `N` ||
|| **IS_REPEATED_APPROACH**
[`char`](../data-types.md) | Indicates manual amount calculation. Allowed values are `Y` and `N` ||
|| **PROBABILITY**
[`integer`](../data-types.md) | Probability ||
|| **CURRENCY_ID**
[`crm_currency`](./data-types.md) | Deal currency identifier ||
|| **OPPORTUNITY**
[`double`](../data-types.md) | Amount ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../data-types.md) | Repeat Salutation ||
|| **TAX_VALUE**
[`double`](../data-types.md) | Tax rate ||
|| **COMPANY_ID**
[`crm_company`](./data-types.md) | Linked company identifier ||
|| **CONTACT_ID**
[`crm_contact`](./data-types.md) | Linked contact identifier. Deprecated. Retained for compatibility ||
|| **CONTACT_IDS**
[`crm_contact`](./data-types.md) | Linked contact identifier. Multiple.

When using [crm.deal.update](./deals/crm-deal-update.md) and [crm.deal.add](./deals/crm-deal-add.md), you can pass an array of contacts.

In the [crm.deal.list](./deals/crm-deal-list.md) and [crm.deal.get](./deals/crm-deal-get.md) methods, this field does not exist, and you must use [crm.deal.contact.items.get](./deals/contacts/crm-deal-contact-items-get.md) to retrieve the list of contacts.

To clear the field, use [crm.deal.contact.items.delete](./deals/contacts/crm-deal-contact-items-delete.md); to replace the value, use [crm.deal.contact.items.set](./deals/contacts/crm-deal-contact-items-set.md) ||
|| **QUOTE_ID**
[`crm_quote`](./data-types.md) | Quote identifier. Read-only. Deprecated. Use the [crm.quote.list](./quote/crm-quote-list.md) method with a filter by deal ||
|| **BEGINDATE**
[`date`](../data-types.md) | Start date ||
|| **CLOSEDATE**
[`date`](../data-types.md) | End date ||
|| **OPENED**
[`char`](../data-types.md) | Available to all ||
|| **CLOSED**
[`char`](../data-types.md) | Is the deal completed ||
|| **COMMENTS**
[`string`](../data-types.md) | Comments ||
|| **ASSIGNED_BY_ID**
[`user`](../data-types.md) | Linked to user by ID ||
|| **CREATED_BY_ID**
[`user`](../data-types.md) | Created by user ||
|| **MODIFY_BY_ID**
[`user`](../data-types.md) | Last change author identifier ||
|| **MOVED_BY_ID**
[`user`](../data-types.md) | Author identifier who moved the item to the current stage ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Create date ||
|| **DATE_MODIFY**
[`datetime`](../data-types.md) | Change date ||
|| **MOVED_TIME**
[`datetime`](../data-types.md) | Date the item was moved to the current stage ||
|| **SOURCE_ID**
[`string`](../data-types.md) | Source identifier. Determines the source of the deal (callback, advertisement, Email, etc.).

A list of possible identifiers can be obtained using the [crm.status.list](./status/crm-status-list.md) method with the `filter[ENTITY_ID]=SOURCE` filter. ||
|| **SOURCE_DESCRIPTION**
[`string`](../data-types.md) | Additional information about the source. Text field. ||
|| **ADDITIONAL_INFO**
[`string`](../data-types.md) | Additional information. ||
|| **LEAD_ID**
[`crm_lead`](./data-types.md) | Linked lead identifier. ||
|| **LOCATION_ID**
[`location`](./data-types.md) | Customer location. Service field, not recommended for use. ||
|| **ORIGINATOR_ID**
[`string`](../data-types.md) | Data source identifier. Used only for linking to an external source. ||
|| **ORIGIN_ID**
[`string`](../data-types.md) | Item identifier in the data source. Used only for linking to an external source. ||
|| **UTM_SOURCE**
[`string`](../data-types.md) | Ad system (Search Ads, Display Ads, and others). ||
|| **UTM_MEDIUM**
[`string`](../data-types.md) | Traffic type: CPC (ads), CPM (banners). ||
|| **UTM_CAMPAIGN**
[`string`](../data-types.md) | Advertising campaign designation. ||
|| **UTM_CONTENT**
[`string`](../data-types.md) | Campaign contents. For example, for contextual ads. ||
|| **UTM_TERM**
[`string`](../data-types.md) | Campaign search condition. For example, contextual advertising keywords. ||
|| **PARENT_ID_xxx**
[`crm_entity`](./data-types.md) | Relation fields.

If there are SPAs on the portal linked to contacts, then for each such SPA, there is a field that stores the connection between this SPA and the contact. The field itself stores the item identifier of that SPA.

For example, the `PARENT_ID_153` field — a connection with SPA `entityTypeId=153`, stores the item identifier of this SPA linked to the current contact. ||
|| **LAST_ACTIVITY_BY**
[`string`](../data-types.md) | Identifier of the user assigned to the last activity in this lead (for example, the one who created a new activity in the lead). ||
|| **LAST_ACTIVITY_TIME**
[`datetime`](../data-types.md) | Last activity time. ||
|| **UF_CRM_xxx** | [Custom fields](./deals/user-defined-fields/index.md) ||
|#

## Leads

The field description is returned by the method [crm.lead.fields](./leads/crm-lead-fields.md)

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Integer lead identifier. ||
|| **TITLE**
[`string`](../data-types.md) | Lead name. ||
|| **HONORIFIC**
[`crm_status`](./data-types.md) | Salutation. Status from the directory.

A list of possible identifiers can be obtained using the [crm.status.list](./status/crm-status-list.md) method with the `filter[ENTITY_ID]=HONORIFIC` filter. ||
|| **NAME**
[`string`](../data-types.md) |  Contact first name. ||
|| **SECOND_NAME**
[`string`](../data-types.md) |  Contact middle name. ||
|| **LAST_NAME**
[`string`](../data-types.md) |  Contact last name. ||
|| **BIRTHDATE**
[`date`](../data-types.md) | Date of birth. ||
|| **COMPANY_TITLE**
[`string`](../data-types.md) | Name of the company linked to the lead. ||
|| **SOURCE_ID**
[`crm_status`](./data-types.md) | Source identifier. Status from the directory.

A list of possible identifiers can be obtained using the [crm.status.list](./status/crm-status-list.md) method with the `filter[ENTITY_ID]=SOURCE` filter. ||
|| **SOURCE_DESCRIPTION**
[`string`](../data-types.md) | Source description. ||
|| **STATUS_ID**
[`crm_status`](./data-types.md) | Lead stage identifier. Status from the directory.

A list of possible identifiers can be obtained using the [crm.status.list](./status/crm-status-list.md) method with the `filter[ENTITY_ID]=STATUS` filter. ||
|| **STATUS_DESCRIPTION**
[`string`](../data-types.md) | Additional information about the stage. ||
|| **STATUS_SEMANTIC_ID**
[`string`](../data-types.md) | Status. Possible values:
- `F` (failed) — processed unsuccessfully
- `S` (success) — processed successfully
- `P` (processing) — lead is being processed ||
|| **POST**
[`string`](../data-types.md) | Job title ||
|| **ADDRESS**
[`string`](../data-types.md) | Contact address. ||
|| **ADDRESS_2**
[`string`](../data-types.md) | Address line 2. In some countries, addresses are commonly split into two parts ||
|| **ADDRESS_CITY**
[`string`](../data-types.md) | City ||
|| **ADDRESS_POSTAL_CODE**
[`string`](../data-types.md) | Postal code ||
|| **ADDRESS_REGION**
[`string`](../data-types.md) | District ||
|| **ADDRESS_PROVINCE**
[`string`](../data-types.md) | Region ||
|| **ADDRESS_COUNTRY**
[`string`](../data-types.md) | Country ||
|| **ADDRESS_COUNTRY_CODE**
[`string`](../data-types.md) | Country code ||
|| **ADDRESS_LOC_ADDR_ID**
[`integer`](../data-types.md) | Address identifier from the locations module ||
|| **CURRENCY_ID**
[`crm_currency`](./data-types.md) | Currency identifier ||
|| **OPPORTUNITY**
[`double`](../data-types.md) | Estimated amount ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../data-types.md) | Manual amount calculation flag. Allowed values `Y` or `N` ||
|| **OPENED**
[`char`](../data-types.md) | Available to all. Allowed values `Y` or `N` ||
|| **COMMENTS**
[`string`](../data-types.md) | Comments ||
|| **HAS_PHONE**
[`char`](../data-types.md) | `phone` field completion flag. Allowed values `Y` or `N` ||
|| **HAS_EMAIL**
[`char`](../data-types.md) | Email field completion flag. Allowed values `Y` or `N` ||
|| **HAS_IMOL**
[`char`](../data-types.md) | Linked Open Channel presence flag. Allowed values `Y` or `N` ||
|| **ASSIGNED_BY_ID**
[`user`](../data-types.md) | Identifier of the user assigned to the lead ||
|| **CREATED_BY_ID**
[`user`](../data-types.md) | Identifier of the user who created the lead ||
|| **MODIFY_BY_ID**
[`user`](../data-types.md) | Last change author identifier ||
|| **MOVED_BY_ID**
[`user`](../data-types.md) | Identifier of the author who moved the item to the current stage ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Create date ||
|| **DATE_MODIFY**
[`datetime`](../data-types.md) | Change date ||
|| **MOVED_TIME**
[`datetime`](../data-types.md) | Date the item was moved to the current stage ||
|| **COMPANY_ID**
[`crm_company`](./data-types.md) | Linking the lead to a company (Client->Company field) ||
|| **CONTACT_ID**
[`crm_contact`](./data-types.md) | Linking the lead to a contact. Obsolete field, currently not used. Kept for backward compatibility ||
|| **CONTACT_IDS**
[`crm_contact`](./data-types.md) |  Linked contact identifier. Multiple.

When using [crm.lead.update](./leads/crm-lead-update.md) and [crm.lead.add](./leads/crm-lead-add.md), you can pass an array of contacts  ||
|| **IS_RETURN_CUSTOMER**
[`char`](../data-types.md) | Repeat lead indicator. Allowed values `Y` or `N` ||
|| **DATE_CLOSED**
[`datetime`](../data-types.md) | Closing date ||
|| **ORIGINATOR_ID**
[`string`](../data-types.md) | Data source identifier. Used only for linking to an external source. ||
|| **ORIGIN_ID**
[`string`](../data-types.md) | Item identifier in the data source. Used only for linking to an external source. ||
|| **UTM_SOURCE**
[`string`](../data-types.md) | Ad system (Search Ads, Display Ads, and others). ||
|| **UTM_MEDIUM**
[`string`](../data-types.md) | Traffic type: CPC (ads), CPM (banners). ||
|| **UTM_CAMPAIGN**
[`string`](../data-types.md) | Advertising campaign designation. ||
|| **UTM_CONTENT**
[`string`](../data-types.md) | Campaign contents. For example, for contextual ads. ||
|| **UTM_TERM**
[`string`](../data-types.md) | Campaign search condition. For example, contextual advertising keywords. ||
|| **LAST_ACTIVITY_TIME**
[`datetime`](../data-types.md) | Last activity time. ||
|| **LAST_ACTIVITY_BY**
[`string`](../data-types.md) | Identifier of the user assigned to the last activity in this lead (for example, the one who created a new activity in the lead). ||
|| **PHONE**
[`crm_multifield`](./data-types.md) | Contact phone. Multiple ||
|| **EMAIL**
[`crm_multifield`](./data-types.md) | Email address. Multiple ||
|| **WEB**
[`crm_multifield`](./data-types.md) | Lead URL resources. Multiple ||
|| **IM**
[`crm_multifield`](./data-types.md) | Messengers. Multiple ||
|| **LINK**
[`crm_multifield`](./data-types.md) |  Links. Multiple. Service ||
|| **UF_CRM_xxx** | [Custom fields](./leads/userfield/index.md) ||
|#

## Companies

The field description is returned by the method [crm.company.fields](./companies/crm-company-fields.md)

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Company identifier ||
|| **TITLE**
[`string`](../data-types.md) | Name. Required ||
|| **COMPANY_TYPE**
[`crm_status`](./data-types.md) | Company type ||
|| **LOGO**
[`file`](../data-types.md) | Logo ||
|| **ADDRESS**
[`string`](../data-types.md) | Company address ||
|| **ADDRESS_2**
[`string`](../data-types.md) | Address line 2. In some countries, it is common to split the address into 2 parts. ||
|| **ADDRESS_CITY**
[`string`](../data-types.md) | City ||
|| **ADDRESS_POSTAL_CODE**
[`string`](../data-types.md) | Postal code ||
|| **ADDRESS_REGION**
[`string`](../data-types.md) | District ||
|| **ADDRESS_PROVINCE**
[`string`](../data-types.md) | Region ||
|| **ADDRESS_COUNTRY**
[`string`](../data-types.md) | Country ||
|| **ADDRESS_COUNTRY_CODE**
[`string`](../data-types.md) | Country code ||
|| **ADDRESS_LOC_ADDR_ID**
[`integer`](../data-types.md) | Location address identifier ||
|| **ADDRESS_LEGAL**
[`string`](../data-types.md) | Legal address ||
|| **REG_ADDRESS**
[`string`](../data-types.md) | Company legal address. Obsolete, used for compatibility ||
|| **REG_ADDRESS_2**
[`string`](../data-types.md) | Legal address line 2. In some countries, it is common to split the address into 2 parts.

Obsolete, used for compatibility ||
|| **REG_ADDRESS_CITY**
[`string`](../data-types.md) | Legal address city. Obsolete, used for compatibility ||
|| **REG_ADDRESS_POSTAL_CODE**
[`string`](../data-types.md) | Legal address postal code. Obsolete, used for compatibility ||
|| **REG_ADDRESS_REGION**
[`string`](../data-types.md) | Legal address district. Obsolete, used for compatibility ||
|| **REG_ADDRESS_PROVINCE**
[`string`](../data-types.md) | Legal address region. Obsolete, used for compatibility ||
|| **REG_ADDRESS_COUNTRY**
[`string`](../data-types.md) | Legal address country. Obsolete, used for compatibility ||
|| **REG_ADDRESS_COUNTRY_CODE**
[`string`](../data-types.md) | Legal address country code. Obsolete, used for compatibility ||
|| **REG_ADDRESS_LOC_ADDR_ID**
[`integer`](../data-types.md) | Legal address location identifier. Obsolete, used for compatibility ||
|| **BANKING_DETAILS**
[`string`](../data-types.md) | Bank Company details ||
|| **INDUSTRY**
[`crm_status`](./data-types.md) | Industry ||
|| **EMPLOYEES**
[`crm_status`](./data-types.md) | Number of employees ||
|| **CURRENCY_ID**
[`crm_currency`](./data-types.md) | Currency ||
|| **REVENUE**
[`double`](../data-types.md) | Annual turnover ||
|| **OPENED**
[`char`](../data-types.md) | Available to all ||
|| **COMMENTS**
[`string`](../data-types.md) | Comments ||
|| **HAS_PHONE**
[`char`](../data-types.md) | Has phone ||
|| **HAS_EMAIL**
[`char`](../data-types.md) | Has email ||
|| **HAS_IMOL**
[`char`](../data-types.md) | Has Open Channel ||
|| **IS_MY_COMPANY**
[`char`](../data-types.md) | My company ||
|| **ASSIGNED_BY_ID**
[`user`](../data-types.md) | Linked to user by ID ||
|| **CREATED_BY_ID**
[`user`](../data-types.md) | Created by ||
|| **MODIFY_BY_ID**
[`user`](../data-types.md) | Last change author identifier ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Create date ||
|| **DATE_MODIFY**
[`datetime`](../data-types.md) | Change date ||
|| **CONTACT_ID**
[`string`](../data-types.md) | Contact. Used only for linking to an external source. ||
|| **LEAD_ID**
[`crm_lead`](./data-types.md) | Identifier of the lead linked to the company ||
|| **ORIGINATOR_ID**
[`string`](../data-types.md) | Data source identifier. Used only for linking to an external source. ||
|| **ORIGIN_ID**
[`string`](../data-types.md) | Item identifier in the data source. Used only for linking to an external source. ||
|| **ORIGIN_VERSION**
[`string`](../data-types.md) | Original version. Used to protect data from accidental overwriting by an external system.

If the data was imported and has not changed in the external system, then such data can be edited in the CRM without fear that the next upload will lead to overwriting the data ||
|| **UTM_SOURCE**
[`string`](../data-types.md) | Ad system (Search Ads, Display Ads, and others). ||
|| **UTM_MEDIUM**
[`string`](../data-types.md) | Traffic type: CPC (ads), CPM (banners). ||
|| **UTM_CAMPAIGN**
[`string`](../data-types.md) | Advertising campaign designation. ||
|| **UTM_CONTENT**
[`string`](../data-types.md) | Campaign contents. For example, for contextual ads. ||
|| **UTM_TERM**
[`string`](../data-types.md) | Campaign search condition. For example, contextual advertising keywords. ||
|| **PARENT_ID_xxx**
[`crm_entity`](./data-types.md) | Relation fields.

If the Bitrix24 account has SPAs linked to contacts, each such SPA has a linking field. The field stores the identifier of the SPA item linked to the current contact.

For example, the field `PARENT_ID_153` — link to SPA `entityTypeId=153`, stores the item identifier of this SPA linked to the current contact ||
|| **LAST_ACTIVITY_TIME**
[`datetime`](../data-types.md) | Last activity time. ||
|| **LAST_ACTIVITY_BY**
[`user`](../data-types.md) | Author of the latest timeline activity ||
|| **PHONE**
[`crm_multifield`](./data-types.md) | Company phone. Multiple ||
|| **EMAIL**
[`crm_multifield`](./data-types.md) | Email address. Multiple ||
|| **WEB**
[`crm_multifield`](./data-types.md) | Company resource URLs. Multiple ||
|| **IM**
[`crm_multifield`](./data-types.md) | Messengers. Multiple ||
|| **LINK**
[`crm_multifield`](./data-types.md) |  Links. Multiple. Service ||
|#

## Contacts

The field description is returned by the method [crm.contact.fields](./contacts/crm-contact-fields.md)

#|
|| **Name**
`type` | **Description** ||
||**ID**
[`integer`](../data-types.md) | Contact identifier ||
||**HONORIFIC**
[`crm_status`](./data-types.md) | Salutation.

You can obtain the dictionary values using the [crm.status.list](./status/crm-status-list.md) method with a filter by `ENTITY_ID=HONORIFIC` ||
||**NAME**
[`string`](../data-types.md) | First name ||
||**SECOND_NAME**
[`string`](../data-types.md) | Middle name ||
||**LAST_NAME**
[`string`](../data-types.md) | Last name ||
||**PHOTO**
[`file`](../data-types.md) | Photo ||
||**BIRTHDATE**
[`date`](../data-types.md) | Date of birth. ||
||**TYPE_ID**
[`crm_status`](./data-types.md)| Contact type.

You can obtain the dictionary values using the [crm.status.list](./status/crm-status-list.md) method with a filter by `ENTITY_ID=CONTACT_TYPE` ||
||**SOURCE_ID**
[`crm_status`](./data-types.md) | Source.

You can obtain the dictionary values using the [crm.status.list](./status/crm-status-list.md) method with a filter by `ENTITY_ID=SOURCE` ||
||**SOURCE_DESCRIPTION**
[`string`](../data-types.md) | Additional info about the source ||
||**POST**
[`string`](../data-types.md) | Job title. ||
|| {% note tip "Deprecated fields" %}

Address fields in the contact are deprecated and used only for backward compatibility. To work with addresses, use [Company details](./requisites/index.md).

{% endnote %}
| > ||
||**ADDRESS**
[`string`](../data-types.md) | Address (deprecated) ||
||**ADDRESS_2**
[`string`](../data-types.md) | Address line 2 (deprecated) ||
||**ADDRESS_CITY**
[`string`](../data-types.md) | City (deprecated) ||
||**ADDRESS_POSTAL_CODE**
[`string`](../data-types.md) | Postal code (deprecated) ||
||**ADDRESS_REGION**
[`string`](../data-types.md) | District (deprecated) ||
||**ADDRESS_PROVINCE**
[`string`](../data-types.md) | Region (deprecated) ||
||**ADDRESS_COUNTRY**
[`string`](../data-types.md) | Country (deprecated) ||
||**ADDRESS_COUNTRY_CODE**
[`string`](../data-types.md) | Country code (deprecated) ||
||**ADDRESS_LOC_ADDR_ID**
[`location`](./data-types.md) | Location address identifier (deprecated) ||
||**COMMENTS**
[`string`](../data-types.md) | Comment. Supports bb-codes ||
||**OPENED**
[`char`](../data-types.md) | Available to all. Can take values `Y` or `N`. Taken into account in access rights for roles with "All open" access level ||
||**EXPORT**
[`char`](../data-types.md) | Include in contact export. Can take values `Y` or `N`  ||
||**HAS_PHONE**
[`char`](../data-types.md) | Phone is set. Can take values `Y` or `N` ||
||**HAS_EMAIL**
[`char`](../data-types.md) | E-mail is set. Can take values `Y` or `N` ||
||**HAS_IMOL**
[`char`](../data-types.md) | Open Channel is set. Can take values `Y` or `N` ||
||**ASSIGNED_BY_ID**
[`user`](../data-types.md) | assigned user ||
||**CREATED_BY_ID**
[`user`](../data-types.md) | Created by ||
||**MODIFY_BY_ID**
[`user`](../data-types.md) | Changed by ||
||**DATE_CREATE**
[`datetime`](../data-types.md) | Create date ||
||**DATE_MODIFY**
[`datetime`](../data-types.md) | Change date ||
||**COMPANY_ID**
[`crm_company`](./data-types.md) | Primary company of the contact ||
||**COMPANY_IDS**
[`crm_company`](./data-types.md) | Linking a contact to companies. Multiple.

In methods [crm.contact.update](./contacts/crm-contact-update.md) and [crm.contact.add](./contacts/crm-contact-add.md), it is used to provide an array of companies.

In methods [crm.contact.list](./contacts/crm-contact-list.md) and [crm.contact.get](./contacts/crm-contact-get.md), this field does not exist, and you must use [crm.contact.company.items.get](./contacts/company/crm-contact-company-items-get.md) to retrieve the list of companies  ||
||**LEAD_ID**
[`crm_lead`](./data-types.md) | Identifier of the lead associated with the contact ||
|| {% note tip "Connection fields with external data sources" %}

If the contact was created by an external system, then:
- the `ORIGINATOR_ID` field stores the string identifier of that system
- the `ORIGIN_ID` field stores the string identifier of the contact in that external system
- the `ORIGIN_VERSION` field stores the version of the contact data in that external system

{% endnote %} | > ||
||**ORIGINATOR_ID**
[`string`](../data-types.md) | Identifier of the external system that is the source of data about this contact ||
||**ORIGIN_ID**
[`string`](../data-types.md) | Identifier of the contact in the external system ||
||**ORIGIN_VERSION**
[`string`](../data-types.md) | Version of the contact data in the external system. Used to protect data from accidental overwriting by an external system.

If the data was imported and has not changed in the external system, such data can be edited in the CRM without fear that the next export will lead to overwriting the data. ||
||**FACE_ID**
[`integer`](../data-types.md) | Link to persons from the `faceid` module ||
||**UTM_SOURCE**
[`string`](../data-types.md) | Ad system (Search Ads, Display Ads, and others). ||
||**UTM_MEDIUM**
[`string`](../data-types.md) | Traffic type: CPC (ads), CPM (banners). ||
||**UTM_CAMPAIGN**
[`string`](../data-types.md) | Advertising campaign designation. ||
||**UTM_CONTENT**
[`string`](../data-types.md) | Campaign contents. For example, for contextual ads. ||
||**UTM_TERM**
[`string`](../data-types.md) | Campaign search condition. For example, contextual advertising keywords. ||
||**PARENT_ID_...** | Relation fields.

If there are SPAs on the portal linked to contacts, for each such SPA there is a field storing the link between this SPA and the contact. The field itself stores the item identifier of such an SPA.

For example, the field `PARENT_ID_153` — link to SPA `entityTypeId=153`, stores the item identifier of this SPA linked to the current contact ||
||**LAST_ACTIVITY_TIME**
[`datetime`](../data-types.md) | Date of the last activity in the timeline ||
||**LAST_ACTIVITY_BY**
[`user`](../data-types.md) | Author of the last activity in the timeline ||
||**PHONE**
[`crm_multifield`](./data-types.md) | Phones. Multiple ||
||**EMAIL**
[`crm_multifield`](./data-types.md) | E-mail. Multiple ||
||**WEB**
[`crm_multifield`](./data-types.md) | Websites. Multiple ||
||**IM**
[`crm_multifield`](./data-types.md) | Messengers. Multiple ||
||**LINK**
[`crm_multifield`](./data-types.md) | Links. Multiple. Service ||
||**UF_CRM_xxx**  | custom fields. For example, `UF_CRM_25534736`.

Depending on the portal settings, contacts may have a set of custom fields of certain types. You can add a custom field to a contact using the [crm.contact.userfield.add](./contacts/userfield/crm-contact-userfield-add.md) method  ||
|#

## Company Details

### General Requisites

The field description is returned by the method [crm.requisite.fields](./requisites/universal/crm-requisite-fields.md)

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the requisite.

Can be obtained using the [crm.requisite.list](./requisites/universal/crm-requisite-list.md) method.

Created automatically and is unique within the portal ||
|| **ENTITY_TYPE_ID**
[`integer`](../data-types.md) | Identifier of the parent entity type. Currently, this can only be:
- `3` — contact
- `4` — company

The [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) method returns identifiers for all CRM object types ||
|| **ENTITY_ID**
[`integer`](../data-types.md) | Identifier of the parent entity (contact or company).

The identifier can be obtained using the [crm.company.list](./companies/crm-company-list.md) method for a company and the [crm.contact.list](./contacts/crm-contact-list.md) method for a contact. ||
|| **PRESET_ID**
[`integer`](../data-types.md) | Requisite template identifier.

Template identifiers can be obtained using the [crm.requisite.preset.list](./requisites/presets/crm-requisite-preset-list.md) method. ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Create date ||
|| **DATE_MODIFY**
[`datetime`](../data-types.md) | Change date ||
|| **CREATED_BY_ID**
[`user`](../data-types.md) | Identifier of the user who created the requisite. ||
|| **MODIFY_BY_ID**
[`user`](../data-types.md) | Identifier of the user who changed the requisite. ||
|| **NAME**
[`string`](../data-types.md) | Requisite name. ||
|| **CODE**
[`string`](../data-types.md) | Character code of the requisite. ||
|| **XML_ID**
[`string`](../data-types.md) | Foreign key, used for exchange operations.

Identifier of the external information base object.

The purpose of the field may be changed by the end developer. ||
|| **ORIGINATOR_ID**
[`string`](../data-types.md) | External information base identifier.

The purpose of the field may be changed by the end developer. ||
|| **ACTIVE**
[`char`](../data-types.md) | Activity flag.

Uses values `Y` or `N`.

Currently, the field does not actually affect anything. ||
|| **ADDRESS_ONLY**
[`char`](../data-types.md) | Status flag, when the requisite is used only for storing an address.

Uses values `Y` or `N`. When the value is `Y`, Company details are not shown in the entity card, but the address is displayed. ||
|| **SORT**
[`integer`](../data-types.md) | Sorting. The order in the entity's requisite list when there are multiple requisites. ||
|| **RQ_NAME**
[`string`](../data-types.md) | Full Name ||
|| **RQ_FIRST_NAME**
[`string`](../data-types.md) | First name ||
|| **RQ_LAST_NAME**
[`string`](../data-types.md) | Last name ||
|| **RQ_SECOND_NAME**
[`string`](../data-types.md) | Middle name ||
|| **RQ_COMPANY_ID**
[`string`](../data-types.md) | Organization identifier ||
|| **RQ_COMPANY_NAME**
[`string`](../data-types.md) | Abbreviated organization name ||
|| **RQ_COMPANY_FULL_NAME**
[`string`](../data-types.md) | Full organization name ||
|| **RQ_COMPANY_REG_DATE**
[`string`](../data-types.md) | State registration date ||
|| **RQ_DIRECTOR**
[`string`](../data-types.md) | General Director ||
|| **RQ_ACCOUNTANT**
[`string`](../data-types.md) | Chief Accountant ||
|| **RQ_CEO_NAME**
[`string`](../data-types.md) | Full Name of the first manager ||
|| **RQ_CEO_WORK_POS**
[`string`](../data-types.md) | Position of the first manager ||
|| **RQ_CONTACT**
[`string`](../data-types.md) | Contact person ||
|| **RQ_EMAIL**
[`string`](../data-types.md) | E-Mail ||
|| **RQ_PHONE**
[`string`](../data-types.md) | Phone ||
|| **RQ_FAX**
[`string`](../data-types.md) | Fax ||
|| **RQ_IDENT_TYPE**
[`crm_status`](./data-types.md) | Identification method ||
|| **RQ_IDENT_DOC**
[`string`](../data-types.md) | Document type ||
|| **RQ_IDENT_DOC_SER**
[`string`](../data-types.md) | Series ||
|| **RQ_IDENT_DOC_NUM**
[`string`](../data-types.md) | Number ||
|| **RQ_IDENT_DOC_PERS_NUM**
[`string`](../data-types.md) | Personal number ||
|| **RQ_IDENT_DOC_DATE**
[`string`](../data-types.md) | Date of issue ||
|| **RQ_IDENT_DOC_ISSUED_BY**
[`string`](../data-types.md) | Issued by ||
|| **RQ_IDENT_DOC_DEP_CODE**
[`string`](../data-types.md) | Department code ||
|| **RQ_INN**
[`string`](../data-types.md) | TIN ||
|| **RQ_KPP**
[`string`](../data-types.md) | KPP ||
|| **RQ_USRLE**
[`string`](../data-types.md) | Handelsregisternummer (for country DE) ||
|| **RQ_IFNS**
[`string`](../data-types.md) | IFNS ||
|| **RQ_OGRN**
[`string`](../data-types.md) | OGRN ||
|| **RQ_OGRNIP**
[`string`](../data-types.md) | OGRNIP ||
|| **RQ_OKPO**
[`string`](../data-types.md) | OKPO ||
|| **RQ_OKTMO**
[`string`](../data-types.md) | OKTMO ||
|| **RQ_OKVED**
[`string`](../data-types.md) | OKVED ||
|| **RQ_EDRPOU**
[`string`](../data-types.md) | EDRPOU ||
|| **RQ_DRFO**
[`string`](../data-types.md) | DRFO ||
|| **RQ_KBE**
[`string`](../data-types.md) | KBE ||
|| **RQ_IIN**
[`string`](../data-types.md) | IIN ||
|| **RQ_BIN**
[`string`](../data-types.md) | BIN ||
|| **RQ_ST_CERT_SER**
[`string`](../data-types.md) | State registration certificate series ||
|| **RQ_ST_CERT_NUM**
[`string`](../data-types.md) | State registration certificate number ||
|| **RQ_ST_CERT_DATE**
[`string`](../data-types.md) | State registration certificate date ||
|| **RQ_VAT_PAYER**
[`char`](../data-types.md) | VAT Payer (for country UA).

Uses values `Y` or `N` ||
|| **RQ_VAT_ID**
[`string`](../data-types.md) | VAT ID (Value Added Tax identification number) ||
|| **RQ_VAT_CERT_SER**
[`string`](../data-types.md) | VAT certificate series ||
|| **RQ_VAT_CERT_NUM**
[`string`](../data-types.md) | VAT certificate number ||
|| **RQ_VAT_CERT_DATE**
[`string`](../data-types.md) | VAT certificate date ||
|| **RQ_RESIDENCE_COUNTRY**
[`string`](../data-types.md) | Country of residence ||
|| **RQ_BASE_DOC**
[`string`](../data-types.md) | Basis of action ||
|| **RQ_REGON**
[`string`](../data-types.md) | REGON (for country PL) ||
|| **RQ_KRS**
[`string`](../data-types.md) | KRS (for country PL) ||
|| **RQ_PESEL**
[`string`](../data-types.md) | PESEL (for country PL) ||
|| **RQ_LEGAL_FORM**
[`string`](../data-types.md) | Legal form (for country FR) ||
|| **RQ_SIRET**
[`string`](../data-types.md) | Siret Number (for country FR) ||
|| **RQ_SIREN**
[`string`](../data-types.md) | Siren Number (for country FR) ||
|| **RQ_CAPITAL**
[`string`](../data-types.md) | Share capital (for country FR) ||
|| **RQ_RCS**
[`string`](../data-types.md) | RCS (for country FR) ||
|| **RQ_CNPJ**
[`string`](../data-types.md) | CNPJ (for country BR) ||
|| **RQ_STATE_REG**
[`string`](../data-types.md) | State Registration (IE) (for country BR) ||
|| **RQ_MNPL_REG**
[`string`](../data-types.md) | Municipal Registration (IM) (for country BR) ||
|| **RQ_CPF**
[`string`](../data-types.md) | CPF (for country BR) ||
|| **UF_CRM_...** | Custom fields. For example, `UF_CRM_1694526604`.

Requisites can have a set of custom fields with types: `string`, `boolean`, `double`, `datetime`.

You can add a custom field to requisites using the [crm.requisite.userfield.add](./requisites/user-fields/crm-requisite-userfield-add.md) method ||
|#

### Bank Details

The field description is returned by the method [crm.requisite.bankdetail.fields](./requisites/bank-detail/crm-requisite-bank-detail-fields.md)

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Bank requisite identifier. Created automatically and is unique within the portal ||
|| **ENTITY_TYPE_ID**
[`integer`](../data-types.md) | Parent object type identifier. Can only be `Attribute` (value `8`).

The method [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) returns object type identifiers ||
|| **ENTITY_ID**
[`integer`](../data-types.md) | Parent object identifier ||
|| **COUNTRY_ID**
[`integer`](../data-types.md) | Country identifier that corresponds to the set of bank requisite fields (see method [crm.requisite.preset.countries](./requisites/presets/crm-requisite-preset-countries.md) to get available values).

The bank requisite country code matches the country code in the linked requisite template, whose identifier is specified in the `ENTITY_ID` field ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Create date ||
|| **DATE_MODIFY**
[`datetime`](../data-types.md) | Change date ||
|| **CREATED_BY_ID**
[`user`](../data-types.md) | Identifier of the user who created the requisite ||
|| **MODIFY_BY_ID**
[`user`](../data-types.md) | Identifier of the user who changed the requisite ||
|| **NAME^*^**
[`string`](../data-types.md) | Bank requisite name ||
|| **CODE**
[`string`](../data-types.md) | Character code of the requisite. ||
|| **XML_ID**
[`string`](../data-types.md) | External key. Used for exchange operations. Identifier of the external information base object.

The purpose of the field may be changed by the end developer. Each application ensures the uniqueness of values in this field.

It is recommended to use a unique prefix to avoid collisions with other applications ||
|| **ACTIVE**
[`char`](../data-types.md) | Activity flag. Uses values `Y` or `N`.

Currently, the field does not actually affect anything ||
|| **SORT**
[`integer`](../data-types.md) | Sorting ||
|| **RQ_BANK_NAME**
[`string`](../data-types.md) | Bank name ||
|| **RQ_BANK_ADDR**
[`string`](../data-types.md) | Bank address ||
|| **RQ_BANK_CODE**
[`string`](../data-types.md) | Bank Code (for country BR) ||
|| **RQ_BANK_ROUTE_NUM**
[`string`](../data-types.md) | Bank Routing Number ||
|| **RQ_BIK**
[`string`](../data-types.md) | BIC ||
|| **RQ_CODEB**
[`string`](../data-types.md) | Bank Code (for country FR) ||
|| **RQ_CODEG**
[`string`](../data-types.md) | Branch Code (for country FR) ||
|| **RQ_RIB**
[`string`](../data-types.md) | RIB Key (for country FR) ||
|| **RQ_MFO**
[`string`](../data-types.md) | MFO ||
|| **RQ_ACC_NAME**
[`string`](../data-types.md) | Bank Account Holder Name ||
|| **RQ_ACC_NUM**
[`string`](../data-types.md) | Bank Account Number ||
|| **RQ_ACC_TYPE**
[`string`](../data-types.md) | Account Type (for country BR) ||
|| **RQ_AGENCY_NAME**
[`string`](../data-types.md) | Agency (for country BR) ||
|| **RQ_IIK**
[`string`](../data-types.md) | IIK ||
|| **RQ_ACC_CURRENCY**
[`string`](../data-types.md) | Account currency ||
|| **RQ_COR_ACC_NUM**
[`string`](../data-types.md) | Correspondent account ||
|| **RQ_IBAN**
[`string`](../data-types.md) | IBAN ||
|| **RQ_SWIFT**
[`string`](../data-types.md) | SWIFT ||
|| **RQ_BIC**
[`string`](../data-types.md) | BIC ||
|| **COMMENTS**
[`string`](../data-types.md) | Comment ||
|| **ORIGINATOR_ID**
[`string`](../data-types.md) | External information base identifier. The purpose of the field may be changed by the end developer ||
|#

### Templates of Requisites

The field description is returned by the method [crm.requisite.preset.fields](./requisites/presets/crm-requisite-preset-fields.md)

#|
|| **Name** | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Requisite identifier. Created automatically and is unique within the portal ||
|| **ENTITY_TYPE_ID**
[`integer`](../data-types.md) | Parent object type identifier.

The method [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) returns CRM object type identifiers ||
|| **COUNTRY_ID**
[`integer`](../data-types.md) | Country identifier that corresponds to the set of fields in the requisite template (to get available values, see method [crm.requisite.preset.countries](./requisites/presets/crm-requisite-preset-countries.md)) ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Create date ||
|| **DATE_MODIFY**
[`datetime`](../data-types.md) | Modification date. Contains an empty string if the template has not changed since creation. ||
|| **CREATED_BY_ID**
[`user`](../data-types.md) | Identifier of the user who created the requisite ||
|| **MODIFY_BY_ID**
[`user`](../data-types.md) | Identifier of the user who changed the requisite ||
|| **NAME**
[`string`](../data-types.md) | Requisite name. ||
|| **XML_ID**
[`string`](../data-types.md) | Foreign key. Used for exchange operations. Identifier of an object in an external information base.

The purpose of the field may change depending on the end developer.

Each application ensures the uniqueness of values in this field. It is recommended to use a unique prefix to avoid collisions with other applications.

In CRM, values like `#CRM_REQUISITE_PRESET_DEF_...` are reserved for identifying default templates. These identifiers should not be used for your own purposes, as this may lead to logic violations. ||
|| **ACTIVE**
[`char`](../data-types.md) | Activity flag. Uses values `Y` or `N`. Determines the availability of the template in the selection list when adding requisites. ||
|| **SORT**
[`integer`](../data-types.md) | Sorting ||
|#

### Fields of Requisites Templates

The field description is returned by the method [crm.requisite.preset.field.fields](./requisites/presets/fields/crm-requisite-preset-field-fields.md)

#|
||  **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Field identifier. Created automatically and is unique within the template. ||
|| **FIELD_NAME**
[`string`](../data-types.md) | Field name ||
|| **FIELD_TITLE**
[`string`](../data-types.md) | Alternative field name for a requisite.

The alternative name is displayed in various forms for filling out requisites. Depending on the specific form, the alternative name may or may not be used. ||
|| **SORT**
[`integer`](../data-types.md) | Sorting. The order in the template field list. ||
|| **IN_SHORT_LIST**
[`char`](../data-types.md) | Show in short list. Deprecated field, currently not used. Kept for backward compatibility. Can take values `Y` or `N`. ||
|#

### Addresses of Requisites

The field description is returned by the method [crm.address.fields](./requisites/addresses/crm-address-fields.md)

#|
|| **Name** | **Description** ||
|| **TYPE_ID**
[`integer`](../data-types.md) | Address type identifier. "Address type" enumeration item.

"Address type" enumeration items can be obtained using the [crm.enum.addresstype](./auxiliary/enum/crm-enum-address-type.md) method ||
|| **ENTITY_TYPE_ID**
[`integer`](../data-types.md) | Parent object type identifier.

Object type identifiers can be obtained using the [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) method.

{% note tip "" %}

Addresses can only be linked to Requisites (whereas Company details are already linked to companies or contacts) or Leads. For backward compatibility, the ability to link Addresses to Contacts or Companies has been kept. However, this connection is only possible on some old portals where the old address mode was specifically enabled by technical support.

{% endnote %} ||
|| **ENTITY_ID**
[`string`](../data-types.md) | Parent object identifier ||
|| **ADDRESS_1**
[`string`](../data-types.md) | Street, house, building, structure ||
|| **ADDRESS_2**
[`string`](../data-types.md) | Apartment / office ||
|| **CITY**
[`string`](../data-types.md) | City ||
|| **POSTAL_CODE**
[`string`](../data-types.md) | Postal code ||
|| **REGION**
[`string`](../data-types.md) | District ||
|| **PROVINCE**
[`string`](../data-types.md) | Region ||
|| **COUNTRY**
[`string`](../data-types.md) | Country ||
|| **COUNTRY_CODE**
[`string`](../data-types.md) | Country code ||
|| **LOC_ADDR_ID**
[`integer`](../data-types.md) | Location identifier.

This field contains the identifier of the address object in the `Location` module, associated with the CRM address object. Each CRM address corresponds to an address object in the module `location`. This can be used to copy an existing CRM address with location information that is not present in the CRM address fields.

If a `location` module address identifier is specified when creating an address, a copy of the address is created `location` and linked to the created CRM address. If, in this case, no values are specified for the string address fields, they will be filled from the location address.

However, if at least one string field was specified, only the specified fields will be saved in the CRM address, and their values will overwrite the corresponding values in the location address object. The same behavior applies when updating an address. ||
|| **ANCHOR_TYPE_ID**
[`integer`](../data-types.md) | Main parent object type identifier.

This field is for internal use. The value is filled automatically when an address is added.

Object type identifiers can be obtained using the [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) method.

This field contains the identifier of the attribute's parent object type (company or contact) if the address is linked to an attribute. If the address is linked to a lead, this value will be the lead type identifier. ||
|| **ANCHOR_ID**
[`integer`](../data-types.md) | This field is for internal use. The value is filled automatically when an address is added.

This field contains the identifier of the attribute's parent object (company or contact) if the address is linked to an attribute. If the address is linked to a lead, this value will be the lead identifier. ||
|#

## Activities

The field description is returned by the method [crm.activity.fields](./timeline/activities/activity-base/crm-activity-fields.md)

#|
|| **Name** | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Activity identifier ||
|| **OWNER_ID**
[`integer`](../data-types.md) | Owner identifier, immutable ||
|| **OWNER_TYPE_ID**
[`crm_enum_ownertype`](./data-types.md#activity-enums) | Owner type, immutable ||
|| **TYPE_ID**
[`crm_enum_activitytype`](./data-types.md#activity-enums) | Type, immutable ||
|| **PROVIDER_ID**
[`string`](../data-types.md) | Provider identifier ||
|| **PROVIDER_TYPE_ID**
[`string`](../data-types.md) | Provider type identifier ||
|| **PROVIDER_GROUP_ID**
[`string`](../data-types.md) | Connector type ||
|| **ASSOCIATED_ENTITY_ID**
[`integer`](../data-types.md) | Entity identifier related to the activity ||
|| **SUBJECT**
[`string`](../data-types.md) | Subject, activity title ||
|| **START_TIME**
[`datetime`](../data-types.md) | Start time ||
|| **END_TIME**
[`datetime`](../data-types.md) | Completion time ||
|| **DEADLINE**
[`datetime`](../data-types.md) | Due date. This field is not set directly; the value is taken from `START_TIME` for calls and meetings and from `END_TIME` for tasks. ||
|| **COMPLETED**
[`char`](../data-types.md) | Completed ||
|| **STATUS**
[`crm_enum_activitystatus`](./data-types.md#activity-enums) | Status ||
|| **RESPONSIBLE_ID**
[`user`](../data-types.md) | assigned user ||
|| **PRIORITY**
[`crm_enum_activitypriority`](./data-types.md#activity-enums) | Importance ||
|| **NOTIFY_TYPE**
[`crm_enum_activitynotifytype`](./data-types.md#activity-enums) | Notification type ||
|| **NOTIFY_VALUE**
[`integer`](../data-types.md) | Notification parameter ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description ||
|| **DESCRIPTION_TYPE**
[`crm_enum_contenttype`](./data-types.md#activity-enums) | Description type ||
|| **DIRECTION**
[`crm_enum_activitydirection`](./data-types.md#activity-enums) | Activity direction: inbound/outbound. Relevant for calls and emails, not used for meetings. ||
|| **LOCATION**
[`string`](../data-types.md) | Location ||
|| **CREATED**
[`datetime`](../data-types.md) | Create date ||
|| **AUTHOR_ID**
[`user`](../data-types.md) | Activity creator ||
|| **LAST_UPDATED**
[`datetime`](../data-types.md) | Last update date ||
|| **EDITOR_ID**
[`user`](../data-types.md) | Changed by ||
|| **SETTINGS**
[`object`](../data-types.md) | Settings ||
|| **ORIGIN_ID**
[`string`](../data-types.md) | Item identifier in the data source. Used only for linking to an external source. ||
|| **ORIGINATOR_ID**
[`string`](../data-types.md) | Data source identifier. Used only for linking to an external source. ||
|| **RESULT_STATUS**
[`integer`](../data-types.md) | Unused field, remains for compatibility ||
|| **RESULT_STREAM**
[`integer`](../data-types.md) | Report statistics ||
|| **RESULT_SOURCE_ID**
[`string`](../data-types.md) | Unused field, remains for compatibility ||
|| **PROVIDER_PARAMS**
[`object`](../data-types.md) | Provider parameters ||
|| **PROVIDER_DATA**
[`string`](../data-types.md) | Provider data ||
|| **RESULT_MARK**
[`integer`](../data-types.md) | Unused field, remains for compatibility ||
|| **RESULT_VALUE**
[`double`](../data-types.md) | Unused field, remains for compatibility ||
|| **RESULT_SUM**
[`double`](../data-types.md) | Unused field, remains for compatibility ||
|| **RESULT_CURRENCY_ID**
[`string`](../data-types.md) | Unused field, remains for compatibility ||
|| **AUTOCOMPLETE_RULE**
[`integer`](../data-types.md) | Autofill ||
|| **BINDINGS**
[`crm_activity_binding`](./data-types.md#crm_activity_binding) | Links ||
|| **COMMUNICATIONS**
[`crm_activity_communication`](./data-types.md) | Communication channel. Multiple, mandatory ||
|| **FILES**
[`diskfile`](./data-types.md#diskfile) | Added files. Multiple ||
|| **WEBDAV_ELEMENTS**
[`diskfile`](./data-types.md#diskfile) | Added files. Multiple. Deprecated, kept for compatibility ||
|| **IS_INCOMING_CHANNEL**
[`char`](../data-types.md) | Whether the activity is inbound, i.e., created as a result of an incoming client Salutation to a communication channel ||
|#

## Continue Exploring

- [CRM Data Types](./data-types.md) — special field types and links between objects
- [Deal Custom Fields](./deals/user-defined-fields/index.md) — create and configure additional deal fields
- [CRM Statuses and Directories](./status/index.md) — retrieve stage values and other directory fields
