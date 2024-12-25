# Contact-Company Relationship: Overview of Methods

Using the group of methods `crm.contact.company.*`, you can establish or remove the relationship between a contact and a company or a group of companies.

> Quick navigation: [all methods](#all-methods) 
> 
> User documentation: [Relationship between deals, contacts, and companies](https://helpdesk.bitrix24.com/open/2519229/) 

## Benefits of the Relationship Between Contacts and Companies

1. The contact card displays information about the companies: name, phone number, e-mail, address, company type, and industry. 
2. You can call or send an e-mail directly from the contact card without navigating to the company card.
3. When [generating documents from a template](../../document-generator/index.md), you can use symbolic codes that will automatically insert data from related companies into the document.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#| 
|| **Method** | **Description** ||
|| [crm.contact.company.add](./crm-contact-company-add.md) | Adds a company to the specified contact ||
|| [crm.contact.company.delete](./crm-contact-company-delete.md) | Removes a company from the specified contact ||
|| [crm.contact.company.fields](./crm-contact-company-fields.md) | Returns the description of fields for the contact-company relationship ||
|| [crm.contact.company.items.get](./crm-contact-company-items-get.md) | Returns a set of companies associated with the specified contact ||
|| [crm.contact.company.items.set](./crm-contact-company-items-set.md) | Establishes a set of companies associated with the specified contact ||
|| [crm.contact.company.items.delete](./crm-contact-company-items-delete.md) | Clears the set of companies associated with the specified contact ||
|#