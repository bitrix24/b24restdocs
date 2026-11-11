# Managing Deal Cards: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The group of methods `crm.deal.details.configuration.*` manages the settings for the card in two views:

* "General view" — the card view for all employees
* "My view" — personal card settings for the employee

For each card view, you can configure sections, each with its own list of fields. For example, you can create a "Payment Information" section with the "Amount and Currency" and "Advance" fields and move fields unrelated to payment to another section.

{% note warning "" %}

Development of the `crm.deal.details.configuration.*` methods has been discontinued. For new development, use the REST 3.0 methods [crm.deal.detailpagelayout.*](../../rest-v3/detail-page-layout/index.md).

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [CRM item form layout](https://helpdesk.bitrix24.com/open/25791235/)

## Current API Version

The `crm.deal.details.configuration.*` methods continue to work. Keep them only in existing integrations.

#|
|| **Discontinued Method** | **Replacement** ||
|| `crm.deal.details.configuration.get` | [crm.deal.detailpagelayout.get](../../rest-v3/detail-page-layout/crm-entity-detail-page-layout-get-rest-v3.md) ||
|| `crm.deal.details.configuration.set` | [crm.deal.detailpagelayout.update](../../rest-v3/detail-page-layout/crm-entity-detail-page-layout-update-rest-v3.md) ||
|| `crm.deal.details.configuration.reset` | [crm.item.details.configuration.reset](../../universal/item-details-configuration/crm-item-details-configuration-reset.md) with `entityTypeId = 2`. REST 3.0 has no reset method ||
|| `crm.deal.details.configuration.forceCommonScopeForAll` | [crm.deal.detailpagelayout.setcommonforall](../../rest-v3/detail-page-layout/crm-entity-detail-page-layout-set-common-for-all-rest-v3.md). Unlike `forceCommonScopeForAll`, it does not delete users' personal settings ||
|#

## How to Select a Card

The `get`, `set`, and `reset` methods work with a single card defined by three parameters. Without them, the call addresses the personal card of the current user in the default deal funnel. `forceCommonScopeForAll` accepts only `extras.dealCategoryId` and always works with the general view.

#|
|| **Parameter** | **What It Defines** | **Values** ||
|| `scope` | The card view | `P` — "My view". `C` — "General view". Default — `P` ||
|| `userId` | The owner of the personal settings | User identifier from the method [user.get](../../../user/user-get.md). Required only when you read or change the personal card of another employee ||
|| `extras.dealCategoryId` | The deal funnel | Funnel identifier from the method [crm.category.list](../../universal/category/crm-category-list.md) with `entityTypeId = 2` or [crm.deal.category.list](../../rest-v3/categories/crm-entity-category-list-rest-v3.md) in REST 3.0 — the identifiers are the same. If it is not specified, the default funnel is used ||
|#

## What Card Settings Consist Of

Card settings are an array of sections, each listing deal fields. The `get` method returns this array, and the `set` method accepts it in the `data` parameter. The section and field schema is described in the [crm.deal.details.configuration.set](./crm-deal-details-configuration-set.md#section) method.

## Getting Started with REST 3.0

1. Find out the funnel identifier using the method [crm.deal.category.list](../../rest-v3/categories/crm-entity-category-list-rest-v3.md). The default funnel has the identifier `0`
2. Retrieve the general view identifier using the method [crm.deal.detailpagelayout.getcommonlayoutid](../../rest-v3/detail-page-layout/crm-entity-detail-page-layout-get-common-layout-id-rest-v3.md) or the personal view identifier using the method [crm.deal.detailpagelayout.getpersonallayoutid](../../rest-v3/detail-page-layout/crm-entity-detail-page-layout-get-personal-layout-id-rest-v3.md)
3. Review the current view structure using the method [crm.deal.detailpagelayout.get](../../rest-v3/detail-page-layout/crm-entity-detail-page-layout-get-rest-v3.md) — it returns sections with their fields
4. Modify the sections and save them using the method [crm.deal.detailpagelayout.update](../../rest-v3/detail-page-layout/crm-entity-detail-page-layout-update-rest-v3.md)
5. Apply the general view to all users using the method [crm.deal.detailpagelayout.setcommonforall](../../rest-v3/detail-page-layout/crm-entity-detail-page-layout-set-common-for-all-rest-v3.md)

## Linking Deal Cards with Other Objects

**Deal Fields.** A card section is a set of deal fields, so the card structure is assembled from their identifiers. The method [crm.deal.fields](../crm-deal-fields.md) returns both system and custom fields. A new custom field created by the method [crm.deal.userfield.add](../user-defined-fields/crm-deal-userfield-add.md) can also be placed in a section — under a name with the `UF_CRM_` prefix.

**Sales Funnels.** Each funnel has its own card structure: the "Sales" funnel can have one set of sections and fields, and the "Service" funnel another. The funnel is selected with the `extras.dealCategoryId` parameter, so the configuration is repeated in every funnel where it is required.

**User.** A personal card belongs to a specific employee. To access the personal card of another employee, pass their identifier in the `userId` parameter.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
> - any user — read, change, and reset their own personal card, and read the general view
> - a user with permission to edit the general view of the deal card — change and reset the general view and apply it to all users
> - a CRM administrator — read, change, and reset the personal card of another employee

#|
|| **Method** | **Description** ||
|| [crm.deal.details.configuration.get](./crm-deal-details-configuration-get.md) | Retrieves the settings for the deal card ||
|| [crm.deal.details.configuration.set](./crm-deal-details-configuration-set.md) | Sets the settings for the deal card ||
|| [crm.deal.details.configuration.reset](./crm-deal-details-configuration-reset.md) | Resets the settings for the deal card ||
|| [crm.deal.details.configuration.forceCommonScopeForAll](./crm-deal-details-configuration-force-common-scope-for-all.md) | Applies the general view of the deal card to all users and deletes their personal settings ||
|#
