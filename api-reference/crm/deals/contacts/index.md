# Linking Deals to Contacts: Overview of Methods

Using the group of methods `crm.deal.contact.*`, you can establish or remove the connection between contacts and a deal. Use the methods `crm.deal.contact.*` for working with a single contact, and the methods `crm.deal.contact.items.*` for managing a group of contacts.

> Quick navigation: [all methods](#all-methods) 

## Benefits of Linking Deals to Contacts

1. The deal detail form displays information about related contacts: name, phone number, e-mail, and position.
2. You can call or send an e-mail directly from the deal detail form without navigating to the contact detail form.
3. Communication with the client: e-mails, calls, and chats from open lines will be stored in both the contact detail form and the deal detail form. Communications are not attached to closed deals.
4. CoPilot in CRM processes client calls from the deal detail form: it transcribes recordings, summarizes conversations, and fills in fields in the CRM activity.
5. When [generating documents from a template](../../document-generator/index.md), you can use symbolic codes that automatically insert data from related contacts into the document.

{% note tip "User Documentation" %}

- [CoPilot in CRM](https://helpdesk.bitrix24.com/open/19268296/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.deal.contact.add](./crm-deal-contact-add.md) | Adds a contact to a deal ||
|| [crm.deal.contact.items.set](./crm-deal-contact-items-set.md) | Adds multiple contacts to a deal ||
|| [crm.deal.contact.fields](./crm-deal-contact-fields.md) | Returns the fields for the deal-contact link ||
|| [crm.deal.contact.items.get](./crm-deal-contact-items-get.md) | Retrieves a set of contacts linked to a deal ||
|| [crm.deal.contact.delete](./crm-deal-contact-delete.md) | Removes a contact from the specified deal ||
|| [crm.deal.contact.items.delete](./crm-deal-contact-items-delete.md) | Deletes a set of contacts linked to the specified deal ||
|#