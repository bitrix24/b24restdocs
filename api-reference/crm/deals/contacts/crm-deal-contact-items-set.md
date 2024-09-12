# Add Multiple Contacts to a Deal crm.deal.contact.items.set

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.contact.items.set` sets a collection of contacts associated with the specified deal.

#|
|| **Parameter** | **Description** ||
|| **id** | Identifier of the deal. ||
|| **items** | A collection of contacts represented as an array of objects with the following [fields](./crm-deal-contact-fields.md): 
- **CONTACT_ID** - identifier of the contact (required field) 
- **SORT** - sorting index 
- **IS_PRIMARY** - primary contact flag ||
|#