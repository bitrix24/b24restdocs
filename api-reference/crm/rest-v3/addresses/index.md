# Addresses in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An address stores the street, city, postal code, region, and country. The methods of this group work with the addresses of contacts, companies, and leads: a contact or a company can have several addresses of different types, and a lead has one address. The `crm.address.list` method returns the addresses of all contacts, companies, and leads in Bitrix24 for export.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Add Company Details](https://helpdesk.bitrix24.com/open/24353440/)

## How to Choose a Method

#|
|| **Task** | **Methods** ||
|| Add, update, delete, or retrieve a contact or company address | [crm.&lt;type&gt;.address.add](./crm-entity-address-add-rest-v3.md), [update](./crm-entity-address-update-rest-v3.md), [delete](./crm-entity-address-delete-rest-v3.md), [get](./crm-entity-address-get-rest-v3.md), [list](./crm-entity-address-list-rest-v3.md) with the `contact` or `company` segment ||
|| Set, retrieve, or delete a lead address | [crm.lead.address.set](./crm-lead-address-set-rest-v3.md), [crm.lead.address.get](./crm-entity-address-get-rest-v3.md), [crm.lead.address.delete](./crm-entity-address-delete-rest-v3.md) ||
|| Export the addresses of all contacts, companies, and leads | [crm.address.list](./crm-address-list-rest-v3.md) ||
|| Work with the address of a specific requisite | The [crm.&lt;type&gt;.requisite.address.*](../requisites/crm-entity-requisite-address-list-rest-v3.md) methods ||
|#

Method names are written in lowercase: `crm.contact.address.add`.

## Relationship with Other Objects

A contact or company address is stored in a requisite, and a lead address is stored in the lead itself.

**Contact and company.** The address owner is set by the `contactId` or `companyId` field, and the owner type by the `contact` or `company` segment of the method name. The address is stored in one of the owner's requisites, and its ID is returned in the `requisiteId` field. When an address is added, CRM chooses the requisite itself: a requisite with an address of the same type, the requisite selected in the form, or the owner's first requisite. If there are no requisites, CRM creates a requisite to store the address. After the address is deleted, the requisite remains. The owner's requisites are returned by the [crm.&lt;type&gt;.requisite.list](../requisites/crm-entity-requisite-list-rest-v3.md) method.

**Lead.** A lead has no requisites and no separate address ID. The `crm.lead.address.*` methods take the lead ID, and [crm.lead.address.set](./crm-lead-address-set-rest-v3.md) both creates and updates the address.

**Address type.** The `typeId` field is the address type from the reference. An owner can have only one address of each type. The type is set when the address is added and does not change afterwards. Address types are returned by the [crm.dictionary.addresstype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method:

#|
|| **typeId** | **Address type** ||
|| `1` | Street address ||
|| `4` | Registered address ||
|| `6` | Legal address ||
|| `9` | Beneficiary address ||
|| `11` | Delivery address ||
|#

## Address Fields {#fields}

Field names are written in camelCase. The `crm.<type>.address.get` and `crm.<type>.address.list` methods return all address fields, and the `select` parameter does not affect the response.

#|
|| **Field**
`type` | **Description** | **Editable** ||
|| **id**
[`integer`](../../../data-types.md) | Address ID. A lead address does not have it | No ||
|| **typeId**
[`integer`](../../../data-types.md) | Address type, see the values in the table above. A lead address does not have it | Only when adding ||
|| **contactId**
[`integer`](../../../data-types.md) | Contact ID. Present only in the `crm.contact.address.*` methods | Only when adding ||
|| **companyId**
[`integer`](../../../data-types.md) | Company ID. Present only in the `crm.company.address.*` methods | Only when adding ||
|| **requisiteId**
[`integer`](../../../data-types.md) | ID of the requisite in which the address is stored. A lead address does not have it | No ||
|| **address1**
[`string`](../../../data-types.md) | Street, building, block, structure | Yes ||
|| **address2**
[`string`](../../../data-types.md) | Apartment or office | Yes ||
|| **city**
[`string`](../../../data-types.md) | City | Yes ||
|| **postalCode**
[`string`](../../../data-types.md) | Postal code | Yes ||
|| **region**
[`string`](../../../data-types.md) | District | Yes ||
|| **province**
[`string`](../../../data-types.md) | Region | Yes ||
|| **country**
[`string`](../../../data-types.md) | Country | Yes ||
|| **countryCode**
[`string`](../../../data-types.md) | Country code from the Bitrix24 locations reference. It is retained only if it matches the country name in `country` in the Bitrix24 language | Yes ||
|#

Empty text fields are returned as an empty string.

The description of fields with the `editable`, `filterable`, and `sortable` flags is returned by the `crm.<type>.address.field.list` and `crm.<type>.address.field.get` methods, and for `crm.address.list` by the `crm.address.field.list` and `crm.address.field.get` methods. They work like the other REST 3.0 field methods: `field.list` takes `select` with a list of flags, and `field.get` also takes `name` with the field name, for example, `{"name":"city"}`.

## How to Get Started

1. Retrieve the contact or company ID with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method
2. Choose an address type with the [crm.dictionary.addresstype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method
3. Add an address with the [crm.&lt;type&gt;.address.add](./crm-entity-address-add-rest-v3.md) method. For a lead, set the address with the [crm.lead.address.set](./crm-lead-address-set-rest-v3.md) method
4. Check the addresses with the [crm.&lt;type&gt;.address.list](./crm-entity-address-list-rest-v3.md) method: the response contains the address ID and the ID of the requisite in which it is stored

## Methods of the Previous Version

At the `/rest/` address, addresses are handled by the [crm.address.*](../../requisites/addresses/index.md) methods. In them, a contact or company address is linked to a requisite rather than to the contact or company itself and is identified by the combination of `TYPE_ID`, `ENTITY_TYPE_ID`, and `ENTITY_ID`. In REST 3.0, an address has its own `id`, and the owner is set by the `contactId` or `companyId` field.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Contact and Company Addresses

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.address.add](./crm-entity-address-add-rest-v3.md) | Adds an address to a contact or company ||
|| [crm.&lt;type&gt;.address.update](./crm-entity-address-update-rest-v3.md) | Updates a contact or company address ||
|| [crm.&lt;type&gt;.address.get](./crm-entity-address-get-rest-v3.md) | Returns a contact, company, or lead address ||
|| [crm.&lt;type&gt;.address.list](./crm-entity-address-list-rest-v3.md) | Returns the addresses of a contact or company ||
|| [crm.&lt;type&gt;.address.delete](./crm-entity-address-delete-rest-v3.md) | Deletes a contact, company, or lead address ||
|| `crm.<type>.address.field.list` | Returns the description of address fields ||
|| `crm.<type>.address.field.get` | Returns the description of an address field by name ||
|#

### Lead Address

#|
|| **Method** | **Description** ||
|| [crm.lead.address.set](./crm-lead-address-set-rest-v3.md) | Sets a lead address ||
|| [crm.lead.address.get](./crm-entity-address-get-rest-v3.md) | Returns a lead address ||
|| [crm.lead.address.delete](./crm-entity-address-delete-rest-v3.md) | Deletes a lead address ||
|#

### Bitrix24 Addresses

#|
|| **Method** | **Description** ||
|| [crm.address.list](./crm-address-list-rest-v3.md) | Returns the addresses of all contacts, companies, and leads ||
|| `crm.address.field.list` | Returns the description of address fields ||
|| `crm.address.field.get` | Returns the description of an address field by name ||
|#
