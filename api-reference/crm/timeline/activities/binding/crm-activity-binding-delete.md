# Delete the binding of the CRM activity

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- required parameters are not specified
- examples are missing
- no response in case of an error
- links to pages that have not yet been created (reference of available types) are not provided

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.binding.delete` removes the binding. On success, the method will return `true`.

The deletion of the binding is only possible for entities that the current user has edit access to.

If the activity is bound to only one entity, this binding cannot be removed.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **activityId**
[`number`](../../../../data-types.md) | identifier of the activity ||
|| **entityTypeId**
[`number`](../../../../data-types.md) | identifier of the entity type ([Reference of available types](.)) ||
|| **entityId**
[`number`](../../../../data-types.md) | identifier of the entity ||
|#

## Examples

```http
crm.activity.binding.delete?activityId=1&entityTypeId=4&entityId=1000
```

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

## Response on success

> 200 OK
```json
{
    "result": true
}
```