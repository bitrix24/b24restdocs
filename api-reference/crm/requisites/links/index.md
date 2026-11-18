# Linking Company Details to CRM Objects: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Links maintain the correspondence between a CRM object and the Company details used in the context of that object.

For example, to print an invoice, you need the Company details of the seller (my company) and the buyer (the customer). Each company may have several sets of Company details, and the link specifies which ones to use for the invoice.

The fields **REQUISITE_ID** and **BANK_DETAIL_ID** store the identifiers of the customer's Company details and banking details. The customer can be a company or a contact. The fields **MC_REQUISITE_ID** and **MC_BANK_DETAIL_ID** store similar identifiers for my company.

A value of `0` in a Company details or banking details field means that the corresponding details are not selected. You can pass `0` in all four fields.

The `register`, `get`, `unregister`, and `fields` methods are deprecated: their counterparts are in the REST 3.0 [Requisites](../../rest-v3/requisites/index.md) group, and the limitations of the replacement are described on the method pages. The `list` method has no counterpart.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [How to use your company's Company details](https://helpdesk.bitrix24.com/open/25892275/)

## Getting Started

1. Determine the CRM object type `ENTITY_TYPE_ID`
2. Retrieve the object identifier `ENTITY_ID`
3. Find the customer's Company details using the [crm.requisite.list](../universal/crm-requisite-list.md) method
4. Find the banking details using the [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) method
5. Register a link using the [crm.requisite.link.register](./crm-requisite-link-register.md) method
6. Verify the link using the [crm.requisite.link.get](./crm-requisite-link-get.md) or [crm.requisite.link.list](./crm-requisite-link-list.md) method

## Fields of the Link Between Requisite and CRM Object

Required fields are marked with `*`.

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the object type to which the link belongs.

The following types can be used:
- deal (value `2`)
- old invoice (value `5`)
- estimate (value `7`)
- new invoice (value `31`)
- other dynamic objects (to get possible values, see the method [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md))

Object type identifiers can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) ||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Identifier of the object to which the link refers.

Object identifiers can be obtained using the following methods: [crm.deal.list](../../deals/crm-deal-list.md), [crm.quote.list](../../quote/crm-quote-list.md), [crm.item.list](../../universal/crm-item-list.md) ||
|| **REQUISITE_ID***
[`integer`](../../../data-types.md) | Identifier of the client's details selected for the object.

Details identifiers can be obtained using the [crm.requisite.list](../universal/crm-requisite-list.md) method ||
|| **BANK_DETAIL_ID***
[`integer`](../../../data-types.md) | Identifier of the client's bank requisite selected for the object.

Bank requisite identifiers can be obtained using the method [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) ||
|| **MC_REQUISITE_ID***
[`integer`](../../../data-types.md) | Identifier of my company's details selected for the object.

Details identifiers can be obtained using the [crm.requisite.list](../universal/crm-requisite-list.md) method ||
|| **MC_BANK_DETAIL_ID***
[`integer`](../../../data-types.md) | Identifier of my company's bank details selected for the object.

Bank details identifiers can be obtained using the [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) method ||
|#

Example of a link object from the [crm.requisite.link.get](./crm-requisite-link-get.md) response: the customer's Company details `305` are selected, while their banking details are not (`0`); for my company, Company details `3` and banking details `3` are selected.

```json
{
    "ENTITY_TYPE_ID": 2,
    "ENTITY_ID": 6553,
    "REQUISITE_ID": "305",
    "BANK_DETAIL_ID": "0",
    "MC_REQUISITE_ID": "3",
    "MC_BANK_DETAIL_ID": "3"
}
```

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: `get` requires permission to read the CRM object, `register` and `unregister` require permission to edit it. Requirements for `list` and `fields` are described on the method pages

#|
|| **Method** | **Description** ||
|| [crm.requisite.link.register](./crm-requisite-link-register.md) | Registers the connection of requisites with an entity ||
|| [crm.requisite.link.get](./crm-requisite-link-get.md) | Returns the connection of requisites with an entity ||
|| [crm.requisite.link.list](./crm-requisite-link-list.md) | Returns a list of requisites connections by filter ||
|| [crm.requisite.link.unregister](./crm-requisite-link-unregister.md) | Deletes the connection of requisites with an entity ||
|| [crm.requisite.link.fields](./crm-requisite-link-fields.md) | Returns the formal description of fields for requisites connections ||
|#
