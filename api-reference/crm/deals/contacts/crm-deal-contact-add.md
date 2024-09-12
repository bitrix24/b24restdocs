# Add Contact to Deal crm.deal.contact.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

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

The method `crm.deal.contact.add` adds a contact to the specified deal.

#|
|| **Parameter** | **Description** ||
|| **id** | Identifier of the deal. ||
|| **fields** | An object with the following [fields](./crm-deal-contact-fields.md): 
- **CONTACT_ID** - identifier of the contact (required field) 
- **SORT** - sorting index 
- **IS_PRIMARY** - primary contact flag 

{% note info %}

To find out the required format for the fields, execute the method [crm.deal.contact.fields](./crm-deal-contact-fields.md) and check the format of the incoming values for these fields. 

{% endnote %}

||
|#