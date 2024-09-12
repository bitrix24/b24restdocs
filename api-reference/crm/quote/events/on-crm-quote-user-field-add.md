# Event for Adding a Custom Field onCrmQuoteUserFieldAdd

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onCrmQuoteUserFieldAdd` is triggered when a custom field is added.

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Identifier of the custom field. ||
|| **entityId**
[`unknown`](../../../data-types.md) | Symbolic identifier of the entity for which the field was created. ||
|| **fieldName**
[`unknown`](../../../data-types.md) | Name of the created custom field. ||
|#