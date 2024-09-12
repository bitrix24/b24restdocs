# When Adding the OnCalendarEntryAdd Event

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- field types are not specified
- field requirements are not indicated
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnCalendarEntryAdd` event is triggered when an event is added. The following data is passed to the handler:

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity that triggered the event. || 
|#