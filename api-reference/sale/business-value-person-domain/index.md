# Statuses of Payer Types in the Online Store: Overview of Methods

When creating a [payer type](../person-type/index.md), you can specify any name, but you need to define the status: 
- `I` — individual,
- `E` — legal entity.

> Quick navigation: [all methods](#all-methods)

## Relationship of Payer Type Statuses with Other Objects

**Payer Types.** Specify the payer type for which you are configuring the correspondence to an individual or legal entity. You can access the available payer types using the method [sale.persontype.list](../person-type/sale-person-type-list.md).

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