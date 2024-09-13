# Triggered when a workgroup is deleted onSonetGroupDelete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

The `onSonetGroupDelete` event is triggered at the moment a workgroup is deleted. It is a proxy to the `OnSocNetGroupDelete` event.

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity that triggered the event. ||
|#
{% include [Parameters note](../../_includes/required.md) %}