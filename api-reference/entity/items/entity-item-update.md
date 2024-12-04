# Update Storage Item entity.item.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `entity.item.update` updates a storage item. The user must have at least write access permission (**W**) in the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../data-types.md) | Required. String identifier of the storage. ||
|| **ID^*^**
[`integer`](../../data-types.md) | Required. Identifier of the item. ||
|| **NAME**
[`string`](../../data-types.md) | Name of the item. ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | Active flag of the item (Y\|N). ||
|| **DATE_ACTIVE_FROM**
[`unknown`](../../data-types.md) | Date when the item becomes active. ||
|| **DATE_ACTIVE_TO**
[`unknown`](../../data-types.md) | Date when the item is no longer active. ||
|| **SORT**
[`unknown`](../../data-types.md) | Sorting weight of the item. ||
|| **PREVIEW_PICTURE**
[`unknown`](../../data-types.md) | Preview image of the item. ||
|| **PREVIEW_TEXT**
[`unknown`](../../data-types.md) | Preview text of the item. ||
|| **DETAIL_PICTURE**
[`unknown`](../../data-types.md) | Detailed image of the item. ||
|| **DETAIL_TEXT**
[`unknown`](../../data-types.md) | Detailed text of the item. ||
|| **CODE**
[`unknown`](../../data-types.md) | Symbolic code of the item. ||
|| **SECTION**
[`unknown`](../../data-types.md) | Identifier of the storage section. ||
|| **PROPERTY_VALUES^*^**
[`unknown`](../../data-types.md) | Required. Associative list of property values of the item. Storage properties are created using [entity.item.property.add](./properties/entity-item-property-add.md). ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

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

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.item.update.json?DATE_ACTIVE_FROM=2013-06-26T12%3A03%3A31.653Z&DETAIL_PICTURE=&ENTITY=menu_new&ID=842&NAME=Goodbye%20Cruel%20World&PROPERTY_VALUES%5Btest1%5D=22&PROPERTY_VALUES%5Btest%5D=11&PROPERTY_VALUES%5Btest_file%5D=&SECTION=219&auth=9affe382af74d9c5caa588e28096e872
    ```

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{"result":true}
```