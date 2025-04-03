# Add contact to the specified company crm.company.contact.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.contact.add` adds a contact to the specified company.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Company identifier. ||
|| **fields**
[`object`](../../../data-types.md) | Object with the following fields: 
- **CONTACT_ID** - contact identifier (required parameter) 
- **SORT** - sorting index
- **IS_PRIMARY** - primary contact flag

{% note info %}

To find out the required format of the fields, execute the method [crm.company.fields](../crm-company-fields.md) and check the format of the returned values for these fields. 

{% endnote %}

||
|#