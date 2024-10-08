# Add User to Existing Task rpa.task.addUser

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- Examples are missing
- Success response is missing
- Error response is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.task.addUser` will add a user to an existing task.

#|
|| **Parameter** / **Type** | **Description** ||
|| **typeId** 
[`number`](../../../data-types.md) | Process identifier. ||
|| **stageId** 
[`number`](../../../data-types.md) | Stage identifier. ||
|| **automationRuleName** 
[`string`](../../../data-types.md) | Name of the Automation rule. ||
|| **userValue** 
[`string`](../../../data-types.md) | String with the user in the format "First Last [user id]". ||
|#