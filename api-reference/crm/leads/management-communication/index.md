# Linking Leads to Contacts: Overview of Methods

Using the group of methods `crm.lead.contact.*`, you can establish or remove the connection between contacts and leads. A lead linked to a contact becomes a duplicate.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [The "Client" field in the CRM detail form](https://helpdesk.bitrix24.com/open/17748916/)

## Features of Duplicate Leads

A duplicate lead is a request from a client who is already in the company's customer database. Duplicate leads have hidden contact information fields: "Phone", "Email", "Address", "Details". A duplicate lead can only be converted into a deal. When a known client makes a new request, a duplicate lead will automatically be created, linked to the client’s detail form, if the CRM is set to handle duplicate leads.

The methods [crm.lead.contact.add](./crm-lead-contact-add.md) and [crm.lead.contact.items.set](./crm-lead-contact-items-set.md) add a connection between a lead and one or more contacts. When this connection is established, the lead automatically becomes a duplicate.

To restore the ability to convert a lead into a contact, remove the connection with contacts using the methods [crm.lead.contact.delete](./crm-lead-contact-delete.md) if there is one contact, and [crm.lead.contact.items.delete](./crm-lead-contact-items-delete.md) if there are multiple contacts. Once the connection is removed, the lead will no longer be a duplicate.

{% note tip "User Documentation" %}

[Duplicate Leads and Deals](https://helpdesk.bitrix24.com/open/24147842/)

{% endnote %}

## Benefits of Linking Leads and Contacts

1. The lead detail form displays information about linked contacts: name, phone number, email, position.

2. You can call or send an email directly from the lead detail form without navigating to the contact detail form.

3. Communication with the client: emails, calls, and chats from open channels will be stored in both the contact detail form and the lead detail form. Communications are not attached to closed leads.

4. CoPilot in CRM processes client calls from the lead detail form: it transcribes recordings, summarizes conversations, and fills in fields in the CRM detail form.

5. When [generating documents from a template](../../document-generator/index), you can use symbolic codes that automatically insert data from linked contacts into the document.

{% note tip "User Documentation" %}

[CoPilot in CRM](https://helpdesk.bitrix24.com/open/19268296/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform the method: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.lead.contact.add](./crm-lead-contact-add.md) | Adds a contact link to the specified lead ||
|| [crm.lead.contact.delete](./crm-lead-contact-delete.md) | Removes a contact link from the specified lead ||
|| [crm.lead.contact.items.get](./crm-lead-contact-items-get.md) | Retrieves a list of contacts linked to the lead ||
|| [crm.lead.contact.items.set](./crm-lead-contact-items-set.md) | Attaches a list of contacts to the specified lead ||
|| [crm.lead.contact.items.delete](./crm-lead-contact-items-delete.md) | Removes a list of contacts from the lead ||
|| [crm.lead.contact.fields](./crm-lead-contact-fields.md) | Retrieves the description of fields for the lead-contact link used by the methods in the `crm.lead.contact.*` family ||
|#