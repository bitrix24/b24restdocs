# Managing Deal Cards: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The group of methods `crm.deal.details.configuration.*` manages the settings for the card in two views:

* "General view" — the card view for all employees
* "My view" — personal card settings for the employee

For each card view, sections can be configured, and within each section, a list of fields can be defined. For example, you can create a section called "Payment Information" and include fields for "Amount and Currency" and "Advance." For fields that do not pertain to payment information, another section can be created.

{% note warning "" %}

Development of the `crm.deal.details.configuration.*` methods has been discontinued. For new development, use the universal methods [crm.item.details.configuration.*](../../universal/item-details-configuration/index.md) with `entityTypeId = 2`.

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [CRM item form layout](https://helpdesk.bitrix24.com/open/25791235/)

## Current API Version

A deal card is configured by the same methods as the card of any other CRM object. The group [crm.item.details.configuration.*](../../universal/item-details-configuration/index.md) differs from `crm.deal.details.configuration.*` by a single additional parameter `entityTypeId` — the identifier of the object type. The method names are the same: `get`, `set`, `reset`, and `forceCommonScopeForAll`. For a deal, pass `entityTypeId = 2`; the remaining parameters are identical.

## Which Card the Method Changes

The method works with one specific card. That card is defined by three parameters common to both groups of methods. Without them, the method addresses the personal card of the current user in the default deal funnel.

#|
|| **Parameter** | **What It Defines** | **Values** ||
|| `scope` | The card view | `P` — "My view", personal settings. `C` — "General view", settings for all employees. Default — `P` ||
|| `userId` | The owner of the personal settings | User identifier from the method [user.get](../../../user/user-get.md). Required only when you read or change the personal card of another employee ||
|| `extras.dealCategoryId` | The deal funnel | Funnel identifier from the method [crm.category.list](../../universal/category/crm-category-list.md) with `entityTypeId = 2`. If it is not specified, the default funnel is used ||
|#

## Getting Started

The following sequence is intended for new development, based on the universal methods.

1. Retrieve the identifiers of the deal fields using the method [crm.item.fields](../../universal/crm-item-fields.md) with `entityTypeId = 2` — the composition of the sections is assembled from them
2. Find out the identifier of the required funnel using the method [crm.category.list](../../universal/category/crm-category-list.md) with the parameter `entityTypeId = 2`
3. Review the current card structure using the method [crm.item.details.configuration.get](../../universal/item-details-configuration/crm-item-details-configuration-get.md) — it returns a list of sections with their fields
4. Define your own structure using the method [crm.item.details.configuration.set](../../universal/item-details-configuration/crm-item-details-configuration-set.md), passing `scope` and the composition of the sections
5. Restore the default configurations using the method [crm.item.details.configuration.reset](../../universal/item-details-configuration/crm-item-details-configuration-reset.md) if the structure is no longer needed

## Linking Deal Cards with Other Objects

**Deal Fields.** A card section is a set of deal fields, so the card structure is assembled from their identifiers. The method [crm.deal.fields](../crm-deal-fields.md) returns both system and custom fields. A new custom field created by the method [crm.deal.userfield.add](../user-defined-fields/crm-deal-userfield-add.md) can also be placed in a section — under a name with the `UF_CRM_` prefix.

**Sales Funnels.** Each funnel has its own card structure: the "Sales" funnel can have one set of sections and fields, and the "Service" funnel another. The funnel is selected with the `extras.dealCategoryId` parameter, so the configuration has to be repeated for every funnel where it is needed.

**User.** A personal card belongs to a specific employee. To read or change the personal card of another employee, pass their identifier in the `userId` parameter and retrieve it using the method [user.get](../../../user/user-get.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user can work with their own personal card. Changing the general view or the personal card of another employee requires edit permissions for the corresponding view

#|
|| **Method** | **Description** ||
|| [crm.deal.details.configuration.get](./crm-deal-details-configuration-get.md) | Retrieves the settings for the deal card ||
|| [crm.deal.details.configuration.set](./crm-deal-details-configuration-set.md) | Sets the settings for the deal card ||
|| [crm.deal.details.configuration.reset](./crm-deal-details-configuration-reset.md) | Resets the settings for the deal card ||
|| [crm.deal.details.configuration.forceCommonScopeForAll](./crm-deal-details-configuration-force-common-scope-for-all.md) | Forcefully sets a common deal card for all users ||
|#
