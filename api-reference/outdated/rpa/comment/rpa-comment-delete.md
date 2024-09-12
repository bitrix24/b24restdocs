# Delete Comment rpa.comment.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- Examples are missing
- Success response is absent
- Error response is absent

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.comment.delete` will delete the comment with the identifier id.

#|
|| **Parameter** / **Type** | **Description** ||
|| **id** 
[`number`](../../../data-types.md) | Identifier of the record. ||
|#

{% note warning %}

This method allows deleting only those comments that were added by the same user.

{% endnote %}