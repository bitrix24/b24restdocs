# Managing Lead Cards: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The group of methods `crm.lead.details.configuration.*` manages the settings of the card for two views:

* "General view" — the card view for all employees
* "My view" — personal card settings for the employee

For each card view, sections can be configured, and within each section, a list of fields can be defined. For example, create a section called "Contact Information" and include the fields "Phone" and "Email." For fields that do not pertain to contact information, create a different section.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [CRM item form layout](https://helpdesk.bitrix24.com/open/25791235/)

## Current API Version

Development of the `crm.lead.details.configuration.*` methods has been discontinued. For new development, use the universal methods [crm.item.details.configuration.*](../../universal/item-details-configuration/index.md) and pass `entityTypeId: 1` to them — this is the identifier of the "Lead" object type. The `entityTypeId` parameter allows a single group of methods to configure the cards of any CRM object.

#|
|| **Method with Discontinued Development** | **Replacement** ||
|| `crm.lead.details.configuration.get` | [crm.item.details.configuration.get](../../universal/item-details-configuration/crm-item-details-configuration-get.md) ||
|| `crm.lead.details.configuration.set` | [crm.item.details.configuration.set](../../universal/item-details-configuration/crm-item-details-configuration-set.md) ||
|| `crm.lead.details.configuration.reset` | [crm.item.details.configuration.reset](../../universal/item-details-configuration/crm-item-details-configuration-reset.md) ||
|| `crm.lead.details.configuration.forceCommonScopeForAll` | [crm.item.details.configuration.forceCommonScopeForAll](../../universal/item-details-configuration/crm-item-details-configuration-forceCommonScopeForAll.md) ||
|#

The `crm.lead.details.configuration.*` methods continue to work — retain them only in existing integrations.

## How to Configure the Card

1. Retrieve the identifiers of the fields you want to display in the card with the [crm.lead.fields](../crm-lead-fields.md) method. The card configurations use the same names as the response of this method: `TITLE`, `STATUS_ID`, `PHONE`, and others.
2. Review the current structure of the card with the [crm.lead.details.configuration.get](./crm-lead-details-configuration-get.md) method — it returns a list of sections with nested fields.
3. Pass the new structure with the [crm.lead.details.configuration.set](./crm-lead-details-configuration-set.md) method. List the sections in the `data` parameter, and the fields of each section in its `elements`.
4. If the result does not suit you, return to the default configurations with the [crm.lead.details.configuration.reset](./crm-lead-details-configuration-reset.md) method.
5. To force the general view of the card onto all employees and delete their personal configurations, call [crm.lead.details.configuration.forceCommonScopeForAll](./crm-lead-details-configuration-force-common-scope-for-all.md).

## Configuration Scope

The configuration scope is set by the `scope` parameter:

#|
|| **Value** | **What It Configures** | **What Else to Pass** ||
|| `P` | Personal card view of an employee. Default value | `userId` — the employee identifier. If not passed, the methods work with the configurations of the user calling the method ||
|| `C` | General card view for all employees | — ||
|#

The scope also determines the access permissions: any user can read and change their own personal configurations, while the general and other users' configurations require the "Allow to modify settings" permission in CRM. This is a single role permission for the entire CRM module — it cannot be granted for leads separately.

## Linking Lead Cards with Other Objects

**User.** The user identifier `userId` is used when setting personal card settings. The user identifier can be obtained using the method [user.get](../../../user/user-get.md).

**Lead Fields.** The field identifiers are used when setting visible fields in the card section. The identifiers for system and custom lead fields can be obtained using the method [crm.lead.fields](../crm-lead-fields.md).

## Configuring Cards for Simple and Repeat Leads

Leads can be of two types: simple and repeat. A lead becomes repeat if the "Client" field is filled. Repeat leads do not have contact fields such as "Phone," "Email," or "Address" — this information is stored in the contact or company linked through the "Client" field.

Configurations for simple and repeat lead cards are retained separately. The lead type is selected by the `leadCustomerType` parameter inside the `extras` object:

#|
|| **Value** | **Which Lead Card Is Configured** ||
|| `1` | Simple lead ||
|| `2` | Repeat lead ||
|#

If `extras` is not passed, the methods work with the simple lead card.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.lead.details.configuration.set](./crm-lead-details-configuration-set.md) | Sets lead card settings ||
|| [crm.lead.details.configuration.get](./crm-lead-details-configuration-get.md) | Retrieves lead card settings ||
|| [crm.lead.details.configuration.reset](./crm-lead-details-configuration-reset.md) | Resets lead card settings ||
|| [crm.lead.details.configuration.forceCommonScopeForAll](./crm-lead-details-configuration-force-common-scope-for-all.md) | Forces a common lead card for all users ||
|#