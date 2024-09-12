# Add Section to Storage entity.section.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

The method `entity.section.add` adds a section to the storage. The user must have at least write access (**W**) to the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Required. String identifier of the storage. ||
|| **NAME**^*^
[`string`](../../data-types.md) | Required. Name of the section. ||
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

{% include [Parameter Notes](../../../_includes/required.md) %}

## Example

Call
```js
BX24.callMethod(
    'entity.section.add',
    {
        ENTITY: 'menu_new',
        'NAME': 'Test Section'
    }
);
```

Request
```http
https://my.bitrix24.com/rest/entity.section.add.json?ENTITY=menu_new&NAME=%D0%A2%D0%B5%D1%81%D1%82%D0%BE%D0%B2%D1%8B%D0%B9%20%D1%80%D0%B0%D0%B7%D0%B4%D0%B5%D0%BB&auth=9affe382af74d9c5caa588e28096e872
```

{% include [Example Notes](../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{"result":220}
```