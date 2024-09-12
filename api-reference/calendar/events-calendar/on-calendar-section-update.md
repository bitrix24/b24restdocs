# OnCalendarSectionUpdate Event

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- field types are not specified
- field requirements are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnCalendarSectionUpdate` event is triggered when a section/resource is modified. The following data is passed to the handler:

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity that triggered the event. ||
|#