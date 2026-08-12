# Finding and Processing Duplicates in CRM: Overview of Methods

A duplicate is a set of CRM elements that stand for one and the same person or organization. The first call from a customer creates a contact with a name and phone number, and an email from that same customer creates a second contact with a name and email address. Bitrix24 can find such pairs and merge them into a single element.

Duplicate search works only with leads, contacts, and companies — both in the interface and through the method [crm.duplicate.findbycomm](./crm-duplicate-find-by-comm.md). Merging works more broadly: the method [crm.entity.mergeBatch](./crm-entity-merge-batch.md) also accepts deals, invoices, estimates, and SPA elements, provided that you have determined the identifiers of the elements to merge yourself.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Finding and processing duplicates in Bitrix24](https://helpdesk.bitrix24.com/open/18346126/)

## How to Choose a Method

#|
|| **If You Need To** | **Use** ||
|| Check, before creating a lead, contact, or company, whether such a phone number or email address is already in the database | [crm.duplicate.findbycomm](./crm-duplicate-find-by-comm.md) ||
|| Merge several elements of the same type into one | [crm.entity.mergeBatch](./crm-entity-merge-batch.md) ||
|| Add a new field to the duplicate search in the interface, for example a Tax ID or a custom field | The methods of the [Duplicate Search Settings for Any Fields](./volatile-type/index.md) section ||
|#

## Searching for Duplicates via REST

The method [crm.duplicate.findbycomm](./crm-duplicate-find-by-comm.md) searches only by phone number and email address. Other fields are not available to this method, even if they have been added to the duplicate search settings.

The method takes the communication type `type` with the value `PHONE` or `EMAIL` and a list of values `values`. The optional `entity_type` narrows the search down to a single object — `LEAD`, `CONTACT`, or `COMPANY`. The response returns the identifiers of the elements found, grouped by these same types.

The method is convenient for selecting a CRM element. For example, you have a [CRM activity of type email](../timeline/activities/index.md) that came from the address `test@bitrix.com`. To retain the email in the CRM without creating a duplicate, check whether there is a lead, contact, or company in the database with that address. The same address may be recorded for both a contact and the company associated with it — the request returns the identifiers of both elements along with their types, so you can choose where to attach the email.

{% note tip "Tutorial on using the method" %}

- [How to Find Duplicates in CRM by Phone and Email](../../../tutorials/crm/how-to-get-lists/search-by-phone-and-email.md)

{% endnote %}

## Merging Duplicates

The method [crm.entity.mergeBatch](./crm-entity-merge-batch.md) merges several elements of the same type. You can merge two leads, but you cannot merge a lead with a contact. The element whose `id` comes first in the `entityIds` array becomes the main one: the data of the others is transferred into it, and the others are deleted.

Full automatic merging is available in two cases:

- the elements are identical to each other
- the elements are not identical, but the difference in field values does not require manual processing. For example, one element has a filled field, while the other has the same field empty — the value from the filled field is retained

If the elements cannot be merged automatically, the method returns the status `CONFLICT`. There are two possible reasons:

- the elements have filled fields with different values. You can check the values of all fields using the `get` method, for example [crm.lead.get](../leads/crm-lead-get.md) for leads
- the user on whose behalf the request is executed does not have access to one or more fields

A conflict is resolved manually in the Bitrix24 interface — the links for each object type are provided on the page of the method [crm.entity.mergeBatch](./crm-entity-merge-batch.md).

{% note tip "User documentation" %}

- [How to restrict visibility of custom fields in CRM](https://helpdesk.bitrix24.com/open/23342780/)

{% endnote %}

## Setting Up Duplicate Search in the Interface

In the Bitrix24 interface, the employee chooses which fields to use to identify duplicates. The following are available by default:

- **Leads** — Full Name, company name, phone, email address
- **Contacts** — Full Name, phone, email address, requisites, and bank details
- **Companies** — company name, phone, email address, requisites, and bank details

The composition of the requisites depends on the country.

This list can be extended with the `crm.duplicate.volatileType.*` methods — you can add standard and custom fields. The added fields appear in the search settings for all employees. In total, no more than seven additional fields can be connected for leads, contacts, and companies combined. For details, see the section [Duplicate Search Settings for Any Fields](./volatile-type/index.md).

These settings affect only the duplicate search in the interface. The method `crm.duplicate.findbycomm` does not take them into account.

## Overview of Methods {#all-methods}

### Searching and Merging Duplicates

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method — searching requires permission to read CRM elements, merging requires permission to modify and delete them

#|
|| **Method** | **Description** ||
|| [crm.duplicate.findbycomm](./crm-duplicate-find-by-comm.md) | Returns a list of duplicates ||
|| [crm.entity.mergeBatch](./crm-entity-merge-batch.md) | Merges duplicates ||
|#

### Setting Up Duplicate Search

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the methods: Bitrix24 administrator or CRM administrator

#|
|| **Method** | **Description** ||
|| [crm.duplicate.volatileType.fields](./volatile-type/crm-duplicate-volatile-type-fields.md) | Returns a list of fields that can be used to search for duplicates ||
|| [crm.duplicate.volatileType.list](./volatile-type/crm-duplicate-volatile-type-list.md) | Returns a list of non-standard fields involved in duplicate search ||
|| [crm.duplicate.volatileType.register](./volatile-type/crm-duplicate-volatile-type-register.md) | Adds a field to the duplicate search ||
|| [crm.duplicate.volatileType.unregister](./volatile-type/crm-duplicate-volatile-type-unregister.md) | Removes a field from the duplicate search ||
|#
