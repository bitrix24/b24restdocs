# Update the entity.section.update storage section

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `entity.section.update` method updates a storage section. The user must have at least write access permission (**W**) in the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../data-types.md) | Required. String identifier of the storage. ||
|| **ID^*^**
[`integer`](../../data-types.md) | Required. Identifier of the section being updated. ||
|| **NAME**
[`string`](../../data-types.md) | Name of the section. ||
|| **DESCRIPTION**
[`unknown`](../../data-types.md) | Description of the section. ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | Active flag of the section (Y\|N). ||
|| **SORT**
[`unknown`](../../data-types.md) | Sorting parameter of the section. ||
|| **PICTURE**
[`unknown`](../../data-types.md) | Picture of the section. ||
|| **DETAIL_PICTURE**
[`unknown`](../../data-types.md) | Detailed picture of the section. ||
|| **SECTION**
[`unknown`](../../data-types.md) | Identifier of the parent section. ||
|#

{% include [Parameter note](../../../_includes/required.md) %}

## Example

Call
```js
BX24.callMethod(
    'entity.section.update',
    {
        ENTITY: 'menu_new',
        ID: 220,
        NAME: 'Not a very test section'
    }
);
```

Request
```http
https://my.bitrix24.com/rest/entity.section.update.json?auth=9affe382af74d9c5caa588e28096e872&ENTITY=menu_new&ID=220&NAME=Not%20a%20very%20test%20section
```

{% include [Example note](../../../_includes/examples.md) %}

## Success response

> 200 OK
```json
{"result":true}
```