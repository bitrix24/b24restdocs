# Update Stage rpa.stage.update

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative functionality.

{% endnote %}

This method updates the stage by `id`.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`number`](../../../data-types.md) | Identifier of the stage ||
|| **fields** 
[`array`](../../../data-types.md) | Object with [fields](#fields) of the stage ||
|#

## Fields Parameters

#|
|| **Name**
`type` | **Description** ||
|| **name** | Name of the stage ||
|| **code** | Symbolic code ||
|| **color** | Color of the stage in HEX format with 6 characters ||
|| **sort** | Sorting identifier ||
|| **semantic** | Semantic code of the stage. It can be either `SUCCESS` or `FAIL`

There must always be one successful stage in the process. The semantics of the successful stage cannot be changed. ||
|#

## Response Handling

HTTP Status: **200**

Returns data in the response similar to the response from the [rpa.stage.get](./rpa-stage-get.md) request.

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-stage-add.md)
- [{#T}](./rpa-stage-get.md)
- [{#T}](./rpa-stage-list-for-type.md)
- [{#T}](./rpa-stage-delete.md)