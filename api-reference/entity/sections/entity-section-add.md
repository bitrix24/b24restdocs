# Add Storage Section entity.section.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `entity.section.add` adds a storage section. The user must have at least write access (**W**) to the storage.

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
[`unknown`](../../data-types.md) | Flag indicating the section's activity (Y\|N). ||
|| **SORT**
[`unknown`](../../data-types.md) | Sorting parameter of the section. ||
|| **PICTURE**
[`unknown`](../../data-types.md) | Picture of the section. ||
|| **DETAIL_PICTURE**
[`unknown`](../../data-types.md) | Detailed picture of the section. ||
|| **SECTION**
[`unknown`](../../data-types.md) | Identifier of the parent section. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'entity.section.add',
        {
            ENTITY: 'menu_new',
            'NAME': 'Test Section'
        }
    );
    ```

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.section.add.json?ENTITY=menu_new&NAME=Test%20Section&auth=9affe382af74d9c5caa588e28096e872
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{"result":220}
```