# Update Additional Property of Storage Elements `entity.item.property.update`

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- examples missing
- error response not provided

{% endnote %}

{% endif %}

> Scope: [`entity`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `entity.item.property.update` updates the additional property of storage elements. The user must have management rights (**X**) for the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../../data-types.md) | Required. String identifier of the storage. ||
|| **PROPERTY^*^**
[`string`](../../../data-types.md) | Required. String identifier of the property. ||
|| **PROPERTY_NEW**
[`string`](../../../data-types.md) | New string identifier of the property. ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the property. ||
|| **TYPE**
[`unknown`](../../../data-types.md) | Type of the property (**S** - string, **N** - number, **F** - file). ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

## Example

Call

```js
BX24.callMethod(
    'entity.item.property.update',
    {
        ENTITY: 'menu_new',
        PROPERTY: 'new_prop',
        NAME: 'Not a New Property Anymore'
    }
);
```

Request

```http
https://my.bitrix24.com/rest/entity.item.property.update.json?ENTITY=menu_new&NAME=Not%20a%20New%20Property%20Anymore&PROPERTY=new_prop&auth=ad5a6f34f14f644136830eb8a936f07f
```

{% include [Example Note](../../../../_includes/examples.md) %}

## Successful Response

> 200 OK
```json
{"result":true}
```