# Change Parameters of entity.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `entity.update` method updates the parameters of the data storage. The user must have management rights (**X**) for the storage. The user cannot revoke their own management rights.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Required. String identifier of the storage being updated. ||
|| **NAME**
[`string`](../../data-types.md) | New name of the storage. ||
|| **ACCESS**
[`unknown`](../../data-types.md) | Description of the new set of access permissions for the storage. 
It should be in the form of an associative array, where the keys are the access permission identifiers, and the values are **R** (read), **W** (write), or **X** (manage). ||
|| **ENTITY_NEW**
[`string`](../../data-types.md) | New string identifier of the storage. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'entity.update',
        {
            'ENTITY': 'dish',
            'ACCESS': {
                U1:'W',
                AU:'R'
            }
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}