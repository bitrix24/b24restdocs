# Delete Department department.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- no response in case of error
- no examples
  
{% endnote %}

{% endif %}

> Scope: [`department`](../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to modify the structure

Deletes the specified department.

#|
|| **Parameter** | **Description** ||
|| **ID^*^** | department identifier ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}

## Call

```js
BX24.callMethod('department.delete', {"ID": 222});
```

## Request

```
https://my.bitrix24.com/rest/department.delete.json?ID=222&auth=70a32986f1bf204dec4567147ca6a2af
```

## Response

> 200 OK

```json
{"result":true}
```