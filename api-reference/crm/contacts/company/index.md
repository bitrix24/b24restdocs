# Contact-Company Relationship: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The group of methods `crm.contact.company.*` manages the relationship between a [contact](../index.md) and companies: it adds and removes an individual company, reads and replaces the entire set of the contact's companies. A single contact can be linked to several companies.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Relationship between deals, contacts, and companies](https://helpdesk.bitrix24.com/open/2519229/)

## Benefits of the Relationship Between Contacts and Companies

1. The contact card displays information about the companies: name, phone number, e-mail, address, company type, and industry.
2. You can call or send an e-mail directly from the contact card without navigating to the company card.
3. When [generating documents from a template](../../document-generator/index.md), you can use symbolic codes that will automatically insert data from related companies into the document.

## How the Relationship Is Structured in the API

The relationship is a separate record, not a contact field. The methods of the group work with a link object that has four fields.

#|
|| **Field** | **What It Means** | **Example Value** ||
|| `COMPANY_ID` | Identifier of the linked company. The only required field of the link. You can retrieve the identifiers with the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 4` | `7` ||
|| `SORT` | Sorting index. Defines the order of the companies in the contact card | `100` ||
|| `IS_PRIMARY` | Whether the link is the primary one. The company from the primary link is written to the contact field `COMPANY_ID` and is considered its main company. Possible values: `Y` — yes, `N` — no | `Y` ||
|| `ROLE_ID` | Identifier of the role. The field is reserved, you do not need to set it | `0` ||
|#

Bitrix24 maintains the primary link itself. The method [crm.contact.company.add](./crm-contact-company-add.md) marks the added company as the primary one if `IS_PRIMARY = Y` is passed or if the contact does not have a primary company yet. If the primary link is deleted with the method [crm.contact.company.delete](./crm-contact-company-delete.md), the first of the remaining links becomes the primary one. That is why there is no need to update the contact field `COMPANY_ID` separately.

## How to Choose a Method

Some methods of the group work with an individual link, others with the entire set of companies at once.

#|
|| **If You Need To** | **Open the Method** ||
|| Add a single company without affecting the others | [crm.contact.company.add](./crm-contact-company-add.md) ||
|| Remove a single company without affecting the others | [crm.contact.company.delete](./crm-contact-company-delete.md) ||
|| Retrieve the list of the contact's companies | [crm.contact.company.items.get](./crm-contact-company-items-get.md) ||
|| Replace the entire set of companies with the one you pass | [crm.contact.company.items.set](./crm-contact-company-items-set.md) ||
|| Unlink all companies from the contact | [crm.contact.company.items.delete](./crm-contact-company-items-delete.md) ||
|| Retrieve the description of the link fields | [crm.contact.company.fields](./crm-contact-company-fields.md) ||
|#

The mirror task — managing the contacts of a company — is solved by the group of methods [crm.company.contact.*](../../companies/contacts/index.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method — reading the links requires the "read" access permission for the contact, changing the links requires the "modify" permission for the contact and the "read" permission for the company being added or removed, and the description of the fields is available to any user

#| 
|| **Method** | **Description** ||
|| [crm.contact.company.add](./crm-contact-company-add.md) | Adds a company to the specified contact ||
|| [crm.contact.company.delete](./crm-contact-company-delete.md) | Removes a company from the specified contact ||
|| [crm.contact.company.items.get](./crm-contact-company-items-get.md) | Returns a set of companies associated with the specified contact ||
|| [crm.contact.company.items.set](./crm-contact-company-items-set.md) | Establishes a set of companies associated with the specified contact ||
|| [crm.contact.company.items.delete](./crm-contact-company-items-delete.md) | Clears the set of companies associated with the specified contact ||
|| [crm.contact.company.fields](./crm-contact-company-fields.md) | Returns the description of fields for the contact-company relationship ||
|#