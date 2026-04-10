# Duplicate Search Settings for Any Fields: Overview of Methods

When there are many leads, contacts, and companies in the CRM, duplicates arise due to incomplete or inconsistently filled data.

The methods `crm.duplicate.volatileType.*` extend the standard duplicate check and allow for the configuration of additional fields.

To search for duplicates based solely on phone numbers and emails, use [crm.duplicate.findbycomm](../crm-duplicate-find-by-comm.md).

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Searching and Handling Duplicates in Bitrix24](https://helpdesk.bitrix24.com/open/25404806/) 

## Connection of Duplicate Search Settings with CRM Entities

The `entityTypeId` parameter specifies the type of CRM entity and determines which objects the methods work with regarding additional search fields.

#| 
|| **CRM Entity Type** | **entityTypeId** ||
|| Lead | `1` ||
|| Contact | `3` ||
|| Company | `4` ||
|#

## Important Considerations

- You can register no more than 7 custom fields in total for all entity types.
- To remove a field, you need the `id` of the record, which is returned by [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md).

## How to Configure Search by Additional Fields

1. Retrieve the available fields using the method [crm.duplicate.volatileType.fields](./crm-duplicate-volatile-type-fields.md).
2. Check the current connections via [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md).
3. Add the required field using the method [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md).
4. Re-invoke [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md) and check the result.
5. Remove the field using the method [crm.duplicate.volatileType.unregister](./crm-duplicate-volatile-type-unregister.md) if it is no longer needed.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#| 
|| **Method** | **Description** ||
|| [crm.duplicate.volatileType.fields](./crm-duplicate-volatile-type-fields.md) | Returns a list of standard and custom fields for duplicate searches ||
|| [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md) | Returns a list of additional fields already connected to duplicate searches ||
|| [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md) | Adds a field to the duplicate search settings ||
|| [crm.duplicate.volatileType.unregister](./crm-duplicate-volatile-type-unregister.md) | Removes a field from the duplicate search settings ||
|#