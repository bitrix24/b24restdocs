# When Creating a CRM Activity onCrmActivityAdd

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- Examples are missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onCrmActivityAdd` is triggered when a CRM activity is created.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **FIELDS**
[`array`](../../../../data-types.md) | The array contains the field `ID` with the value of the created activity's identifier. || 
|#