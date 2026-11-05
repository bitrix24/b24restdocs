# Statuses of Payer Types in the Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A [payer type](../person-type/index.md) can have any name, so the name does not tell you who pays — a person or a company. Who pays is defined by the mapping of the payer type to an individual or legal entity. In the REST API, this is the `businessValuePersonDomain` object. Each payer type can have one mapping. Its value is called the domain:

- `I` — individual
- `E` — legal entity

The domain is used for order export to an ERP system and for the business meanings settings. Business meanings match order data to the fields that payment systems require. A payer type without a mapping is not included in the business meanings settings.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Place an Order on the Website](https://helpdesk.bitrix24.com/open/17300484/)

## Relationship of Payer Type Statuses with Other Objects

**Payer Types.** A mapping is set for a payer type: pass its ID in the `personTypeId` field. The [sale.persontype.list](../person-type/sale-person-type-list.md) method returns the IDs of payer types.

## How to Get Started

1. Retrieve the list of payer types using [sale.persontype.list](../person-type/sale-person-type-list.md)
2. Check which fields a mapping requires using [sale.businessValuePersonDomain.getFields](./sale-business-value-person-domain-get-fields.md)
3. Check whether the payer type already has a mapping using [sale.businessValuePersonDomain.list](./sale-business-value-person-domain-list.md)
4. Create a mapping using [sale.businessValuePersonDomain.add](./sale-business-value-person-domain-add.md): `I` for an individual or `E` for a legal entity
5. To change the domain, delete the mapping using [sale.businessValuePersonDomain.deleteByFilter](./sale-business-value-person-domain-delete-by-filter.md) and create it again: there is no update method

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [sale.businessValuePersonDomain.add](./sale-business-value-person-domain-add.md) | Adds correspondence to an individual or legal entity ||
|| [sale.businessValuePersonDomain.list](./sale-business-value-person-domain-list.md) | Returns a list of correspondences to an individual or legal entity ||
|| [sale.businessValuePersonDomain.deleteByFilter](./sale-business-value-person-domain-delete-by-filter.md) | Deletes correspondence to an individual or legal entity ||
|| [sale.businessValuePersonDomain.getFields](./sale-business-value-person-domain-get-fields.md) | Returns fields of correspondence to an individual or legal entity ||
|#