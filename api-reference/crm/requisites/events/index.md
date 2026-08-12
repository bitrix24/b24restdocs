# Overview of Events When Working with Requisites

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in near real-time. Notifications are available for the addition, modification, and deletion of requisites, bank details, and custom fields of requisites, as well as for the registration and deletion of addresses. The methods for working with these objects are collected in the [Details](../index.md) section.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to requisite events through:

- an [outgoing webhook](../../../../local-integrations/local-webhooks.md)
- an [application](../../../../settings/app-installation/index.md) and the [event.bind](../../../events/event-bind.md) method

An example of a handler code for an event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## What the Handler Receives

Requisite events do not pass the entire object data — the handler receives only the key by which the object can be requested with a method.

**Requisite and bank detail events.** The `data.FIELDS.ID` field contains the object identifier. The rest of the data is returned by the [crm.requisite.get](../universal/crm-requisite-get.md) and [crm.requisite.bankdetail.get](../bank-detail/crm-requisite-bank-detail-get.md) methods. Once the object is deleted, its data can no longer be retrieved by identifier.

**Address events.** An address has no identifier of its own, so `data.FIELDS` contains a composite key: the address type `TYPE_ID`, the parent object type `ENTITY_TYPE_ID`, and its identifier `ENTITY_ID`. The address can be found by this key with the [crm.address.list](../addresses/crm-address-list.md) method. The service fields `ANCHOR_ID` and `ANCHOR_TYPE_ID` are passed as well and contain the primary owner of the address: for a requisite address, it is a contact or a company; for a lead address, it is the lead itself.

**Custom field events.** `data.FIELDS` contains the field identifier `ID`, the symbolic object identifier `ENTITY_ID`, and the field code `FIELD_NAME`. The field description is returned by the [crm.requisite.userfield.get](../user-fields/crm-requisite-userfield-get.md) method.

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
|| [onCrmRequisiteUserFieldSetEnumValues](./on-crm-requisite-user-field-set-enum-values.md) | When the set of values for a custom field of list type is modified manually or via the [crm.requisite.userfield.update](../user-fields/crm-requisite-userfield-update.md) method ||
|#