# When Deleting the Event OnCalendarEntryDelete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- field types are not specified
- field requirements are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnCalendarEntryDelete` event is triggered when an event is deleted. The following data is passed to the handler:

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity for which the event was triggered. ||
|#