# When Deleting a Product CATALOG.PRODUCT.ON.DELETE

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- field types are not specified
- field requirements are not indicated
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `CATALOG.PRODUCT.ON.DELETE` is triggered when a product is deleted. The following data is passed to the handler:

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity for which the event was triggered. || 
|#