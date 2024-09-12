# About Addresses

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Address Fields

#|
|| **Name** | **Description** | **Read** | **Write** | **Required** | **Immutable** ||
|| **TYPE_ID**
[`integer`](../../../data-types.md) | Identifier for the address type. Enumeration element "Address Type".

Enumeration elements for "Address Type" can be obtained using the method [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md) | Yes | Yes | Yes | Yes ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier for the parent object type.

Identifiers for object types can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

Addresses can only be linked to Requisites (which are already linked to companies or contacts) or Leads. For backward compatibility, the ability to link Addresses to Contacts or Companies has been retained. However, this linkage is only possible on some older accounts where the old address handling mode was specifically enabled by technical support.

{% endnote %} | Yes | Yes | Yes | Yes ||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | Identifier for the parent object | Yes | Yes | Yes | Yes ||
|| **ADDRESS_1**
[`string`](../../../data-types.md) | Street, house, building, structure | Yes | Yes | No | No ||
|| **ADDRESS_2**
[`string`](../../../data-types.md) | Apartment / office | Yes | Yes | No | No ||
|| **CITY**
[`string`](../../../data-types.md) | City | Yes | Yes | No | No ||
|| **POSTAL_CODE**
[`string`](../../../data-types.md) | Postal code | Yes | Yes | No | No ||
|| **REGION**
[`string`](../../../data-types.md) | Region | Yes | Yes | No | No ||
|| **PROVINCE**
[`string`](../../../data-types.md) | Province | Yes | Yes | No | No ||
|| **COUNTRY**
[`string`](../../../data-types.md) | Country | Yes | Yes | No | No ||
|| **COUNTRY_CODE**
[`string`](../../../data-types.md) | Country code | Yes | Yes | No | No ||
|| **LOC_ADDR_ID**
[`integer`](../../../data-types.md) | Identifier for the location address.

This field contains the identifier of the address object in the `Location` module, associated with the CRM address object. Each CRM address corresponds to an address object in the `location` module. This can be used to copy an existing address into CRM with location information that is not present in the CRM address fields.

If the identifier of the `location` address is specified when creating an address, a copy of the `location` address is created and linked to the newly created CRM address. If no values are specified for the string address fields in this case, they will be filled from the location address.

However, if at least one string field is specified, only the specified fields will be saved in the CRM address, and their values will overwrite the corresponding values in the location address object. The same behavior will occur when updating the address | Yes | Yes | No | No ||
|| **ANCHOR_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier for the main parent object type.

This field is for internal use. The value is automatically filled when the address is added.

Identifiers for object types can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

This field contains the identifier for the parent object type of the requisite (company or contact) if the address is linked to a requisite. If the address is linked to a lead, this value will be the lead type identifier | Yes | No | No | No ||
|| **ANCHOR_ID**
[`integer`](../../../data-types.md) | This field is for internal use. The value is automatically filled when the address is added.

This field contains the identifier for the parent object of the requisite (company or contact) if the address is linked to a requisite. If the address is linked to a lead, this value will be the lead identifier | Yes | No | No | No ||
|#

Use the method [crm.address.fields](./crm-address-fields.md) to obtain a formal description of the address fields.

## Overview of Methods

#|
|| **Method** | **Description** ||
|| [crm.address.add](./crm-address-add.md) | Adds a new address for a requisite or lead ||
|| [crm.address.update](./crm-address-update.md) | Modifies the address for a requisite or lead ||
|| [crm.address.list](./crm-address-list.md) | Returns a list of addresses based on a filter ||
|| [crm.address.delete](./crm-address-delete.md) | Deletes an address ||
|| [crm.address.fields](./crm-address-fields.md) | Returns a formal description of the address fields ||
|#