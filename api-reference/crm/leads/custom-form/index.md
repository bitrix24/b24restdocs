# Managing Lead Cards: Overview of Methods

The group of methods `crm.lead.details.configuration.*` manages the settings of the card for two views:

* "General view" — the card view for all employees
* "My view" — personal card settings for the employee

For each card view, sections can be configured, and within each section, a list of fields can be defined. For example, create a section called "Contact Information" and include the fields "Phone" and "Email." For fields that do not pertain to contact information, create a different section.

> Quick navigation: [all methods](#all-methods) 

## Linking Lead Cards with Other Objects

**User.** The user identifier `userId` is used when setting personal card settings. The user identifier can be obtained using the method [user.get](../../../user/user-get.md).

**Lead Fields.** The field identifiers are used when setting visible fields in the card section. The identifiers for system and custom lead fields can be obtained using the method [crm.lead.fields](../crm-lead-fields.md).

## Configuring Cards for Simple and Repeat Leads

Leads can be of two types: simple and repeat. A lead becomes repeat if the "Client" field is filled. Repeat leads do not have contact fields such as "Phone," "Email," or "Address" — this information is stored in the contact or company linked through the "Client" field.

For both simple and repeat lead cards, you can configure your view. Pass the parameter `leadCustomerType: 1` for a simple lead and `leadCustomerType: 2` for a repeat lead.

## Universal Methods for Managing Cards

Alongside the methods `crm.deal.details.configuration.*` for configuring lead cards, you can use the group of universal methods [crm.item.details.configuration.*](../../universal/item-details-configuration/index.md). The capabilities of the methods are the same, but the execution time may differ.

Universal methods [crm.item.details.configuration.*](../../universal/item-details-configuration/index.md) have an additional parameter `entityTypeId` — the ID of the object type. The `entityTypeId` parameter allows you to apply universal methods to any CRM object. To manage a lead card through a universal method, pass `entityTypeId: 1`. 

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [crm.lead.details.configuration.get](./crm-lead-details-configuration-get.md) | Retrieves lead card settings ||
|| [crm.lead.details.configuration.reset](./crm-lead-details-configuration-reset.md) | Resets lead card settings ||
|| [crm.lead.details.configuration.set](./crm-lead-details-configuration-set.md) | Sets lead card settings ||
|| [crm.lead.details.configuration.forceCommonScopeForAll](./crm-lead-details-configuration-force-common-scope-for-all.md) | Forces a common lead card for all users ||
|#