# Links Between Requisites and CRM Objects

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform methods: any user

Links maintain the correspondence between a CRM object and the requisites used in the context of that object.

For example, there is an invoice. To print it, the requisites of the selling company (my company) and the buying company (the client) are required. Since a company may have multiple requisites, it is unclear which ones to use for printing the invoice. This is where the link is needed to specify the required requisites.

The fields **REQUISITE_ID** and **BANK_DETAIL_ID** store the identifiers of the requisite and bank requisite, respectively, used for the buying company. The fields **MC_REQUISITE_ID** and **MC_BANK_DETAIL_ID** store similar identifiers for the selling company.

If any identifier has a value of `0`, it is considered unselected. The requisites of the selling company or the bank requisites may not be selected.

## Fields of the Link Between Requisite and CRM Object

#|
|| **Name** 
`type` | **Description** | **Read** | **Write** | **Required** | **Immutable** ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the object type to which the link pertains.

The following types can be used:
- deal (value `2`)
- old invoice (value `5`)
- estimate (value `7`)
- new invoice (value `31`)
- other dynamic objects (to get possible values, see the method [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md)).

Identifiers of CRM object types can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) | Yes | Yes | Yes | Yes ||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | Identifier of the object to which the link pertains. 

Identifiers of objects can be obtained using the following methods: [crm.deal.list](../../deals/crm-deal-list.md), [crm.quote.list](../../quote/crm-quote-list.md), [crm.item.list](../../universal/crm-item-list.md). | Yes | Yes | Yes | Yes ||
|| **REQUISITE_ID**
[`integer`](../../../data-types.md) | Identifier of the client's requisite selected for the object. 

Identifiers of requisites can be obtained using the method [crm.requisite.list](../universal/crm-requisite-list.md) | Yes | Yes | Yes | No ||
|| **BANK_DETAIL_ID**
[`integer`](../../../data-types.md) | Identifier of the client's bank requisite selected for the object. 

Identifiers of bank requisites can be obtained using the method [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) | Yes | Yes | Yes | No ||
|| **MC_REQUISITE_ID**
[`integer`](../../../data-types.md) | Identifier of my company's requisite selected for the object. 

Identifiers of requisites can be obtained using the method [crm.requisite.list](../universal/crm-requisite-list.md) | Yes | Yes | Yes | No ||
|| **MC_BANK_DETAIL_ID**
[`integer`](../../../data-types.md) | Identifier of my company's bank requisite selected for the object. 

Identifiers of bank requisites can be obtained using the method [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) | Yes | Yes | Yes | No ||
|#

## Overview of Methods

#|
|| **Method** | **Description** ||
|| [crm.requisite.link.register](./crm-requisite-link-register.md) | Registers the link between requisites and an object ||
|| [crm.requisite.link.get](./crm-requisite-link-get.md) | Returns the link between requisites and an object ||
|| [crm.requisite.link.list](./crm-requisite-link-list.md) | Returns a list of links between requisites based on a filter ||
|| [crm.requisite.link.unregister](./crm-requisite-link-unregister.md) | Removes the link between requisites and an object ||
|| [crm.requisite.link.fields](./crm-requisite-link-fields.md) | Returns a formal description of the fields of the requisites link ||
|#