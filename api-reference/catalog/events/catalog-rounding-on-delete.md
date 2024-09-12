# When Deleting the Price Rounding Rule CATALOG.ROUNDING.ON.DELETE

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- field types are not specified
- field mandatory status is not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `CATALOG.ROUNDING.ON.DELETE` is triggered when a price rounding rule is deleted. The following data is passed to the handler:

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity for which the event was triggered. || 
|#