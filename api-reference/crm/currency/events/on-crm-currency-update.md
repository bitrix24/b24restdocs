# Event onCrmCurrencyUpdate

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- the FIELDS array consists of only one field ID?
- examples are missing (there should be three examples - curl, js, php)

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onCrmCurrencyUpdate` event is triggered after the currency is updated.

#|
|| **Parameter** | **Description** ||
|| **FIELDS** | The array contains the field ID with the value of the updated currency identifier. ||
|#