# Change Owner of Group sonet_group.setowner

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no response in case of error
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method changes the owner of a group. It can be executed either by the network administrator or the current owner of the group.

## Parameters:

#|
|| **Parameter** | **Description** ||
|| **GROUP_ID** | The identifier of the group whose owner is being changed. ||
|| **USER_ID** | The identifier of the new owner. ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}

On success, it returns `true`.

## Example

```js
BX24.callMethod('sonet_group.setowner', {
    'GROUP_ID': 11,
    'USER_ID': 2
});
```
{% include [Example Notes](../../_includes/examples.md) %}