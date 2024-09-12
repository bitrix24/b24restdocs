# Delete Additional Property of Storage Items entity.item.property.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`entity`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method deletes additional properties of storage items. The user must have management rights (**X**) for the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../../data-types.md) | Required. String identifier of the storage. ||
|| **PROPERTY^*^**
[`string`](../../../data-types.md) | Required. String identifier of the property. ||
|#

{% include [Note on parameters](../../../../_includes/required.md) %}

## Example

Call

```js
BX24.callMethod(
    'entity.item.property.delete',
    {
        ENTITY: 'menu_new',
        PROPERTY: 'new_prop'
    }
);
```

Request

```http
https://my.bitrix24.com/rest/entity.item.property.delete.json?ENTITY=menu_new&PROPERTY=new_prop&auth=d92dd12b9b9b904254776104eed2bb76
```

{% include [Note on examples](../../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{"result":true}
```