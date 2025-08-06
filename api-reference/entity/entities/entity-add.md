# Create Data Storage entity.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- parameter types not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `entity.add` method creates a data storage. Before creating, you can check for the existence of the storage using [entity.get](./entity-get.md).

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Required. String identifier of the storage, unique for this application (maximum length - 13 characters). ||
|| **NAME**^*^
[`string`](../../data-types.md) | Required. Name of the storage. ||
|| **ACCESS**
[`unknown`](../../data-types.md) | Description of access permissions for the storage. 
Should be in the form of an associative array, where the keys are the identifiers of access permissions, and the values are **R** (read), **W** (write), or **X** (manage). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

The Creator of the storage automatically receives **X** permission.

## Example

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'entity.add',
        {
            'ENTITY': 'dish',
            'NAME': 'Dishes',
            'ACCESS': {
                U1:'W',
                AU:'R'
            }
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}