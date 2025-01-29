# Update the process rpa.type.update

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates the process by its `id`.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`number`](../../../data-types.md) | Identifier of the process ||
|| **fields** 
[`array`](../../../data-types.md) | List of process fields. Fully analogous to the set from the method [rpa.type.add](./rpa-type-add.md) ||
|#

## Response Handling

HTTP status: **200**

Returns a response similar to the method [rpa.type.get](./rpa-type-get.md).

## Continue Exploring 

- [{#T}](./index.md)
- [{#T}](./rpa-type-add.md)
- [{#T}](./rpa-type-get.md)
- [{#T}](./rpa-type-list.md)
- [{#T}](./rpa-type-delete.md)