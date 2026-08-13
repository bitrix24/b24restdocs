# Company Contacts: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Using the group of methods `crm.company.contact.*`, you can manage the relationships between contacts and companies: establishing or removing connections for a contact or a group of contacts.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Link deals, contacts and companies](https://helpdesk.bitrix24.com/open/2519229)

## Benefits of Connecting Companies and Contacts

- The contacts of a company are populated automatically when you select it in the `Client` field of a deal or an SPA.
- The company detail form displays the data of the related contacts: name, phone number, e-mail, position.
- From the company detail form, you can call or send an e-mail without navigating to the contact detail form.
- When [generating documents from a template](../../document-generator/index.md), symbolic codes insert the data of the related contacts into the document.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.company.contact.add](./crm-company-contact-add.md) | Adds a contact to the specified company ||
|| [crm.company.contact.items.get](./crm-company-contact-items-get.md) | Returns the set of contacts associated with the specified company ||
|| [crm.company.contact.items.set](./crm-company-contact-items-set.md) | Establishes the set of contacts associated with the specified company ||
|| [crm.company.contact.delete](./crm-company-contact-delete.md) | Removes a contact from the specified company ||
|| [crm.company.contact.items.delete](./crm-company-contact-items-delete.md) | Clears the set of contacts associated with the specified company ||
|| [crm.company.contact.fields](./crm-company-contact-fields.md) | Returns the description of fields for the company-contact relationship ||
|#
