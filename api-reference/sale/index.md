# Online Store

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- need to outline the logic
- think about the logical order of sections

{% endnote %}

{% endif %}

{% note info "Permissions" %}

> Scope: [`sale`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

{% endnote %}

Namespaces for methods related to the Online Store:

#| 
|| **Namespace** | **Description** ||
|| [Basket](./basket-item/index.md) | Methods for working with the Basket ||
|| [Basket Properties](./basket-properties/index.md) | Methods for working with basket properties ||
|| [Order](./order/index.md) | Methods for working with Orders ||
|| [Orders from External Sources](./trade-binding/index.md) | Methods for working with Orders from external sources ||
|| [Order Properties](./property/index.md) | Methods for working with order properties ||
|| [Property Groups](./property-group/index.md) | Methods for working with property groups ||
|| [Order Property Variants of Type ENUM](./property-variant/index.md) | Methods for working with order property variants of type ENUM ||
|| [Trading Platforms](./trade-platform/index.md) | Methods for working with Trading Platforms ||
|| [Payments](./payment/index.md) | Methods for working with Payments ||
|| [Payer Types](./person-type/index.md) | Methods for working with Payer Types ||
|| [Payer Type Statuses](./business-value-person-domain/index.md) | Methods for configuring the correspondence of payer types to individuals or legal entities ||
|| [Shipments](./shipment/index.md) | Methods for working with Shipments ||
|| [Shipment Line Items](./shipment-item/index.md) | Methods for working with shipment line items ||
|| [Shipment Properties](./shipment-property/index.md) | Methods for working with shipment properties ||
|| [Shipment Property Values](./shipment-property-value/index.md) | Methods for working with shipment property values ||
|| [Statuses](./status/index.md) | Methods for working with statuses ||
|| [Status Localization](./status-lang/index.md) | Methods for working with status localization ||
|| [Linking Basket Item to Payment](./payment-item-basket/index.md) | Methods for linking basket items to payments ||
|| [Linking Payments to Shipments](./payment-item-shipment/index.md) | Methods for linking payments to shipments ||
|| [Linking Properties](./property-relation/index.md) | Methods for linking properties ||
|| [Property Values](./property-value/index.md) | Methods for working with property values ||
|| [Cash Registers](./cashbox/index.md) | Methods for working with cash registers and receipts ||
|| [Delivery Services](./delivery/delivery/index.md) | Methods and webhooks for working with delivery services ||
|#