# Triggered after creating a group topic onSonetGroupSubjectAdd

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- what data is passed in the event
- missing parameters, parameter types
- missing examples

{% endnote %}

{% endif %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onSonetGroupSubjectAdd` is triggered after a group topic is created. Proxy to the event `OnSocNetGroupSubjectAdd`.

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity that triggered the event. ||
|#
{% include [Note on parameters](../../_includes/required.md) %}