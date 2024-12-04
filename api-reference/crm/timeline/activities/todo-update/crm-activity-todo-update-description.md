# Change Description of crm.activity.todo.updateDescription

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- revisions needed for writing standards
- required parameters are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.todo.updateDescription` changes the text of a universal activity. The result will contain `id` – the identifier of the modified activity.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ownerTypeId**
[`number`](../../../../data-types.md) | Identifier of the element type (directory of available types) to which the activity belongs ||
|| **ownerId**
[`number`](../../../../data-types.md) | Identifier of the element to which the activity belongs ||
|| **id**
[`number`](../../../../data-types.md) | Identifier of the activity ||
|| **value**
[`string`](../../../../data-types.md) | New text of the activity ||
|#

## Examples

{% list tabs %}

- HTTP

    ```http
    crm.activity.todo.updateDescription?ownerTypeId=2&ownerId=1&id=1&value=call back
    ```
{% endlist %}

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

## Response in case of success

> 200 OK
```json
{
    "result": {
        "id": 243
    }
}
```