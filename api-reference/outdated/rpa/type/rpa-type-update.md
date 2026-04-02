# Update the process rpa.type.update

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative to this functionality.

{% endnote %}

This method updates the process by its `id`.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`number`](../../../data-types.md) | Identifier of the process ||
|| **fields** 
[`array`](../../../data-types.md) | List of process fields. Fully analogous to the set from the [rpa.type.add](./rpa-type-add.md) method ||
|#

## Response Handling

HTTP status: **200**

Returns a response similar to the [rpa.type.get](./rpa-type-get.md) method.

## Continue Your Exploration 

- [{#T}](./index.md)
- [{#T}](./rpa-type-add.md)
- [{#T}](./rpa-type-get.md)
- [{#T}](./rpa-type-list.md)
- [{#T}](./rpa-type-delete.md)