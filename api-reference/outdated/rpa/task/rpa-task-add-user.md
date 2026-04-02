# Add User to Existing Task rpa.task.addUser

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative to this functionality.

{% endnote %}

The method `rpa.task.addUser` will add a user to an existing task.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`integer`](../../../data-types.md) | Process identifier ||
|| **stageId** 
[`integer`](../../../data-types.md) | Stage identifier ||
|| **Automation rule name** 
[`string`](../../../data-types.md) | Name of the Automation rule ||
|| **userValue** 
[`string`](../../../data-types.md) | String with user in the format `First Last [user ID]` ||
|#

## Continue Your Exploration 

- [{#T}](./index.md)
- [{#T}](./rpa-task-do.md)
- [{#T}](./rpa-task-delete.md)