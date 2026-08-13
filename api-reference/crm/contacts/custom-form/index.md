# Managing Contact Cards: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The group of methods `crm.contact.details.configuration.*` manages the settings of the [contact](../index.md) card for two views:

* "General view" — the card view for all employees
* "My view" — personal card settings for the employee

For each card view, sections can be configured, and within each section, a list of fields can be defined. For example, create a section "Contact Information" and display the fields "Phone" and "Email" within it. For fields that do not relate to contact information, create a different section.

{% note warning "" %}

Development of the `crm.contact.details.configuration.*` methods has been discontinued. For new development, use the universal methods [crm.item.details.configuration.*](../../universal/item-details-configuration/index.md) with `entityTypeId = 3`.

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [CRM Views](https://helpdesk.bitrix24.com/open/23102786/)

## Current API Version

A contact card is a special case of a CRM object card, so it is managed by the universal methods [crm.item.details.configuration.*](../../universal/item-details-configuration/index.md). They have an additional parameter `entityTypeId` — the identifier of the object type. For the contact card, pass `entityTypeId = 3`. The `crm.contact.details.configuration.*` methods continue to work — retain them only in existing integrations.

#|
|| **Method with Discontinued Development** | **Replacement** ||
|| `crm.contact.details.configuration.get` | [crm.item.details.configuration.get](../../universal/item-details-configuration/crm-item-details-configuration-get.md) ||
|| `crm.contact.details.configuration.set` | [crm.item.details.configuration.set](../../universal/item-details-configuration/crm-item-details-configuration-set.md) ||
|| `crm.contact.details.configuration.reset` | [crm.item.details.configuration.reset](../../universal/item-details-configuration/crm-item-details-configuration-reset.md) ||
|| `crm.contact.details.configuration.forceCommonScopeForAll` | [crm.item.details.configuration.forceCommonScopeForAll](../../universal/item-details-configuration/crm-item-details-configuration-forceCommonScopeForAll.md) ||
|#

## Which Card View the Method Configures

The card view is selected by the `scope` parameter, and the specific employee by the `userId` parameter. By default, `scope` equals `P`, so the methods work with the personal settings of the calling user.

#|
|| **Which View You Work With** | **What to Pass** | **Who Can Read** | **Who Can Change** ||
|| "General view" — the card for all employees | `scope = C` | Any user | CRM administrator ||
|| "My view" — your own personal card | `scope = P` without `userId` | Any user | Any user ||
|| "My view" of another employee | `scope = P` and the `userId` of that employee | CRM administrator | CRM administrator ||
|#

A CRM administrator is a user with the "Allow to change settings" access permission. This is a general permission for the entire CRM module; it is not granted for contacts separately.

## How to Get Started

1. Retrieve the identifiers of the contact fields with the [crm.contact.fields](../crm-contact-fields.md) method — you will list them in the card sections.
2. Read the current configuration with the [crm.contact.details.configuration.get](./crm-contact-details-configuration-get.md) method to see the ready structure of sections instead of assembling it from scratch.
3. Write the new configuration with the [crm.contact.details.configuration.set](./crm-contact-details-configuration-set.md) method: the sections are passed as an array, and each section has its own set of fields.
4. Undo the changes with the [crm.contact.details.configuration.reset](./crm-contact-details-configuration-reset.md) method if you need to return the card to the default settings.
5. Apply the general view to all employees with the [crm.contact.details.configuration.forceCommonScopeForAll](./crm-contact-details-configuration-force-common-scope-for-all.md) method — it deletes the personal settings of the users.

## Connection of Contact Cards with Other Objects

**User.** The user identifier `userId` is used when working with the personal card settings. The user identifier can be obtained using the method [user.get](../../../user/user-get.md).

**Contact Fields.** Field identifiers are used when setting visible fields in the card section. The identifiers for system and custom contact fields can be obtained using the method [crm.contact.fields](../crm-contact-fields.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method — any user can read and change their own personal settings, any user can read the general view, while changing the general view, working with the personal settings of other employees, and the `forceCommonScopeForAll` method are available only to a CRM administrator

#|
|| **Method** | **Description** ||
|| [crm.contact.details.configuration.get](./crm-contact-details-configuration-get.md) | Returns the settings of the contact card ||
|| [crm.contact.details.configuration.set](./crm-contact-details-configuration-set.md) | Sets the settings of the contact card ||
|| [crm.contact.details.configuration.reset](./crm-contact-details-configuration-reset.md) | Resets the settings of the contact card ||
|| [crm.contact.details.configuration.forceCommonScopeForAll](./crm-contact-details-configuration-force-common-scope-for-all.md) | Forcefully sets a common contact card for all users ||
|#