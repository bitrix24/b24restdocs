# CRM Company Details Addresses: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Addresses store the street, city, postal code, and other location data. For example, an application can store the physical and legal addresses of a company in its requisite. A lead has no requisites, so its address is linked to the lead itself.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [How to Add Your Company's Company Details](https://helpdesk.bitrix24.com/open/24353440/)

## Getting Started

1. Choose the address type `TYPE_ID` with the [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md) method
2. Determine the owner type `ENTITY_TYPE_ID` and retrieve the owner identifier `ENTITY_ID` — the sources of the values are listed in the [Fields Table](#fields)
3. Pass these three fields and the address text to the [crm.address.add](./crm-address-add.md) method
4. Check the saved address with the [crm.address.list](./crm-address-list.md) method by specifying its `TYPE_ID`, `ENTITY_TYPE_ID`, and `ENTITY_ID` in `filter`
5. To update or delete the address, pass the same set of identifiers in `fields` of the [crm.address.update](./crm-address-update.md) or [crm.address.delete](./crm-address-delete.md) method

## Address Identifiers

A CRM address has no separate `ID` field. It is identified by the combination of `TYPE_ID`, `ENTITY_TYPE_ID`, and `ENTITY_ID`: the address type, the owner type, and the owner identifier. A single requisite can have multiple addresses of different types.

For the address of a company or contact, work with the requisite: pass the requisite ID in `ENTITY_ID`, not the ID of the company or contact.

## Address Fields {#fields}

Required fields are marked with `*`.

#|
|| **Name**
`type` | **Description** ||
|| **TYPE_ID***
[`integer`](../../../data-types.md) | Address type: for example, `1` — physical, `6` — legal. The available values are returned by [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md) ||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Address owner type: `8` — requisite, `1` — lead. Object type identifiers are returned by [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) ||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Address owner identifier. For a requisite, it is returned by [crm.requisite.list](../universal/crm-requisite-list.md); for a lead, by [crm.lead.list](../../leads/crm-lead-list.md) ||
|| **ADDRESS_1**
[`string`](../../../data-types.md) | Street, house, building, structure ||
|| **ADDRESS_2**
[`string`](../../../data-types.md) | Apartment / office ||
|| **CITY**
[`string`](../../../data-types.md) | City ||
|| **POSTAL_CODE**
[`string`](../../../data-types.md) | Postal code ||
|| **REGION**
[`string`](../../../data-types.md) | District ||
|| **PROVINCE**
[`string`](../../../data-types.md) | Region ||
|| **COUNTRY**
[`string`](../../../data-types.md) | Country ||
|| **COUNTRY_CODE**
[`string`](../../../data-types.md) | Country code ||
|| **LOC_ADDR_ID**
[`integer`](../../../data-types.md) | Identifier of the related address in the Location module, which stores location data. It is returned by [crm.address.list](./crm-address-list.md); it does not replace the three fields that identify a CRM address ||
|| **ANCHOR_TYPE_ID**
[`integer`](../../../data-types.md) | CRM object type: the company or contact that owns the requisite, or the lead itself. Bitrix24 fills in the field automatically with a value from [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) ||
|| **ANCHOR_ID**
[`integer`](../../../data-types.md) | Identifier of the object whose type is specified in `ANCHOR_TYPE_ID`. Bitrix24 fills in the field automatically; both `ANCHOR_*` fields are read-only ||
|#

## Method Results

The [crm.address.list](./crm-address-list.md) method returns an array of addresses in `result`. Each element contains the fields from the table above. The `select` parameter sets the field set; if it is not passed, the method returns all available fields. If no addresses match the filter, `result` is an empty array `[]`.

The [crm.address.add](./crm-address-add.md), [crm.address.update](./crm-address-update.md), and [crm.address.delete](./crm-address-delete.md) methods return `true` in `result` if the request succeeds.

The [crm.address.fields](./crm-address-fields.md) method returns an object with field descriptions in `result`: their types, titles, flags indicating whether they are required or editable, and other attributes.

{% note warning "Check the Saved Address" %}

If you pass only `TYPE_ID`, `ENTITY_TYPE_ID`, and `ENTITY_ID` to [crm.address.add](./crm-address-add.md), the method returns `true` but does not create an address. Pass the text fields of the address or the `LOC_ADDR_ID` of an existing location address, and then check the result with the [crm.address.list](./crm-address-list.md) method.

When updating with [crm.address.update](./crm-address-update.md), pass all text fields that you need to retain. If you pass only some of them, Bitrix24 clears the remaining text fields of the CRM address.

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method — a user with permission to add, edit, or delete the company, contact, or lead to which the address belongs. Retrieving the list requires permission to read contacts, companies, and leads; any user can retrieve the field descriptions

#|
|| **Method** | **Description** ||
|| [crm.address.add](./crm-address-add.md) | Adds a new address for a requisite or lead ||
|| [crm.address.update](./crm-address-update.md) | Modifies the address for a requisite or lead ||
|| [crm.address.list](./crm-address-list.md) | Returns a list of addresses based on a filter ||
|| [crm.address.delete](./crm-address-delete.md) | Deletes an address ||
|| [crm.address.fields](./crm-address-fields.md) | Returns a formal description of the address fields ||
|#
