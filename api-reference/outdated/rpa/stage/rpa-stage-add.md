# Create a New Stage rpa.stage.add

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative to this functionality.

{% endnote %}

This method adds a new stage.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***  
[`object`](../../../data-types.md) | An object with [fields](#fields) of the stage ||
|#

## Fields Parameters {#fields}

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name*** | Name of the stage ||
|| **typeId*** | Identifier of the process ||
|| **code** | Symbolic code ||
|| **color** | Color of the stage in HEX format (6 characters) ||
|| **sort** | Sort identifier ||
|| **semantic** | Semantic code of the stage. It can be either `SUCCESS` or `FAIL`.

A process can have only one successful stage ||
|#

## Response Handling

HTTP Status: **200**

Returns data in the response similar to the response from the [rpa.stage.get](./rpa-stage-get.md) request.

## Continue Exploring 

- [{#T}](./index.md)
- [{#T}](./rpa-stage-update.md)
- [{#T}](./rpa-stage-get.md)
- [{#T}](./rpa-stage-list-for-type.md)
- [{#T}](./rpa-stage-delete.md)