# Delete Stage rpa.stage.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.stage.delete` removes a stage.

#| 
|| **Name** 
`type` | **Description** || 
|| **id**^*^ 
[`number`](../../../data-types.md) | Identifier of the stage. || 
|#

{% include [Footnote on parameters](../../../../_includes/required.md) %}

{% note warning %}

A process must always have one successful stage. A successful stage cannot be deleted.

{% endnote %}