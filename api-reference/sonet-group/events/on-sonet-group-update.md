# On Changing the Working Group onSonetGroupUpdate

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- what data is passed in the event
- missing parameters, types of parameters
- missing examples

{% endnote %}

{% endif %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onSonetGroupUpdate` is triggered after a working group is modified. It is a proxy for the event `OnSocNetGroupUpdate`.

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the entity that triggered the event. ||
|#
{% include [Note on parameters](../../_includes/required.md) %}