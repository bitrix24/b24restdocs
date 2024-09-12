# Send a request to join the group sonet_group.user.request

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

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

This method sends a request from the current user to join a social network group, while checking the current user's access rights to the group. If the group is open for free joining, the user immediately becomes a participant.

## Returns `true` if the request was successful, or an error message.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **GROUP_ID** | ID of the group to which the request is sent. ||
|| **MESSAGE** | Text of the request. ||
|#

{% include [Notes on parameters](../../../_includes/required.md) %}

## Example

```js
// Sending a request to join the group with ID=17
BX24.callMethod('sonet_group.user.request', {
    'GROUP_ID': 17,
    'MESSAGE': 'Request'
});
```
{% include [Notes on examples](../../../_includes/examples.md) %}

## Request:

```
https://mydomain.bitrix24.com/rest/sonet_group.user.request.json?auth=52423d4a5f19f5f964f9b4e96a925cfa&GROUP_ID=17&MESSAGE=Request
```

## Response:

>200 OK

```json
{"result":true}
```