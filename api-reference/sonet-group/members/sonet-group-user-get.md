# Get the list of group participants sonet_group.user.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

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

The method returns an array of participants in a social network group by calling `CSocNetUserToGroup::GetList()`, while checking the current user's access rights to the group. The method does not return inactive users (terminated employees).

## Participant fields:

- **USER_ID** - User ID.
- **ROLE** - User role in the group:
  - **SONET_ROLES_OWNER (A)** - owner,
  - **SONET_ROLES_MODERATOR (E)** - moderator,
  - **SONET_ROLES_USER (K)** - user.

## Function parameters

#|
|| **Parameter** | **Description** ||
|| **ID** | ID of the group whose participants need to be retrieved. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Example

```js
// Getting the list of participants in the social network group with ID=15
BX24.callMethod('sonet_group.user.get', {
    'ID': 15
});
```
{% include [Note on examples](../../../_includes/examples.md) %}

## Request:

```
https://mydomain.bitrix24.com/rest/sonet_group.user.get.json?auth=67df5afc8ce59732e4a21ed3e336979f&ID=15
```

## Response:

>200 OK

```json
{"result":[{"USER_ID":"1","ROLE":"A"}]}
```