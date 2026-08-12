# Sections in the CRM Detail Form: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The sections in the CRM detail form display information about the entity. For example, in a deal detail form, there may be sections about the client, order, and delivery terms.

The methods `crm.item.details.configuration.*` manage the sections of the detail form. You can save user-specific settings or set a uniform view for the entire company.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [CRM Detail Form: Features and Settings](https://helpdesk.bitrix24.com/open/22879716/)

## How to Work with Form Settings

1. Define the CRM object type using `entityTypeId`. You can find the identifiers in the sections [System CRM Types](../../index.md) and [Smart Processes](../user-defined-object-types/index.md).
2. For deals and smart processes, specify the funnel in the `extras` parameter (`dealCategoryId` or `categoryId`). You can retrieve the list of funnels using the method [crm.category.list](../category/crm-category-list.md).
3. Obtain the current configuration using the method [crm.item.details.configuration.get](./crm-item-details-configuration-get.md).
4. Save changes using the method [crm.item.details.configuration.set](./crm-item-details-configuration-set.md) or reset the settings to default values via [crm.item.details.configuration.reset](./crm-item-details-configuration-reset.md).
5. To make the common configuration mandatory for all users, use [crm.item.details.configuration.forceCommonScopeForAll](./crm-item-details-configuration-forceCommonScopeForAll.md).

## Scope of Settings

The `scope` parameter defines whose settings will be changed.

#|
|| **Scope Code** | **Purpose** ||
|| `P` | Personal configuration for a specific user. Used by default ||
|| `C` | Common configuration for all Bitrix24 users ||
|#

To work with personal settings, standard user permissions are sufficient. Changing common or other users' settings requires administrator rights.

## The extras Parameter by Object

In addition to `entityTypeId`, the detail form is specified by the `extras` parameter. Its composition depends on the CRM object.

#|
|| **CRM Object** | **Key in `extras`** | **Meaning** ||
|| Lead | `leadCustomerType` | Type of leads: `1` — simple leads, `2` — repeat leads ||
|| Deal | `dealCategoryId` | Identifier of the deal funnel ||
|| Smart Process | `categoryId` | Identifier of the smart process funnel ||
|| Other objects | — | `extras` is not passed ||
|#

The funnel identifier is returned by the [crm.category.list](../category/crm-category-list.md) method. If `extras` is not passed, the methods work with the default funnel.

## Relationship with Other Objects

**CRM object Type.** The settings of the detail form apply to a specific entity type through the `entityTypeId` parameter. For system objects, use identifiers from the section [System CRM Types](../../index.md), and for smart processes, refer to the section [Smart Processes](../user-defined-object-types/index.md).

**Funnels.** For deals and smart processes, the view of the detail form may differ across different funnels. Funnels are handled by the [crm.category.*](../category/index.md) methods, and in the detail form methods the funnel is passed in the `extras` parameter.

**User.** Personal settings of the detail form are tied to the user through the `userId` parameter when `scope = 'P'`. If `userId` is not provided, the methods use the user executing the request. You can obtain the user identifier using the method [user.get](../../../user/user-get.md).

## Common Causes of Errors

- An unsupported CRM object type has been specified.
- Incorrect `extras` parameters have been provided for the selected type.
- Insufficient permissions to modify the common configuration.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method and parameters

### Configuration Management

#|
|| **Method** | **Description** ||
|| [crm.item.details.configuration.get](./crm-item-details-configuration-get.md) | Returns the current settings of the detail form sections ||
|| [crm.item.details.configuration.set](./crm-item-details-configuration-set.md) | Saves changes to the detail form section settings ||
|| [crm.item.details.configuration.reset](./crm-item-details-configuration-reset.md) | Resets settings to default values ||
|| [crm.item.details.configuration.forceCommonScopeForAll](./crm-item-details-configuration-forceCommonScopeForAll.md) | Makes the common configuration mandatory for all users ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../user-defined-object-types/index.md)
- [{#T}](../category/index.md)