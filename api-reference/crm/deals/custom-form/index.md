# Managing Deal Cards: Overview of Methods

The group of methods `crm.deal.details.configuration.*` manages the settings of the card for two views:

* "General view" — the card view for all employees
* "My view" — personal card settings for the employee

For each card view, sections can be configured, and within each section, a list of fields can be defined. For example, create a section called "Payment Information" and include the fields "Amount and Currency" and "Advance" within it. For fields that do not pertain to payment information, create a different section.

> Quick navigation: [all methods](#all-methods) 

## Linking Deal Cards with Other Objects

**User.** The user identifier `userId` is used when setting personal card settings. The user identifier can be obtained using the method [user.get](../../../user/user-get.md).

**Deal Fields.** The identifiers of the fields are used when setting visible fields in the card section. The identifiers for system and custom deal fields can be obtained using the method [crm.deal.fields](../crm-deal-fields.md). 

**Deal Funnels.** Different deal funnels can have their own card views configured. For the "Sales" funnel, create a card view with specific sections and fields. For the "Service" funnel — with different ones.

To switch between card settings for different funnels, use the parameter `dealCategoryId` — the ID of the deal funnel.

To obtain the IDs of deal funnels, use the method [crm.category.list](../../universal/category/crm-category-list.md) with the parameter `entityTypeId: 2`. 

## Universal Methods for Managing Cards

Alongside the methods `crm.deal.details.configuration.*` for configuring deal cards, the group of universal methods [crm.item.details.configuration.*](../../universal/item-details-configuration/index.md) can also be used. The capabilities of these methods are the same, but the execution time may differ.

Universal methods [crm.item.details.configuration.*](../../universal/item-details-configuration/index.md) have an additional parameter `entityTypeId` — the ID of the object type. The `entityTypeId` parameter allows universal methods to be applied to any CRM object. To manage a deal card through a universal method, pass `entityTypeId: 2`. 

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [crm.deal.details.configuration.get](./crm-deal-details-configuration-get.md) | Retrieves the settings of deal cards ||
|| [crm.deal.details.configuration.reset](./crm-deal-details-configuration-reset.md) | Resets the settings of deal cards ||
|| [crm.deal.details.configuration.set](./crm-deal-details-configuration-set.md) | Allows setting the settings of deal cards ||
|| [crm.deal.details.configuration.forceCommonScopeForAll](./crm-deal-details-configuration-force-common-scope-for-all.md) | Forcefully sets a common deal card for all users ||
|#