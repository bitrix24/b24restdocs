# Delete Social Network Group sonet_group.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- no error response is provided
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

Deletes a social network group. To perform this operation, the current user must either be the owner of the group or have administrator rights in the social network.

## Function Parameters

#|
|| **Parameter** | **Description** ||
|| **GROUP_ID** | ID of the group to be deleted. ||
|#

{% include [Parameter Note](../../_includes/required.md) %}

In case of successful deletion of the group, it returns **true**, otherwise - an error message.

## Example

```js
// Deleting social network group with ID=11

BX24.callMethod('sonet_group.delete', {
    'GROUP_ID': 11
});
```
{% include [Example Note](../../_includes/examples.md) %}

## Request:

```http
https://mydomain.bitrix24.com/rest/sonet_group.delete.json?auth=803f65e30340ff39703f8061c8b63a10&GROUP_ID=11
```

## Response:

>200 OK

```json
{"result":true}
```