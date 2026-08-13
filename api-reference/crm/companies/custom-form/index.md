# Managing Company Cards: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The group of methods `crm.company.details.configuration.*` manages the settings of the card for two views:

- "General view" — the card view for all employees
- "My view" — personal card settings for the employee

In the view, you can configure the sections of the card, for example, create a section "Contact Information." Within the section, you set the list of fields: in "Contact Information" you display the fields "Phone" and "Email," and the remaining fields go to other sections.

General information about companies and the other groups of methods is in the section [Companies in CRM](../index.md).

{% note warning "Method Development Has Been Discontinued" %}

Development of the `crm.company.details.configuration.*` methods has been discontinued. For new development, use the universal methods `crm.item.details.configuration.*` — the replacement table is in the section [Current API Version](#actual-version).

{% endnote %}

> Quick navigation: [all methods](#all-methods)

## Current API Version {#actual-version}

The methods for the company card settings have been replaced by the [universal card settings methods](../../universal/item-details-configuration/index.md). A universal method works with the card of any CRM object and receives the object type in the `entityTypeId` parameter. For a company, `entityTypeId` equals `4`.

#|
|| **Method with Discontinued Development** | **Replacement** ||
|| `crm.company.details.configuration.get` | [crm.item.details.configuration.get](../../universal/item-details-configuration/crm-item-details-configuration-get.md) ||
|| `crm.company.details.configuration.set` | [crm.item.details.configuration.set](../../universal/item-details-configuration/crm-item-details-configuration-set.md) ||
|| `crm.company.details.configuration.reset` | [crm.item.details.configuration.reset](../../universal/item-details-configuration/crm-item-details-configuration-reset.md) ||
|| `crm.company.details.configuration.forceCommonScopeForAll` | [crm.item.details.configuration.forceCommonScopeForAll](../../universal/item-details-configuration/crm-item-details-configuration-forceCommonScopeForAll.md) ||
|#

The discontinued methods keep working — you do not have to rewrite existing integrations.

## Who Can Change the Card Settings

The permissions depend on whose settings you read or change:

- Any user retrieves the general settings and retrieves, sets, and resets their own personal settings.
- Only a user with the "Allow to change settings" access permission in CRM can change and reset the general settings, as well as read and change the personal settings of other users. This permission applies to the entire CRM, not to an individual object.
- The method `crm.company.details.configuration.forceCommonScopeForAll` is available only to a user with the "Allow to change settings" access permission in CRM.

## Connection of Company Cards with Other Objects

**User.** The user identifier `userId` is used when setting personal card settings. You can obtain the user identifier using the method [user.get](../../../user/user-get.md).

**Company Fields.** The identifiers of the fields are used when setting visible fields in the card section. You can obtain the identifiers of system and custom company fields using the method [crm.company.fields](../crm-company-fields.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#| 
|| **Method** | **Description** ||
|| [crm.company.details.configuration.get](./crm-company-details-configuration-get.md) | Retrieves the settings of company cards ||
|| [crm.company.details.configuration.reset](./crm-company-details-configuration-reset.md) | Resets the settings of company cards ||
|| [crm.company.details.configuration.set](./crm-company-details-configuration-set.md) | Sets the settings of company cards ||
|| [crm.company.details.configuration.forceCommonScopeForAll](./crm-company-details-configuration-force-common-scope-for-all.md) | Allows forcing the common company card for all users || 
|#
