# Triggered Before Deleting a Workgroup Topic onSonetGroupSubjectDelete

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

The `onSonetGroupSubjectDelete` event is triggered before a workgroup topic is deleted. It is a proxy to the `OnSocNetGroupSubjectDelete` event.

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity that triggered the event. ||
|#
{% include [Parameters Note](../../_includes/required.md) %}