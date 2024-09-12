# Delete the entity.section.delete storage section

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

The method `entity.section.delete` removes a storage section. The user must have at least write access (**W**) to the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../data-types.md) | Required. String identifier of the storage. ||
|| **ID**^*^
[`integer`](../../data-types.md) | Required. Identifier of the section to be deleted. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Example

Call
```javascript
BX24.callMethod(
    'entity.section.delete',
    {
        ENTITY: 'menu_new',
        ID: 220
    }
);
```

Request
```http
https://my.bitrix24.com/rest/entity.section.delete.json?ENTITY=menu_new&ID=220&auth=9affe382af74d9c5caa588e28096e872
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Response on success

> 200 OK
```json
{"result":true}
```