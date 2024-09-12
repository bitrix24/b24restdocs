# Event for Deleting Estimate onCrmQuoteAdd

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

The event `onCrmQuoteAdd` is triggered when an estimate is deleted.

#|
|| **Parameter** | **Description** ||
|| **FIELDS**
[`unknown`](../../../data-types.md) | The array contains a field ID with the value of the identifier of the deleted estimate. ||
|#