# When Deleting a Custom Field onCrmCompanyUserFieldDelete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onCrmCompanyUserFieldDelete` is triggered when a custom field is deleted.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../../data-types.md) | identifier of the custom field. ||
|| **entityId**
[`unknown`](../../../../data-types.md) | symbolic identifier of the entity for which the field was created. ||
|| **fieldName**
[`unknown`](../../../../data-types.md) | name of the created custom field. ||
|#