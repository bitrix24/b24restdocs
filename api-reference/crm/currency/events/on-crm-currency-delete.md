# Event onCrmCurrencyDelete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- the FIELDS array consists of only one field ID?
- examples are missing (there should be three examples - curl, js, php)

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onCrmCurrencyDelete` event is triggered after a currency is deleted.

#|
|| **Parameter** | **Description** ||
|| **FIELDS** | The array contains the field ID with the value of the deleted currency's identifier. || 
|#