# When Updating a Company onCrmCompanyUpdate

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onCrmCompanyUpdate` is triggered when a company is updated.

Event parameters:

#|
|| **Parameter** | **Description** ||
|| **FIELDS**
[`unknown`](../../../data-types.md) | The array contains the field `ID` with the value of the updated company's identifier. ||
|#