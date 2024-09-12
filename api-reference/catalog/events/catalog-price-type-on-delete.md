# When Deleting Price Type CATALOG.PRICE.TYPE.ON.DELETE

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- field types are not specified
- field requirements are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `CATALOG.PRICE.TYPE.ON.DELETE` is triggered when a price type is deleted. The following data is passed to the handler:

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity for which the event was triggered. ||
|#