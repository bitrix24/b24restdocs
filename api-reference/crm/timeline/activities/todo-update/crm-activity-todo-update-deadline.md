# Change Deadline crm.activity.todo.updateDeadline

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- revisions needed for writing standards
- required parameters not specified
- examples missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.todo.updateDeadline` changes the deadline of a universal deal. The result will contain `id` - the identifier of the modified deal.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ownerTypeId**
[`number`](../../../../data-types.md) | Identifier of the element type (reference of available types) to which the deal belongs. ||
|| **ownerId**
[`number`](../../../../data-types.md) | Identifier of the element to which the deal belongs. ||
|| **id**
[`number`](../../../../data-types.md) | Identifier of the deal. ||
|| **value**
[`string`](../../../../data-types.md) | New deadline for the deal. ||
|#

## Examples

```http
crm.activity.todo.updateDeadline?ownerTypeId=2&ownerId=1&id=1&value=2022-12-12T15:00:00
```

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

## Response on success

```json
{
    "result": {
        "id": 243
    }
}
```