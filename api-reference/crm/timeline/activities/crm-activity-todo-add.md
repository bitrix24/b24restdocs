# Add Universal Activity crm.activity.todo.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _is not exported to prod_" %}

- revisions are needed for standard writing
- required parameters are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.todo.add` adds a universal activity. The result will contain `id` - the identifier of the created activity.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ownerTypeId**
[`number`](../../../data-types.md) | Identifier of the element type (reference of available types) to which the activity belongs ||
|| **ownerId**
[`number`](../../../data-types.md) | Identifier of the element to which the activity belongs ||
|| **description**
[`string`](../../../data-types.md) | Text of the activity ||
|| **deadline**
[`datetime`](../../../data-types.md) | Deadline of the activity ||
|| **responsibleId**
[`number`](../../../data-types.md) | Responsible person for the activity ||
|#

## Examples

{% list tabs %}

- HTTP

    ```http
    crm.activity.todo.add?ownerTypeId=2&ownerId=1&deadline=2022-12-31T15:00:00&description=Contact the client
    ```

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{
    "result": {
        "id": 243
    }
}
```