# Statuses of Payer Types in the Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

When creating a [payer type](../person-type/index.md), you can specify any name, but you need to define the status: 
- `I` — individual,
- `E` — legal entity.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Place an Order on the Website](https://helpdesk.bitrix24.com/open/17300484/)

## Relationship of Payer Type Statuses with Other Objects

**Payer Types.** Specify the payer type for which you are configuring the correspondence to an individual or legal entity. You can access the available payer types using the method [sale.persontype.list](../person-type/sale-person-type-list.md).

## How to Get Started

1. Retrieve the list of payer types using [sale.persontype.list](../person-type/sale-person-type-list.md).
2. Select the domain value: `I` for an individual or `E` for a legal entity.
3. Create a mapping using [sale.businessValuePersonDomain.add](./sale-business-value-person-domain-add.md).
4. Check the list of mappings using [sale.businessValuePersonDomain.list](./sale-business-value-person-domain-list.md).
5. If the mapping is no longer needed, delete it using [sale.businessValuePersonDomain.deleteByFilter](./sale-business-value-person-domain-delete-by-filter.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

#| 
|| **Method** | **Description** ||
|| [sale.businessValuePersonDomain.add](./sale-business-value-person-domain-add.md) | Adds correspondence to an individual or legal entity ||
|| [sale.businessValuePersonDomain.list](./sale-business-value-person-domain-list.md) | Returns a list of correspondences to an individual or legal entity ||
|| [sale.businessValuePersonDomain.deleteByFilter](./sale-business-value-person-domain-delete-by-filter.md) | Deletes correspondence to an individual or legal entity ||
|| [sale.businessValuePersonDomain.getFields](./sale-business-value-person-domain-get-fields.md) | Returns fields of correspondence to an individual or legal entity ||
|#