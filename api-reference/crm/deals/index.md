# About Deals in Bitrix24

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- An introduction corresponding to the title is needed.

{% endnote %}

{% endif %}

{% note info "Permissions" %}

**Scope**: [`crm`](../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The following methods are available for deals in Bitrix24:

#|
|| **Method** | **Description** ||
|| [crm.deal.fields](./crm-deal-fields.md) | Deal fields. ||
|| [crm.deal.add](./crm-deal-add.md) | Create a new deal. ||
|| [crm.deal.update](./crm-deal-update.md) | Update a deal. ||
|| [crm.deal.get](./crm-deal-get.md) | Retrieve a deal by Id. ||
|| [crm.deal.list](./crm-deal-list.md) | Get a list of deals. ||
|| [crm.deal.delete](./crm-deal-delete.md) | Delete a deal. ||
|| [crm.deal.productrows.set](./crm-deal-productrows-set.md) | Add products to a deal. ||
|| [crm.deal.productrows.get](./crm-deal-get.md) | Retrieve products of a deal. ||
|#

## See also

- [Events](./events/index.md)
- [Recurring Deals](./recurring-deals/index.md)
- [Custom Fields](./user-defined-fields/index.md)
- [Contacts in a Deal](./contacts/index.md)