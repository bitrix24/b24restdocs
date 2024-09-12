# On Changing the Set of Values for the Custom List Field onCrmDealUserFieldSetEnumValues

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

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

The event `onCrmDealUserFieldSetEnumValues` is triggered when the set of values for a custom list field is changed.

#|
|| **Parameter** | **Description** ||
|| **id** | Identifier of the custom field. ||
|| **entityId** | Symbolic identifier of the entity for which the field was created. ||
|| **fieldName** | Name of the created custom field. ||
|#