# Get the list of CRM activity bindings

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- required parameters not specified
- examples missing
- response in case of error not provided
- links to pages not yet created (reference of available types) not included

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.binding.list` retrieves a list of bindings.

The method will return an array, where each element will be an array containing:
- `entityTypeId` - the identifier of the entity type ([Reference of available types](.));
- `entityId` - the identifier of the entity.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **activityId**
[`number`](../../../../data-types.md) | identifier of the deal ||
|| **START** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../../../../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Examples

```http
crm.activity.binding.list?activityId=1
```

{% include [Example notes](../../../../../_includes/examples.md) %}

## Response on success

> 200 OK
```json
{
    "result": [
        {
            "entityTypeId": 1,
            "entityId": 123
        },
        {
            "entityTypeId": 2,
            "entityId": 456
        },
        {
            "entityTypeId": 3,
            "entityId": 789
        }
    ]
}
```