# Create a New Stage rpa.stage.add

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds a new stage.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields*** 
[`object`](../../../data-types.md) | An object with [fields](#fields) of the stage ||
|#

## Fields Parameters {#fields}

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name*** | Name of the stage ||
|| **typeId*** | Process identifier ||
|| **code** | Symbolic code ||
|| **color** | Stage color in HEX format with 6 characters ||
|| **sort** | Sort identifier ||
|| **semantic** | Semantic code of the stage. It can be either `SUCCESS` or `FAIL`.

A process can have only one successful stage ||
|#

## Response Handling

HTTP status: **200**

Returns data in the response similar to the response for the request [rpa.stage.get](./rpa-stage-get.md)

## Continue Exploring

- [{#T}](./index.md)
- [{#T}](./rpa-stage-update.md)
- [{#T}](./rpa-stage-get.md)
- [{#T}](./rpa-stage-list-for-type.md)
- [{#T}](./rpa-stage-delete.md)