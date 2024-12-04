# Delete Storage entity.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `entity.delete` method removes the storage. The user must have management rights (**X**) for the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Required. String identifier of the storage to be deleted. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'entity.delete',
        {
            'ENTITY': 'test'
        }
    );
    ```

{% endlist %}

{% include [Example Note](../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{"result":true}
```