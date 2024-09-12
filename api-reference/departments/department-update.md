# Update Department department.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- no response in case of error
- no examples
  
{% endnote %}

{% endif %}

> Scope: [`department`](../scopes/permissions.md)
>
> Who can execute the method: user with permissions to modify the structure

This method updates the specified department.

#|
|| **Parameter** | **Description** ||
|| **ID^*^** | department identifier ||
|| **NAME^*^** | department name ||
|| **SORT** | department sorting parameter ||
|| **PARENT** | identifier of the parent department ||
|| **UF_HEAD** | identifier of the department head ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}

## Call

```js
BX24.callMethod('department.update', {"ID": 222, "NAME": "Old Department", "PARENT": 114, "UF_HEAD": 11});
```

## Request

```
https://my.bitrix24.com/rest/department.update.json?ID=222&NAME=Old Department&PARENT=114&UF_HEAD=11&auth=2577a0459ed8f183c996f44f0995ebe5
```

## Response

> 200 OK

```json
{"result":true}
```