# Overview of Methods

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

About contacts (general description, fields, highlight obsolete address types and indicate that instead, one should go to the details)

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [crm.contact.add](./crm-contact-add.md) | Creates a new contact ||
|| [crm.contact.update](./crm-contact-update.md) | Updates an existing contact ||
|| [crm.contact.get](./crm-contact-get.md) | Returns a contact by ID ||
|| [crm.contact.list](./crm-contact-list.md) | Returns a list of contacts by filter ||
|| [crm.contact.delete](./crm-contact-delete.md) | Deletes a contact and all associated objects ||
|| [crm.contact.fields](./crm-contact-fields.md) | Returns the description of contact fields, including custom fields ||
|#

## Companies

#|
|| **Method** | **Description** ||
|| [crm.contact.company.fields](./company/crm-contact-company-fields.md) | Returns the description of fields for contact-company linkage ||
|| [crm.contact.company.add](./company/crm-contact-company-add.md) | Adds a company to the specified contact ||
|| [crm.contact.company.delete](./company/crm-contact-company-delete.md) | Removes a company from the specified contact ||
|| [crm.contact.company.items.get](./company/crm-contact-company-items-get.md) | Retrieves a set of companies associated with the specified contact ||
|| [crm.contact.company.items.set](./company/crm-contact-company-items-set.md) | Sets the set of companies associated with the specified contact ||
|| [crm.contact.company.items.delete](./company/crm-contact-company-items-delete.md) | Clears the set of companies associated with the specified contact ||
|#

## Custom Fields

#|
|| **Method** | **Description** ||
|| [crm.contact.userfield.add](./userfield/crm-contact-userfield-add.md) | Creates a custom field for contacts ||
|| [crm.contact.userfield.update](./userfield/crm-contact-userfield-update.md) | Modifies an existing custom field for contacts ||
|| [crm.contact.userfield.get](./userfield/crm-contact-userfield-get.md) | Returns a custom field for contacts by ID ||
|| [crm.contact.userfield.list](./userfield/crm-contact-userfield-list.md) | Returns a list of custom fields for contacts ||
|| [crm.contact.userfield.delete](./userfield/crm-contact-userfield-delete.md) | Deletes a custom field for contacts ||
|#

## Managing Contact Cards

#|
|| **Method** | **Description** ||
|| [crm.contact.details.configuration.get](./custom-form/crm-contact-details-configuration-get.md) | Retrieves the settings for contact cards ||
|| [crm.contact.details.configuration.reset](./custom-form/crm-contact-details-configuration-reset.md) | Resets the settings for contact cards ||
|| [crm.contact.details.configuration.set](./custom-form/crm-contact-details-configuration-set.md) | Sets the settings for contact cards ||
|| [crm.contact.details.configuration.forceCommonScopeForAll](./custom-form/crm-contact-details-configuration-force-common-scope-for-all.md) | Allows forcing a common contact card for all users ||
|#