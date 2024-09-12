# On Adding a Custom Field onCrmDealUserFieldAdd

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onCrmDealUserFieldAdd` is triggered when a custom field is added.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** | Identifier of the custom field. ||
|| **entityId** | Symbolic identifier of the entity for which the field was created. ||
|| **fieldName** | Name of the created custom field. ||
|#