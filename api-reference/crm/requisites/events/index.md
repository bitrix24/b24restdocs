# Overview of Events When Working with Requisites

Events allow applications to respond to changes in near real-time. Notifications are available for the addition, modification, and deletion of requisites, bank details, and custom fields of requisites, as well as for the registration and deletion of addresses.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to requisite events through the [application](../../../../settings/app-installation/index.md) and the [event.bind](../../../events/event-bind.md) method.

An example of a handler code for an event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#| 
|| **Event** | **Triggered** ||
|| [onCrmRequisiteAdd](./on-crm-requisite-add.md) | When a requisite is added manually or via the [crm.requisite.add](../universal/crm-requisite-add.md) method ||
|| [onCrmRequisiteUpdate](./on-crm-requisite-update.md) | When a requisite is modified manually or via the [crm.requisite.update](../universal/crm-requisite-update.md) method ||
|| [onCrmRequisiteDelete](./on-crm-requisite-delete.md) | When a requisite is deleted manually or via the [crm.requisite.delete](../universal/crm-requisite-delete.md) method ||
|| [onCrmAddressRegister](./on-crm-address-register.md) | When an address is registered manually or via the [crm.address.add](../addresses/crm-address-add.md) method ||
|| [onCrmAddressUnregister](./on-crm-address-unregister.md) | When an address is deleted manually or via the [crm.address.delete](../addresses/crm-address-delete.md) method ||
|| [onCrmBankDetailAdd](./on-crm-bank-detail-add.md) | When a bank detail is added manually or via the [crm.requisite.bankdetail.add](../bank-detail/crm-requisite-bank-detail-add.md) method ||
|| [onCrmBankDetailUpdate](./on-crm-bank-detail-update.md) | When a bank detail is modified manually or via the [crm.requisite.bankdetail.update](../bank-detail/crm-requisite-bank-detail-update.md) method ||
|| [onCrmBankDetailDelete](./on-crm-bank-detail-delete.md) | When a bank detail is deleted manually or via the [crm.requisite.bankdetail.delete](../bank-detail/crm-requisite-bank-detail-delete.md) method ||
|| [onCrmRequisiteUserFieldAdd](./on-crm-requisite-user-field-add.md) | When a custom field of a requisite is added manually or via the [crm.requisite.userfield.add](../user-fields/crm-requisite-userfield-add.md) method ||
|| [onCrmRequisiteUserFieldUpdate](./on-crm-requisite-user-field-update.md) | When a custom field of a requisite is modified manually or via the [crm.requisite.userfield.update](../user-fields/crm-requisite-userfield-update.md) method ||
|| [onCrmRequisiteUserFieldDelete](./on-crm-requisite-user-field-delete.md) | When a custom field of a requisite is deleted manually or via the [crm.requisite.userfield.delete](../user-fields/crm-requisite-userfield-delete.md) method ||
|| [onCrmRequisiteUserFieldSetEnumValues](./on-crm-requisite-user-field-set-enum-values.md) | When the set of values for a custom field of list type is modified ||
|#