# Remove Binding of CRM Activity

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- required parameters not specified
- examples missing
- response in case of error not provided
- links to pages not yet created (directory of available types) not included

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.binding.list` retrieves a list of bindings.

The method will return an array, where each element will be an array containing:
- `entityTypeId` - identifier of the entity type ([Directory of available types](.));
- `entityId` - identifier of the entity.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **activityId**
[`number`](../../../../data-types.md) | identifier of the activity ||
|| **START** | The ordinal number of the list element from which to return the next elements when calling the current method. Details in the article [{#T}](../../../../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Examples

```http
crm.activity.binding.list?activityId=1
```

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

## Response on Success

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