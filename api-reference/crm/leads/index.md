# Leads in CRM: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A lead is the starting point of the Sales Funnel. Its card contains information about the client's interest in a product or service: filled CRM forms, e-mails, calls, and chats with the client.

The main goal of working with leads is to determine their potential and convert them into deals for further selling of the product or service.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [leads in Bitrix24](https://helpdesk.bitrix24.com/open/19360846/)

## Current API Version

Development of the `crm.lead.*` and `crm.lead.details.configuration.*` methods has been discontinued. For new development, use the universal methods [crm.item.*](../universal/index.md) and pass `entityTypeId: 1` to them — this is the identifier of the "Lead" object type. The `crm.lead.*` methods continue to work — retain them only in existing integrations.

#|
|| **Method with Discontinued Development** | **Replacement** ||
|| `crm.lead.add` | [crm.item.add](../universal/crm-item-add.md) ||
|| `crm.lead.update` | [crm.item.update](../universal/crm-item-update.md) ||
|| `crm.lead.get` | [crm.item.get](../universal/crm-item-get.md) ||
|| `crm.lead.list` | [crm.item.list](../universal/crm-item-list.md) ||
|| `crm.lead.delete` | [crm.item.delete](../universal/crm-item-delete.md) ||
|| `crm.lead.fields` | [crm.item.fields](../universal/crm-item-fields.md) ||
|| `crm.lead.productrows.*` | [crm.item.productrow.*](../universal/product-rows/index.md) ||
|| `crm.lead.details.configuration.*` | [crm.item.details.configuration.*](../universal/item-details-configuration/index.md) ||
|#

The [crm.lead.contact.*](./management-communication/index.md) and [crm.lead.userfield.*](./userfield/index.md) method groups have no replacement — they are current.

Field names differ between the two method groups: `crm.lead.*` accepts and returns fields in `UPPER_CASE` format, for example `STATUS_ID`, while `crm.item.*` uses `camelCase` format, for example `statusId`.

## How to Get Started

1. Retrieve the list of lead fields with the [crm.lead.fields](./crm-lead-fields.md) method. It returns system and custom fields with their types and names.
2. Create a lead with the [crm.lead.add](./crm-lead-add.md) method. A lead has no required fields, but without a filled `TITLE` it will be hard to find in the list.
3. Add product items to the lead with the [crm.lead.productrows.set](./crm-lead-productrows-set.md) method if the client's request concerns specific products.
4. Link the lead to contacts with the [crm.lead.contact.*](./management-communication/index.md) methods if the client is already in the database.
5. Move the lead through the stages with the [crm.lead.update](./crm-lead-update.md) method by changing the `STATUS_ID` field. The list of stages is returned by the [crm.status.list](../status/crm-status-list.md) method with the `filter[ENTITY_ID]=STATUS` filter.
6. Subscribe to [lead events](./events/index.md) to receive notifications about changes in real time.

## Connection of Leads with Other CRM Objects

**Products.** The product items of a lead are set by the [crm.lead.productrows.set](./crm-lead-productrows-set.md) method and returned by [crm.lead.productrows.get](./crm-lead-productrows-get.md). The universal replacement for these methods is the [crm.item.productrow.*](../universal/product-rows/index.md) group with the `ownerType: L` parameter.

**Deal.** The connection appears after converting the lead into a successful one.

**Client.** A field in the lead card consisting of the associated company and contacts. The field is available in the repeat lead form. If repeat leads are disabled, the linking field appears after creating a company or contact based on the lead. There is one company in the lead, and access to it is made directly through the field `COMPANY_ID`. Multiple contacts can be specified, and interaction with them is conducted through a separate group of methods [crm.lead.contact.*](./management-communication/index.md).  

{% note tip "User Documentation" %}

- [How to add products to deals, leads, and estimates](https://helpdesk.bitrix24.com/open/13323830/)
- [How to convert a lead](https://helpdesk.bitrix24.com/open/19359866/)
- [Repeat leads and deals](https://helpdesk.bitrix24.com/open/17720892/)
- [Deals in CRM: Overview of Methods](../deals/index.md)

{% endnote %}

## Lead Card

The main workspace in a lead is the General tab of its card. It consists of two parts:

* The left part contains fields with information. If the system fields are insufficient, you can create your own custom fields. They allow storing information in various data formats: string, number, link, address, and others. To create, modify, retrieve, or delete custom lead fields, the group of methods [crm.lead.userfield.*](./userfield/index.md) is used.

* The right part contains the lead timeline. In it, you can create, edit, filter, and delete CRM activities — the group of methods [crm.activity.*](../timeline/activities/index.md), and timeline records — the group of methods [crm.timeline.*](../timeline/index.md).

The composition of sections and fields of the lead card is managed by the group of methods [crm.lead.details.configuration.*](./custom-form/index.md). Configurations are set separately for the simple lead card and the repeat lead card.

{% note tip "User Documentation" %}

- [CRM Card: Features and Settings](https://helpdesk.bitrix24.com/open/22879716/)
- [System Fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM object](https://helpdesk.bitrix24.com/open/16767378/)

{% endnote %}

## Widgets

An application can be embedded into the lead card. Thanks to embedding, you can use the application without leaving the lead card.

There are two embedding scenarios:

* Use special [embedding locations](../../widgets/crm/index.md). For example, by creating your own tab.
* Create a [custom field](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md), into which the content of your application will be loaded.

{% note tip "Typical Use-Cases and Scenarios" %}

- [Widget Embedding Mechanism](../../widgets/index.md)
- [Embed a Widget in the CRM Card](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Features

**A lead may not be retained as a lead.** Bitrix24 has two CRM operating modes. In classic mode, a lead remains in the system after creation. In simple mode there are no leads: the system immediately converts a created lead into a deal, and it can no longer be retrieved with the [crm.lead.get](./crm-lead-get.md) method. Check the mode with the [crm.settings.mode.get](../crm-settings-mode-get.md) method before building a scenario around leads.

**Conversion is not available in REST.** There is no separate method for converting a lead into a contact, company, or deal. Through the API, you can only move the lead to a successful stage with the [crm.lead.update](./crm-lead-update.md) method — no new objects are created, and they have to be created with separate calls to [crm.contact.add](../contacts/crm-contact-add.md), [crm.company.add](../companies/crm-company-add.md), and [crm.deal.add](../deals/crm-deal-add.md).

**A repeat lead is determined by the system.** The repeat lead indicator `IS_RETURN_CUSTOMER` is read-only: it is set to `Y` automatically when the lead has the `CONTACT_ID` or `COMPANY_ID` field filled. `IS_RETURN_CUSTOMER` cannot be passed directly to [crm.lead.add](./crm-lead-add.md) or [crm.lead.update](./crm-lead-update.md) — the value is recalculated.

{% note tip "User Documentation" %}

- [CRM Operating Modes](https://helpdesk.bitrix24.com/open/17627868/)
- [How to Convert a Lead](https://helpdesk.bitrix24.com/open/19359866/)

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Main

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.lead.add](./crm-lead-add.md) | Creates a new lead ||
    || [crm.lead.update](./crm-lead-update.md) | Modifies a lead ||
    || [crm.lead.get](./crm-lead-get.md) | Returns a lead by ID ||
    || [crm.lead.list](./crm-lead-list.md) | Returns a list of leads by filter ||
    || [crm.lead.delete](./crm-lead-delete.md) | Deletes a lead and all associated objects ||
    || [crm.lead.productrows.set](./crm-lead-productrows-set.md) | Sets the list of lead products ||
    || [crm.lead.productrows.get](./crm-lead-productrows-get.md) | Returns the products of a lead ||
    || [crm.lead.fields](./crm-lead-fields.md) | Returns the description of lead fields ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmLeadAdd](./events/on-crm-lead-add.md) | When a lead is added manually or via the [crm.lead.add](./crm-lead-add.md) method ||
    || [onCrmLeadUpdate](./events/on-crm-lead-update.md) | When a lead is modified manually or via the [crm.lead.update](./crm-lead-update.md) method ||
    || [onCrmLeadDelete](./events/on-crm-lead-delete.md) | When a lead is deleted manually or via the [crm.lead.delete](./crm-lead-delete.md) method ||
    |#

{% endlist %}

### Connection Between Leads and Contacts

#|
|| **Method** | **Description** ||
|| [crm.lead.contact.add](./management-communication/crm-lead-contact-add.md) | Adds a contact binding to the specified lead ||
|| [crm.lead.contact.delete](./management-communication/crm-lead-contact-delete.md) | Removes a contact binding from the specified lead ||
|| [crm.lead.contact.items.get](./management-communication/crm-lead-contact-items-get.md) | Retrieves a list of contacts associated with the lead ||
|| [crm.lead.contact.items.set](./management-communication/crm-lead-contact-items-set.md) | Attaches a list of contacts to the specified lead ||
|| [crm.lead.contact.items.delete](./management-communication/crm-lead-contact-items-delete.md) | Removes the list of contacts from the lead ||
|| [crm.lead.contact.fields](./management-communication/crm-lead-contact-fields.md) | Retrieves the description of fields for lead-contact connection, used by methods of the `crm.lead.contact.*` family ||
|#

### Custom Fields

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.lead.userfield.add](./userfield/crm-lead-userfield-add.md) | Creates a new field ||
    || [crm.lead.userfield.update](./userfield/crm-lead-userfield-update.md) | Modifies a field ||
    || [crm.lead.userfield.get](./userfield/crm-lead-userfield-get.md) | Returns a field by code ||
    || [crm.lead.userfield.list](./userfield/crm-lead-userfield-list.md) | Returns a list of fields ||
    || [crm.lead.userfield.delete](./userfield/crm-lead-userfield-delete.md) | Deletes a field ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmLeadUserFieldAdd](./userfield/events/on-crm-lead-user-field-add.md) | When a custom field is added manually or via the [crm.lead.userfield.add](./userfield/crm-lead-userfield-add.md) method ||
    || [onCrmLeadUserFieldUpdate](./userfield/events/on-crm-lead-user-field-update.md) | When a custom field is modified manually or via the [crm.lead.userfield.update](./userfield/crm-lead-userfield-update.md) method ||
    || [onCrmLeadUserFieldDelete](./userfield/events/on-crm-lead-user-field-delete.md) | When a custom field is deleted manually or via the [crm.lead.userfield.delete](./userfield/crm-lead-userfield-delete.md) method ||
    || [onCrmLeadUserFieldSetEnumValues](./userfield/events/on-crm-lead-user-field-set-enum-values.md) | When the set of values for a custom list-type field is modified manually or via the [crm.lead.userfield.update](./userfield/crm-lead-userfield-update.md) method ||
    |#

{% endlist %}

### Managing Lead Cards

#|
|| **Method** | **Description** ||
|| [crm.lead.details.configuration.set](./custom-form/crm-lead-details-configuration-set.md) | Sets the settings for lead cards ||
|| [crm.lead.details.configuration.get](./custom-form/crm-lead-details-configuration-get.md) | Retrieves the settings parameters for lead cards ||
|| [crm.lead.details.configuration.reset](./custom-form/crm-lead-details-configuration-reset.md) | Resets the settings for lead cards ||
|| [crm.lead.details.configuration.forceCommonScopeForAll](./custom-form/crm-lead-details-configuration-force-common-scope-for-all.md) | Forces a common lead card for all users ||
|#