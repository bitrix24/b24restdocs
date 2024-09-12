# Triggered after changing the group topic onSonetGroupSubjectUpdate

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- what data is passed in the event
- missing parameters, parameter types
- missing examples

{% endnote %}

{% endif %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onSonetGroupSubjectUpdate` is triggered after the group topic is changed. Proxy to the event `OnSocNetGroupSubjectUpdate`.

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity that triggered the event. ||
|#
{% include [Note on parameters](../../_includes/required.md) %}