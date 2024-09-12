# Update Storage Element entity.item.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `entity.item.update` updates a storage element. The user must have at least write access permission (**W**) in the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../data-types.md) | Required. String identifier of the storage. ||
|| **ID^*^**
[`integer`](../../data-types.md) | Required. Identifier of the element. ||
|| **NAME**
[`string`](../../data-types.md) | Name of the element. ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | Flag indicating the element's activity (Y\|N). ||
|| **DATE_ACTIVE_FROM**
[`unknown`](../../data-types.md) | Start date of the element's activity. ||
|| **DATE_ACTIVE_TO**
[`unknown`](../../data-types.md) | End date of the element's activity. ||
|| **SORT**
[`unknown`](../../data-types.md) | Sorting weight of the element. ||
|| **PREVIEW_PICTURE**
[`unknown`](../../data-types.md) | Preview image of the element. ||
|| **PREVIEW_TEXT**
[`unknown`](../../data-types.md) | Preview text of the element. ||
|| **DETAIL_PICTURE**
[`unknown`](../../data-types.md) | Detailed image of the element. ||
|| **DETAIL_TEXT**
[`unknown`](../../data-types.md) | Detailed text of the element. ||
|| **CODE**
[`unknown`](../../data-types.md) | Symbolic code of the element. ||
|| **SECTION**
[`unknown`](../../data-types.md) | Identifier of the storage section. ||
|| **PROPERTY_VALUES^*^**
[`unknown`](../../data-types.md) | Required. Associative list of property values of the element. Storage properties are created using [entity.item.property.add](./properties/entity-item-property-add.md). ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Example

Call

```js
BX24.callMethod(
    'entity.item.update',
    {
        ENTITY: 'menu_new',
        ID: 842,
        DATE_ACTIVE_FROM: new Date(),
        DETAIL_PICTURE: '',
        NAME: 'Goodbye Cruel World',
        PROPERTY_VALUES: {
            test: 11,
            test1: 22,
            test_file: ''
        },
        SECTION: 219
    }
);
```

Request

```http
https://my.bitrix24.com/rest/entity.item.update.json?DATE_ACTIVE_FROM=2013-06-26T12%3A03%3A31.653Z&DETAIL_PICTURE=&ENTITY=menu_new&ID=842&NAME=Goodbye%20Cruel%20World&PROPERTY_VALUES%5Btest1%5D=22&PROPERTY_VALUES%5Btest%5D=11&PROPERTY_VALUES%5Btest_file%5D=&SECTION=219&auth=9affe382af74d9c5caa588e28096e872
```

{% include [Example Note](../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{"result":true}
```