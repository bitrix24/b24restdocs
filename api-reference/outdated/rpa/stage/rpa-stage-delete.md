# Delete Stage rpa.stage.delete

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

The method `rpa.stage.delete` removes a stage.

#|
|| **Parameter** / **Type** | **Description** ||
|| **id**^*^ 
[`number`](../../../data-types.md) | Identifier of the stage. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

{% note warning %}

There must always be one successful stage in the process. A successful stage cannot be deleted.

{% endnote %}