# Company Contacts: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The group of methods `crm.company.contact.*` manages the link between a [company](../index.md) and its contacts: it adds and removes an individual contact, reads and replaces the entire set of company contacts. A single company can have several contacts linked to it.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Link deals, contacts and companies](https://helpdesk.bitrix24.com/open/2519229)

## Benefits of Connecting Companies and Contacts

The linked contacts are visible in the interface and are involved in the work of other tools:

- The contacts of a company are populated automatically when you select it in the `Client` field of a deal or an SPA.
- The company detail form displays the data of the related contacts: name, phone number, e-mail, position.
- The company detail form lets you call or send an e-mail without navigating to the contact detail form.
- When [generating documents from a template](../../document-generator/index.md), symbolic codes insert the data of the related contacts into the document.

## How the Link Works in the API

The link is a separate record, not a field of the company. The methods of the group work with the binding object. It has four fields.

#|
|| **Field** | **What It Means** | **Example Value** ||
|| `CONTACT_ID` | Identifier of the linked contact. The only required field of the binding. The identifiers can be obtained using the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 3` | `7` ||
|| `SORT` | Sort index. Sets the order of contacts in the company detail form. If you do not pass it, Bitrix24 substitutes the value itself — the rule differs between [crm.company.contact.add](./crm-company-contact-add.md) and [crm.company.contact.items.set](./crm-company-contact-items-set.md) | `100` ||
|| `IS_PRIMARY` | Indicates whether the binding is primary. The flag belongs to the contact: it means that this company is the primary one for it. When the binding is written, the company is placed into the contact's `COMPANY_ID` field. Possible values: `Y` — yes, `N` — no | `Y` ||
|| `ROLE_ID` | Role identifier. The field is reserved and read-only. It is returned by [crm.company.contact.items.get](./crm-company-contact-items-get.md), but is missing from the output of [crm.company.contact.fields](./crm-company-contact-fields.md) | `0` ||
|#

Bitrix24 sets the primary flag itself — that is why `IS_PRIMARY = Y` can appear on several bindings at once within the set of a company's contacts. The exact rules are described on the pages of [crm.company.contact.add](./crm-company-contact-add.md) and [crm.company.contact.items.set](./crm-company-contact-items-set.md).

When a binding is removed by [crm.company.contact.delete](./crm-company-contact-delete.md), [crm.company.contact.items.delete](./crm-company-contact-items-delete.md), or [crm.company.contact.items.set](./crm-company-contact-items-set.md), the contact's `COMPANY_ID` field switches to another of its companies, while the `IS_PRIMARY` flag of the remaining bindings does not change — after that, the values of the field and the flag diverge. The details are described on the method pages.

## How to Choose a Method

#|
|| **If You Need To** | **Open the Method** ||
|| Add a single contact without affecting the others | [crm.company.contact.add](./crm-company-contact-add.md) ||
|| Remove a single contact without affecting the others | [crm.company.contact.delete](./crm-company-contact-delete.md) ||
|| Retrieve the list of a company's contacts | [crm.company.contact.items.get](./crm-company-contact-items-get.md) ||
|| Replace the entire set of contacts with the one provided | [crm.company.contact.items.set](./crm-company-contact-items-set.md) ||
|| Change `SORT` or the primary flag of an already linked contact | [crm.company.contact.items.set](./crm-company-contact-items-set.md) — read the set using `items.get` and pass it in full with the new values. Calling `add` again for an already linked contact returns `false` and changes nothing ||
|| Unlink all contacts from a company | [crm.company.contact.items.delete](./crm-company-contact-items-delete.md) ||
|| Retrieve the description of the binding fields | [crm.company.contact.fields](./crm-company-contact-fields.md) ||
|#

The mirror task — managing the companies of a contact — is handled by the group of methods [crm.contact.company.*](../../contacts/company/index.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method — reading the bindings requires the "Read" access permission for companies, changing the bindings requires the "Edit" access permission for companies, and the field description is available to any user. The methods `crm.company.contact.add` and `crm.company.contact.delete` additionally check the "Read" access permission for the contact

#|
|| **Method** | **Description** ||
|| [crm.company.contact.add](./crm-company-contact-add.md) | Adds a contact to the specified company ||
|| [crm.company.contact.delete](./crm-company-contact-delete.md) | Removes a contact from the specified company ||
|| [crm.company.contact.items.get](./crm-company-contact-items-get.md) | Returns the set of contacts associated with the specified company ||
|| [crm.company.contact.items.set](./crm-company-contact-items-set.md) | Establishes the set of contacts associated with the specified company ||
|| [crm.company.contact.items.delete](./crm-company-contact-items-delete.md) | Clears the set of contacts associated with the specified company ||
|| [crm.company.contact.fields](./crm-company-contact-fields.md) | Returns the description of fields for the company-contact relationship ||
|#
