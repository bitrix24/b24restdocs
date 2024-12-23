# Invite users to group sonet_group.user.invite

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no error response is provided
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

This method invites users to a social network group on behalf of the current user, while checking the current user's access rights to the group.

## Returns an array of IDs of users successfully invited to the group.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **GROUP_ID** | ID of the group to which the invitation is sent. ||
|| **USER_ID** | ID of the user (or an array of user IDs) being invited to the group. ||
|| **MESSAGE** | Invitation text. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    // Inviting user with ID=3 to social network group with ID=15
    BX24.callMethod('sonet_group.user.invite', {
        'GROUP_ID': 15,
        'USER_ID': 3,
        'MESSAGE': 'Invitation'
    });
    ```

{% endlist %}


{% include [Note on examples](../../../_includes/examples.md) %}


## Request:

```
https://mydomain.bitrix24.com/rest/sonet_group.user.invite.json?auth=52423d4a5f19f5f964f9b4e96a925cfa&GROUP_ID=15&USER_ID=3&MESSAGE=Invitation
```

## Response:

>200 OK

```json
{"result":["3"]}
```