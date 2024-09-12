# When Updating a CRM Activity on `onCrmActivityUpdate`

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- Required parameters are not specified
- Examples are missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onCrmActivityUpdate` is triggered when a CRM activity is updated.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **FIELDS**
[`array`](../../../../data-types.md) | The array contains the field `ID` with the value of the updated activity's identifier. || 
|#