# Linking Company Details to CRM Objects: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Links maintain the correspondence between a CRM object and the Company details used in the context of that object.

For example, there is an invoice. To print it, the Company details of the selling company (my company) and the buying company (the customer) are required. Since a company may have multiple sets of Company details, it is unclear which ones to use for printing the invoice. This is where a link is needed to specify the required Company details.

The fields **REQUISITE_ID** and **BANK_DETAIL_ID** store the identifiers of the Company details and banking details, respectively, used for the buying company. The fields **MC_REQUISITE_ID** and **MC_BANK_DETAIL_ID** store similar identifiers for the selling company.

If any identifier has a value of `0`, it is considered unselected. The Company details of the selling company or the banking details may not be selected.

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

## Link Identifiers

- `ENTITY_TYPE_ID` — the CRM object type for which Company details are being selected. Values can be retrieved using the [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) method
- `ENTITY_ID` — the CRM object identifier. For deals, this is returned by [crm.deal.list](../../deals/crm-deal-list.md); for estimates, by [crm.quote.list](../../quote/crm-quote-list.md); and for new invoices and dynamic objects, by [crm.item.list](../../universal/crm-item-list.md)
- `REQUISITE_ID` and `MC_REQUISITE_ID` — the identifiers of the customer's Company details and my company's Company details. These can be retrieved using the [crm.requisite.list](../universal/crm-requisite-list.md) method
- `BANK_DETAIL_ID` and `MC_BANK_DETAIL_ID` — the identifiers of the customer's banking details and my company's banking details. These can be retrieved using the [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) method

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
- other dynamic objects (to get possible values, see the method [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md)).

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

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method — access permissions are checked against the CRM object the requisites are linked to

#|
|| **Method** | **Description** ||
|| [crm.requisite.link.register](./crm-requisite-link-register.md) | Registers the connection of requisites with an entity ||
|| [crm.requisite.link.get](./crm-requisite-link-get.md) | Returns the connection of requisites with an entity ||
|| [crm.requisite.link.list](./crm-requisite-link-list.md) | Returns a list of requisites connections by filter ||
|| [crm.requisite.link.unregister](./crm-requisite-link-unregister.md) | Deletes the connection of requisites with an entity ||
|| [crm.requisite.link.fields](./crm-requisite-link-fields.md) | Returns the formal description of fields for requisites connections ||
|#
