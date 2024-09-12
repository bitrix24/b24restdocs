# Delete Timeline Entry rpa.timeline.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.timeline.delete` will delete a timeline entry with the identifier id.

#|
|| **Parameter** / **Type** | **Description** ||
|| **id**^*^ 
[`number`](../../../data-types.md) | Identifier of the entry. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

{% note warning %}

This method allows deleting only those entries that were created by the same user and through the application.

{% endnote %}