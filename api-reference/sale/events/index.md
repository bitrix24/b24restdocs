# Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md) 
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [OnSaleOrderSaved](./on-sale-order-saved.md) | At the end of order saving ||
|| [OnSaleBeforeOrderDelete](./on-sale-before-order-delete.md) | Before order deletion ||
|| [OnPropertyValueEntitySaved](./on-property-value-entity-saved.md) | After saving order property ||
|| [OnPaymentEntitySaved](./on-payment-entity-saved.md) | After saving payment ||
|| [OnShipmentEntitySaved](./on-shipment-entity-saved.md) | After saving shipment ||
|| [OnOrderEntitySaved](./on-order-entity-saved.md) | After saving order ||
|| [OnPropertyValueDeleted](./on-property-value-deleted.md) | When order property is deleted from the database ||
|| [OnPaymentDeleted](./on-payment-deleted.md) | When payment is deleted from the database ||
|| [OnShipmentDeleted](./on-shipment-deleted.md) | When shipment is deleted from the database ||
|| [OnOrderDeleted](./on-order-deleted.md) | When order is deleted from the database ||
|#