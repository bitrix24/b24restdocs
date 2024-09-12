# When Changing the Measurement Unit CATALOG.MEASURE.ON.UPDATE

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

The event `CATALOG.MEASURE.ON.UPDATE` is triggered when the measurement unit is changed. The following data is passed to the handler:

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity that triggered the event. || 
|#