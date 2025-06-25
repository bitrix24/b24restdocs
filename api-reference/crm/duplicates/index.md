# Finding and Processing Duplicates in CRM: Overview of Methods

When working with a large customer database, some CRM elements, such as leads, contacts, or companies, may repeat. The first call from a customer creates a contact with a name and phone number. An email from the same customer creates a new contact with a name and email address — this is a duplicate. Duplicate contacts can be merged.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [finding and processing duplicates in Bitrix24](https://helpdesk.bitrix24.com/open/18346126/)

## Setting Up Duplicate Search in CRM

In the Bitrix24 interface, duplicate search is available for three CRM objects: leads, contacts, and companies.

When initiating a search, the employee can choose which fields will be used to identify duplicates:

* **Leads** — Full Name, company name, phone, email
* **Contacts** — Full Name, phone, email, Tax ID, State Registration Number, and bank account
* **Companies** — company name, phone, email, Tax ID, State Registration Number, and bank account

Using REST methods, you can extend the duplicate search settings in Bitrix24 by adding additional fields, including custom ones. Additional fields in the search settings will appear in the interface for all employees.

To obtain a list of standard and custom fields that can be used to search for duplicates, execute the method [crm.duplicate.volatileType.fields](./volatile-type/crm-duplicate-volatile-type-fields.md).

To get a list of non-standard fields that are already in use for searching, execute the method [crm.duplicate.volatileType.list](./volatile-type/crm-duplicate-volatile-type-list.md).

You can add a field to the duplicate search using the method [crm.duplicate.volatileType.register](./volatile-type/crm-duplicate-volatile-type-register.md), and remove a field from the search using [crm.duplicate.volatileType.unregister](./volatile-type/crm-duplicate-volatile-type-unregister.md).

## Searching for Duplicates via REST

To search for duplicates through an application or webhook, use the method [crm.duplicate.findbycomm](./crm-duplicate-find-by-comm.md). This method allows you to search for duplicates based solely on email address and phone number.

The method crm.duplicate.findbycomm can be used to select a CRM element. For example, you have a [CRM activity of type email](../timeline/activities/index.md) that came from the email test@bitrix.com. To save this email in CRM without creating a duplicate, you need to check if there is a lead, contact, or company in the customer database with that email. The same email may be recorded for both a contact and the associated company. By executing the crm.duplicate.findbycomm request, you will receive the IDs of both elements along with their types, allowing you to decide whether to attach your email to the contact or the company.

{% note tip "Tutorial on using the method" %}

- [How to search in CRM by phone and email](../../../tutorials/crm/how-to-get-lists/search-by-phone-and-email.md)

{% endnote %}

## Merging Duplicates

To merge duplicates, use the method [crm.entity.mergeBatch](./crm-entity-merge-batch.md). This method allows you to merge multiple elements of the same type. You can merge two leads, but you cannot merge a lead with a contact.

Full automatic merging is available in several cases:

* The elements are identical to each other
* The elements are not identical, but the differences in field values do not require manual processing. For example, one element has a filled field, while the other has the same field empty — the value from the filled field will be retained.

If automatic merging of the selected elements is not possible due to data conflicts, there may be several reasons:

* The elements have filled fields with different values. You can check the values of all fields using the get method; for example, to check the fields of leads, use the method [crm.lead.get](../leads/crm-lead-get.md).
* The user making the merge request does not have access to one or more fields.

{% note tip "User documentation" %}

- [How to restrict visibility of custom fields in CRM](https://helpdesk.bitrix24.com/open/23342780/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with permission to modify and delete CRM elements

### Searching and Merging Duplicates

#|
|| **Method** | **Description** ||
|| [crm.duplicate.findbycomm](./crm-duplicate-find-by-comm.md) | Returns a list of duplicates ||
|| [crm.entity.mergeBatch](./crm-entity-merge-batch.md) | Merges duplicates ||
|#

### Setting Up Duplicate Search

#|
|| **Method** | **Description** ||
|| [crm.duplicate.volatileType.fields](./volatile-type/crm-duplicate-volatile-type-fields.md) | Returns a list of fields that can be used to search for duplicates ||
|| [crm.duplicate.volatileType.list](./volatile-type/crm-duplicate-volatile-type-list.md) | Returns a list of non-standard fields involved in duplicate search ||
|| [crm.duplicate.volatileType.register](./volatile-type/crm-duplicate-volatile-type-register.md) | Adds a field to the duplicate search ||
|| [crm.duplicate.volatileType.unregister](./volatile-type/crm-duplicate-volatile-type-unregister.md) | Removes a field from the duplicate search ||
|#