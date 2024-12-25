# Managing Contact Cards: Overview of Methods

The group of methods `crm.contact.details.configuration.*` manages the settings of the card for two views:

* "General view" — the card view for all employees
* "My view" — personal card settings for the employee

For each card view, sections can be configured, and within each section, a list of fields can be defined. For example, create a section "Contact Information" and display the fields "Phone" and "Email" within it. For fields that do not relate to contact information, create a different section.

> Quick navigation: [all methods](#all-methods) 
> 
> User documentation: [CRM Views](https://helpdesk.bitrix24.com/open/23102786/)

## Connection of Contact Cards with Other Objects

**User.** The user identifier `userId` is used when setting personal card settings. The user identifier can be obtained using the method [user.get](../../../user/user-get.md).

**Contact Fields.** Field identifiers are used when setting visible fields in the card section. The identifiers for system and custom contact fields can be obtained using the method [crm.contact.fields](../crm-contact-fields.md).

## Universal Methods for Managing Cards

Alongside the methods `crm.contact.details.configuration.*` for configuring the contact card, the group of universal methods [crm.item.details.configuration.*](../../universal/item-details-configuration/index.md) can also be used. The capabilities of the methods are the same; the difference may lie in the execution time of the request.

The methods [crm.item.details.configuration.*](../../universal/item-details-configuration/index.md) have an additional parameter `entityTypeId` — the ID of the object type. The `entityTypeId` parameter allows the universal methods to be applied to any CRM object. To manage the contact card through the universal method, pass `entityTypeId` = `3`. 

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [crm.contact.details.configuration.get](./crm-contact-details-configuration-get.md) | Retrieves the parameters of the contact card  ||
|| [crm.contact.details.configuration.set](./crm-contact-details-configuration-set.md) | Sets the parameters of the individual card  ||
|| [crm.contact.details.configuration.forceCommonScopeForAll](./crm-contact-details-configuration-force-common-scope-for-all.md) | Sets a common card for all users  ||
|| [crm.contact.details.configuration.reset](./crm-contact-details-configuration-reset.md) | Resets the card parameters  ||
|#