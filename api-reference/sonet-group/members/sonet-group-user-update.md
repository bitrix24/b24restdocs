# Change User Role in Group sonet_group.user.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- no response in case of error
- no response in case of success
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

This method allows you to change the role of a user or users in a working group. To perform this operation, the current user must have administrator rights in the social network. It is important to note that if the current role of the user is the owner of the group, it cannot be changed using this method.

## Call Parameters

#|
|| **Parameter** | **Description** ||
|| **GROUP_ID** | ID of the working group. ||
|| **USER_ID** | ID of the user (or an array of IDs) whose role is being changed. ||
|| **ROLE** | Code of the new role for the group member (available values are `E` - moderator and `K` - participant). ||
|#

{% include [Notes on parameters](../../../_includes/required.md) %}

## Example

```js
// Changing the roles of users with ID=10 and 21 in the social network group with ID=15 to moderators
BX24.callMethod('sonet_group.user.update', {
    GROUP_ID: 15,
    USER_ID: [ 10, 21 ],
    ROLE: 'E'
});
```
{% include [Notes on examples](../../../_includes/examples.md) %}