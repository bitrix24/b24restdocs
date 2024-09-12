# Delete Storage Element entity.item.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `entity.item.delete` removes a storage element. The user must have at least write access (**W**) to the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../data-types.md) | Required. String identifier of the storage. ||
|| **ID^*^**
[`integer`](../../data-types.md) | Required. Identifier of the element. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Example

Call

```js
BX24.callMethod(
    'entity.item.delete',
    {
        ENTITY: 'menu_new',
        ID: 842
    }
);
```

Request

```http
https://my.bitrix24.com/rest/entity.item.delete.json?ENTITY=menu_new&ID=842&auth=340bf57f35ee95e0debf98399632999c
```

{% include [Example Note](../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{"result":true}
```