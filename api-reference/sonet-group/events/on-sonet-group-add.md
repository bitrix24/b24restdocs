# On Adding a New Working Group onSonetGroupAdd

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

The `onSonetGroupAdd` event is triggered after a new working group is added. It is a proxy to the `OnSocNetGroupAdd` event.

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity that triggered the event. ||
|#
{% include [Note on parameters](../../_includes/required.md) %}