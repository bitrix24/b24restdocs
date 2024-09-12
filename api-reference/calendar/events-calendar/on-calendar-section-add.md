# When Adding a Calendar Section OnCalendarSectionAdd

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

The `OnCalendarSectionAdd` event is triggered when a calendar section is added. It will also be triggered when a resource is added.

The following data is passed to the handler:

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity that triggered the event. ||
|#

{% note info %}

Technically, a resource is equivalent to a section. Since the created resources are placed in a special type of calendars and a section is created for each resource, these events will be triggered when resources are added or removed.

{% endnote %}