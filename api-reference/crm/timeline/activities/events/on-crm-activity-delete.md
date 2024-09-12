# When Deleting a CRM Activity on `onCrmActivityDelete`

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- The requiredness of parameters is not specified
- Examples are missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onCrmActivityDelete` is triggered when a CRM activity is deleted.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **FIELDS**
[`array`](../../../../data-types.md) | The array contains the field `ID` with the value of the deleted activity's identifier. || 
|#