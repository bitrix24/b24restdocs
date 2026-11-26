# Duplicate Search Settings for Additional Fields: Overview of Methods

The methods `crm.duplicate.volatileType.*` extend the standard set of fields used to search for duplicates — available standard and custom fields of a lead, contact, or company can be added to the search.

The added field appears in the duplicate search settings in the Bitrix24 interface for all employees. These settings do not affect the search performed by [crm.duplicate.findbycomm](../crm-duplicate-find-by-comm.md): that method works only with phone numbers and email addresses. Duplicate handling as a whole is described in the section [Finding and Processing Duplicates in CRM](../index.md).

For example, you can add the lead name to the search — then Bitrix24 shows leads with the same name as duplicates.

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Searching and Handling Duplicates in Bitrix24](https://helpdesk.bitrix24.com/open/25404806/)

## Fields Available by Default

In the Bitrix24 interface, the employee chooses the fields used to search for duplicates:

- **Leads** — Full Name, company name, phone, email address
- **Contacts** — Full Name, phone, email address, requisites, and bank details
- **Companies** — company name, phone, email address, requisites, and bank details

The composition of the requisites depends on the country.

## How to Configure Search by Additional Fields

1. Retrieve the list of available fields using the method [crm.duplicate.volatileType.fields](./crm-duplicate-volatile-type-fields.md) — the response contains `entityTypeId` and `fieldCode` pairs.
2. Check which fields are already connected using the method [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md).
3. Connect the required field using the method [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md), passing the `entityTypeId` and `fieldCode` from the first step.
4. If a field is no longer needed for the search, disconnect it using the method [crm.duplicate.volatileType.unregister](./crm-duplicate-volatile-type-unregister.md), passing the record `id` from [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md).

### Example of Choosing a Field

A fragment of the `crm.duplicate.volatileType.fields` response for a lead:

```json
{
    "result": [
        {
            "entityTypeId": 1,
            "fieldCode": "TITLE",
            "fieldTitle": "Lead Name"
        }
    ]
}
```

To connect the lead name to duplicate search, pass the values from this element to `crm.duplicate.volatileType.register`: `entityTypeId: 1` and `fieldCode: "TITLE"`.

## Important Considerations

- No more than seven fields in total can be connected for leads, contacts, and companies. If all seven slots are taken, an attempt to connect a new field using [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md) returns the error `MAX_TYPES_COUNT_EXCEEDED`. Registering an already connected field again returns its `id`.
- After a field is connected, the duplicate index is recalculated by a background agent, so new matches do not appear in the interface right away.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: Bitrix24 administrator or CRM administrator

#|
|| **Method** | **Description** ||
|| [crm.duplicate.volatileType.fields](./crm-duplicate-volatile-type-fields.md) | Returns a list of standard and custom fields for duplicate searches ||
|| [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md) | Returns a list of additional fields already connected to duplicate searches ||
|| [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md) | Adds a field to the duplicate search settings ||
|| [crm.duplicate.volatileType.unregister](./crm-duplicate-volatile-type-unregister.md) | Removes a field from the duplicate search settings ||
|#
