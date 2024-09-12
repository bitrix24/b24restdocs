# Add Binding for CRM Activity

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- required parameters not specified
- examples missing
- response in case of error not provided
- links to yet-to-be-created pages (directory of available types) not included

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.binding.add` adds a binding for a deal. On success, the method will return `true`.

Binding a deal is only possible to an entity that the current user has edit access to.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **activityId**
[`number`](../../../../data-types.md) | identifier of the deal ||
|| **entityTypeId**
[`number`](../../../../data-types.md) | identifier of the entity type ([Directory of available types](.)) ||
|| **entityId**
[`number`](../../../../data-types.md) | identifier of the entity ||
|#

## Examples

```http
crm.activity.binding.add?activityId=1&entityTypeId=4&entityId=1000
```

{% include [Example Notes](../../../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{
    "result": true
}
```