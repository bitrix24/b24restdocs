# Set a set of contacts associated with the specified company crm.company.contact.items.set

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.contact.items.set` sets a set of contacts associated with the specified company.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Company identifier. ||
|| **items**
[`unknown`](../../../data-types.md) | A set of contacts in the form of an array of objects with the following fields: 
- **CONTACT_ID** - contact identifier (required field)
- **SORT** - sorting index
- **IS_PRIMARY** - primary contact flag || 
|#