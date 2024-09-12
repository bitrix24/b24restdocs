# Delete Storage entity.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `entity.delete` method removes a storage. The user must have management rights (**X**) for the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Required. The string identifier of the storage to be deleted. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Example

```javascript
BX24.callMethod(
    'entity.delete',
    {
        'ENTITY': 'test'
    }
);
```

{% include [Example Note](../../../_includes/examples.md) %}

## Successful Response

> 200 OK
```json
{"result":true}
```