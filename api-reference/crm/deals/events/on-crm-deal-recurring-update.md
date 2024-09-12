# On Recurring Deal Update onCrmDealRecurringUpdate

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The event `onCrmDealRecurringUpdate` is triggered when a recurring deal is updated.

#|
|| **Parameter** | **Description** ||
|| **FIELDS** | The array contains the following fields: 
- **ID** - the value of the record identifier in the recurring deal settings table; 
- **RECURRING_DEAL_ID** - the value of the identifier for the recurring deal template. ||
|#