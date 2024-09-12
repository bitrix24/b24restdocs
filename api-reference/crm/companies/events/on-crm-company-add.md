# When Creating a Company onCrmCompanyAdd

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onCrmCompanyAdd` event is triggered when a company is created.

Event parameters:

#|
|| **Parameter** | **Description** ||
|| **FIELDS**
[`unknown`](../../../data-types.md) | The array contains the `ID` field with the value of the created company's identifier. ||
|#