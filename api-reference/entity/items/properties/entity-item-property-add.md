# Add an Additional Property to Storage Elements `entity.item.property.add`

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`entity`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `entity.item.property.add` adds an additional property to storage elements. The user must have management rights (**X**) for the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../../data-types.md) | Required. String identifier of the storage. ||
|| **PROPERTY^*^**
[`string`](../../../data-types.md) | Required. String identifier of the property. ||
|| **NAME^*^**
[`string`](../../../data-types.md) | Required. Name of the property. ||
|| **TYPE^*^**
[`unknown`](../../../data-types.md) | Required. Type of the property (**S** - string, **N** - number, **F** - file). ||
|#

{% include [Note on parameters](../../../../_includes/required.md) %}

## Example

Call

```js
BX24.callMethod(
    'entity.item.property.add',
    {
        ENTITY: 'menu_new',
        PROPERTY: 'new_prop',
        NAME: 'New Property',
        TYPE: 'S'
    }
);
```

Request

```http
https://my.bitrix24.com/rest/entity.item.property.add.json?ENTITY=menu_new&NAME=New%20Property&PROPERTY=new_prop&TYPE=S&auth=e690b44d2b3827d2eb9d4dbe59406dbb
```

{% include [Note on examples](../../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{"result":true}
```