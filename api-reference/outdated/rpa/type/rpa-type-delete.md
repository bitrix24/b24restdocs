# Delete process rpa.type.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

The method `rpa.type.delete` deletes a process.

#|
|| **Parameter** / **Type** | **Description** ||
|| **id**^*^ 
[`number`](../../../data-types.md) | Identifier of the process. ||
|#

{% include [Parameter notes](../../../../_includes/required.md) %}