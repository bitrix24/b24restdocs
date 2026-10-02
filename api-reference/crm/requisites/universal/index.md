# Universal Company Details CRM: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Details store identification data for a contact or company: name, legal and registration data, and tax identifiers. Each set of details belongs to one parent CRM object specified by the `ENTITY_TYPE_ID` and `ENTITY_ID` fields.

The set of detail fields is determined by the `PRESET_ID` preset and depends on the country. A contact or company can have multiple sets of details, and the `SORT` field determines their order. The [crm.requisite.fields](./crm-requisite-fields.md#result-fields) method returns the complete set of available fields.
> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [How to Add Your Company Details](https://helpdesk.bitrix24.com/open/24353440/)

## Getting Started

1. Determine the parent object: contact or company
2. Retrieve the requisite preset identifier using the [crm.requisite.preset.list](../presets/crm-requisite-preset-list.md) method
3. Create a requisite using the [crm.requisite.add](./crm-requisite-add.md) method
4. Add an address via [address](../addresses/index.md) methods if the requisite requires a legal or street address
5. Add banking details via [banking details](../bank-detail/index.md) methods if the requisite is used for payment documents
6. Retrieve or update a requisite using the [crm.requisite.get](./crm-requisite-get.md) and [crm.requisite.update](./crm-requisite-update.md) methods

## Linking to Other Objects

The [crm.requisite.add](./crm-requisite-add.md) and [crm.requisite.list](./crm-requisite-list.md) methods return the details identifier in `ID`. It links the details to addresses and bank details.

**Contact or Company.** The `ENTITY_TYPE_ID` and `ENTITY_ID` fields specify the owner of the details. Pass type `3` for a contact and `4` for a company. Retrieve the owner identifier using the [crm.contact.list](../../contacts/crm-contact-list.md) or [crm.company.list](../../companies/crm-company-list.md) method.

**Details Preset.** The `PRESET_ID` field specifies the set of detail fields and the country. The [crm.requisite.preset.list](../presets/crm-requisite-preset-list.md) method returns the preset identifier.

**Address.** For a details address, pass `8` in `ENTITY_TYPE_ID` and the details identifier in `ENTITY_ID`. The `TYPE_ID` field specifies the address type. Use the [details address](../addresses/index.md) methods to add, update, and retrieve addresses.

**Bank Details.** The `ENTITY_ID` field of bank details contains the universal details identifier. Use the [bank details](../bank-detail/index.md) methods to work with accounts.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method and permissions for the contact or company that owns the details

#|
|| **Method** | **Description** ||
|| [crm.requisite.add](./crm-requisite-add.md) | Creates a new requisite ||
|| [crm.requisite.update](./crm-requisite-update.md) | Updates an existing requisite ||
|| [crm.requisite.get](./crm-requisite-get.md) | Returns the requisite by identifier ||
|| [crm.requisite.list](./crm-requisite-list.md) | Returns a list of requisites by filter ||
|| [crm.requisite.delete](./crm-requisite-delete.md) | Deletes the requisite and all related objects ||
|| [crm.requisite.fields](./crm-requisite-fields.md) | Returns the description of requisite fields ||
|#
