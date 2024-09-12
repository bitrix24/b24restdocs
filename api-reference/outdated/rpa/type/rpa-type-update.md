# Change the process rpa.type.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.type.update` will update the process with the specified id and return data in the response similar to the response from the `request rpa.type.get`.

#|
|| **Parameter** / **Type** | **Description** ||
|| **id** 
[`number`](../../../data-types.md) | ID of the process. ||
|| **fields** 
[`array`](../../../data-types.md) | A list of process fields. Fully analogous to the set from the [rpa.type.add](./rpa-type-add.md) method. ||
|#