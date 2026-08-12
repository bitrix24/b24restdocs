# CRM Company Details Addresses: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Addresses store postal and legal data associated with company details, leads, and, for backward compatibility, contacts or companies on older portals.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [How to Add Your Company's Company Details](https://helpdesk.bitrix24.com/open/24353440/)

## Getting Started

1. Determine the parent object type `ENTITY_TYPE_ID`
2. Retrieve the parent object identifier `ENTITY_ID`
3. Create an address using the [crm.address.add](./crm-address-add.md) method
4. Update an address using the [crm.address.update](./crm-address-update.md) method
5. Retrieve a list of addresses by filter using the [crm.address.list](./crm-address-list.md) method
6. If an address is no longer needed, delete it using the [crm.address.delete](./crm-address-delete.md) method

## Address Identifiers

- `TYPE_ID` — the address type, such as legal or physical. Values can be retrieved using the [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md) method
- `ENTITY_TYPE_ID` — the parent object type. For company details, pass `8`; for a lead, pass `1`. All values are returned by the [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) method
- `ENTITY_ID` — the parent object identifier. For a company details address, this is the company details ID from [crm.requisite.list](../universal/crm-requisite-list.md)

## Address Fields

Required fields are marked with `*`.

#|
|| **Name** | **Description** ||
|| **TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the address type. Enumeration element "Address Type".

Elements of the enumeration "Address Type" can be obtained using the method [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md) ||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Parent object type identifier.

Object type identifiers can be obtained using the [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) method.

Addresses can only be linked to Requisites (whereas Company details are already linked to companies or contacts) or Leads. For backward compatibility, the ability to link Addresses to Contacts or Companies has been retained. However, this connection is only possible on some older portals where the old address operating mode was specifically enabled by technical support ||
|| **ENTITY_ID***
[`string`](../../../data-types.md) | Identifier of the parent object ||
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
[`integer`](../../../data-types.md) | Location address identifier.

This field contains the identifier of the address object in the `Location` module, associated with the CRM address object. For each CRM address, there is a corresponding address object in the `location`. This can be used to copy an existing CRM address with location information that is not present in the CRM address fields.

If an identifier for an address in the `location` module is specified when creating an address, a copy of the address is created `location` and linked to the created CRM address. If, in this case, no values are specified for the string address fields, they will be filled from the location address.

However, if at least one string field is specified, only the specified fields will be saved in the CRM address, and their values will overwrite the corresponding values in the location address object. The same behavior applies when updating an address ||
|| **ANCHOR_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the main parent object type.

This field is for internal use. The value is automatically filled when adding an address.

Object type identifiers can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

This field contains the identifier of the parent object type of the requisite (company or contact) if the address is linked to a requisite. If the address is linked to a lead, this value will be the lead type identifier ||
|| **ANCHOR_ID**
[`integer`](../../../data-types.md) | This field is for internal use. The value is automatically filled when adding an address.

This field contains the identifier of the parent object of the requisite (company or contact) if the address is linked to a requisite. If the address is linked to a lead, this value will be the lead identifier ||
|#

Use the method [crm.address.fields](./crm-address-fields.md) to obtain a formal description of the address fields.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method — access permissions are checked against the address owner: a contact, company, or lead

#|
|| **Method** | **Description** ||
|| [crm.address.add](./crm-address-add.md) | Adds a new address for a requisite or lead ||
|| [crm.address.update](./crm-address-update.md) | Modifies the address for a requisite or lead ||
|| [crm.address.list](./crm-address-list.md) | Returns a list of addresses based on a filter ||
|| [crm.address.delete](./crm-address-delete.md) | Deletes an address ||
|| [crm.address.fields](./crm-address-fields.md) | Returns a formal description of the address fields ||
|#
