# Add Storage Element entity.item.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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

The method `entity.item.add` adds a storage element. The user must have at least write access (**W**) to the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Required. String identifier of the storage. ||
|| **NAME**^*^
[`string`](../../data-types.md) | Required. Name of the element. ||
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
|| **PROPERTY_VALUES**
[`unknown`](../../data-types.md) | Associative list of property values for the element. Storage properties are created using [entity.item.property.add](./properties/entity-item-property-add.md). ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'entity.item.add',
        {
            ENTITY: 'menu_new',
            DATE_ACTIVE_FROM: new Date(),
            DETAIL_PICTURE: '',
            NAME: 'Hello, world!',
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
    https://my.bitrix24.com/rest/entity.item.add.json?DATE_ACTIVE_FROM=2013-06-26T11%3A54%3A30.421Z&DETAIL_PICTURE=&ENTITY=menu_new&NAME=Hello%2C%20world!&PROPERTY_VALUES%5Btest1%5D=22&PROPERTY_VALUES%5Btest%5D=11&PROPERTY_VALUES%5Btest_file%5D=&SECTION=219&auth=9affe382af74d9c5caa588e28096e872
    ```

{% endlist %}

{% include [Example Note](../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{"result":842}
```