# Event for Deleting a Custom Field onCrmQuoteUserFieldDelete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

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

The event `onCrmQuoteUserFieldDelete` is triggered when a custom field is deleted.

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Identifier of the custom field. ||
|| **entityId**
[`unknown`](../../../data-types.md) | Symbolic identifier of the entity to which the field belongs. ||
|| **fieldName**
[`unknown`](../../../data-types.md) | Name of the deleted custom field. ||
|#