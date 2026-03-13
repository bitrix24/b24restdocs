# Managing Deal Cards: Overview of Methods

The group of methods `crm.deal.details.configuration.*` manages the settings for the card in two views:

* "General view" — the card view for all employees
* "My view" — personal card settings for the employee

For each card view, sections can be configured, and within each section, a list of fields can be defined. For example, you can create a section called "Payment Information" and include fields for "Amount and Currency" and "Advance." For fields that do not pertain to payment information, another section can be created.

> Quick navigation: [all methods](#all-methods) 
> 
> User documentation: [CRM Views](https://helpdesk.bitrix24.com/open/17914816/)

## Linking Deal Cards with Other Objects

**User.** The user identifier `userId` is used when setting personal card settings. You can obtain the user identifier using the method [user.get](../../../user/user-get.md).

**Deal Fields.** The identifiers of the fields are used when setting visible fields in the card section. You can obtain the identifiers of system and custom deal fields using the method [crm.deal.fields](../crm-deal-fields.md).

**Sales Funnels.** Different sales funnels can have their own card view settings. For the "Sales" funnel, create a card view with specific sections and fields. For the "Service" funnel, create a different set.

To switch between card settings for different funnels, use the parameter `dealCategoryId` — the ID of the deal funnel.

To obtain the IDs of deal funnels, use the method [crm.category.list](../../universal/category/crm-category-list.md) with the parameter `entityTypeId: 2`.

## Universal Methods for Managing Cards

Alongside the methods `crm.deal.details.configuration.*` for configuring deal cards, you can use the group of universal methods [crm.item.details.configuration.*](../../universal/item-details-configuration/index.md). The capabilities of these methods are the same, but the execution time may vary.

Universal methods [crm.item.details.configuration.*](../../universal/item-details-configuration/index.md) have an additional parameter `entityTypeId` — the ID of the object type. The `entityTypeId` parameter allows you to apply universal methods to any CRM object. To manage a deal card through a universal method, pass `entityTypeId: 2`.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.deal.details.configuration.get](./crm-deal-details-configuration-get.md) | Retrieves the settings for the deal card ||
|| [crm.deal.details.configuration.set](./crm-deal-details-configuration-set.md) | Sets the settings for the deal card ||
|| [crm.deal.details.configuration.reset](./crm-deal-details-configuration-reset.md) | Resets the settings for the deal card ||
|| [crm.deal.details.configuration.forceCommonScopeForAll](./crm-deal-details-configuration-force-common-scope-for-all.md) | Forcefully sets a common deal card for all users ||
|#