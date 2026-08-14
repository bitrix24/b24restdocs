# Overview of Events When Working with Custom Field Settings

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in near real-time: receiving notifications about the addition, update, or deletion of custom field settings.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to custom field settings events through:

- [outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the [event.bind](../../../../events/event-bind.md) method

An example of a handler code for an event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../../events/test-handler.md).

The case of the event name does not matter when subscribing: `event.bind` accepts both `onCrmTypeUserFieldAdd` and `ONCRMTYPEUSERFIELDADD`. In [event.get](../../../../events/event-get.md) responses and in the `event` key of the event itself, the name always arrives in uppercase.

The events of this section require the `crm` scope — they are registered by the CRM module. The `userfieldconfig.*` methods require a different set: `userfieldconfig` and the module scope from `moduleId`. An application that both subscribes to the events and reads field settings needs both scopes.

## Which Objects Trigger the Events {#objects}

The events are triggered for custom fields of three CRM objects.

#|
|| **Object** | **Custom field `ENTITY_ID`** ||
|| Smart process | `CRM_{id}`, where `id` is the `id` key from the result of the [crm.type.list](../../user-defined-object-types/crm-type-list.md) method. This is not `entityTypeId`: for a smart process with `id = 13` and `entityTypeId = 156`, the value is `CRM_13` ||
|| New invoice | `CRM_SMART_INVOICE` ||
|| Document for signing | `CRM_SMART_DOCUMENT` ||
|#

The events of this section do not arrive for custom fields of other objects and modules:

- changes to fields in Business Automation processes with `moduleId=rpa` and in inventory management documents with `moduleId=catalog` do not send events
- the events of this section do not arrive for fields of legacy invoices either: those have their own events with the codes `onCrmInvoiceUserField*`

### Custom Field Events of Other CRM Objects {#other-objects}

Leads, contacts, companies, deals, estimates, and requisites have their own custom field events:

- [{#T}](../../../leads/userfield/events/index.md)
- [{#T}](../../../contacts/userfield/events/index.md)
- [{#T}](../../../companies/userfields/events/index.md)
- [{#T}](../../../deals/user-defined-fields/events/index.md)
- [{#T}](../../../quote/user-field/events/index.md)
- [{#T}](../../../requisites/events/index.md)

For requisites, the custom field events belong to the common requisite events section together with the events of the requisite itself and of bank details.

## What the Handler Receives

The events pass the custom field identifier, the object it belongs to, and its code in the `data.FIELDS` object:

```json
{
    "event": "ONCRMTYPEUSERFIELDUPDATE",
    "data": {
        "FIELDS": {
            "ID": "6977",
            "ENTITY_ID": "CRM_13",
            "FIELD_NAME": "UF_CRM_13_1742999523"
        }
    }
}
```

The example shows only the meaningful part of the request. In addition to `event` and `data`, the handler receives three more top-level keys:

- `event_handler_id` — identifier of the event handler
- `ts` — date and time the event was sent from the queue
- `auth` — [authorization parameters](./on-crm-type-user-field-add.md#auth) and information about the account where the event occurred

The full payload with all keys is shown on the event pages, for example [onCrmTypeUserFieldAdd](./on-crm-type-user-field-add.md). To make sure the request came from Bitrix24, match `auth.application_token` and `auth.member_id` against the values of your application.

The event is delivered as a POST request, so the values inside `data.FIELDS` arrive as strings. Cast `ID` to a number if you compare it with identifiers from method responses.

The composition of `FIELDS` is the same for all four events. The field type, its settings, and the list of values are not passed in the event — retrieve them with the [userfieldconfig.get](../userfieldconfig-get.md) method by passing the received `ID` together with `moduleId = crm`. This method requires the `userfieldconfig` scope in addition to the `crm` scope the application subscribes to the event with.

After the `onCrmTypeUserFieldDelete` event, the method returns an error with an empty code and the text `You are not allowed to view custom field settings`. It returns the same error when there are not enough rights to view the field, so the error code does not let you tell a deleted field from a rights denial. The remaining errors are covered on the [userfieldconfig.get](../userfieldconfig-get.md) page.

### Which Events Arrive Together {#pairs}

A single action on a field of the list type `enumeration` sends one or two events:

#|
|| **What happened** | **Which events arrive** ||
|| A field was created together with a set of values | `onCrmTypeUserFieldAdd` and `onCrmTypeUserFieldSetEnumValues` ||
|| The field settings were changed together with the set of values | `onCrmTypeUserFieldUpdate` and `onCrmTypeUserFieldSetEnumValues` ||
|| Only the set of values was changed | `onCrmTypeUserFieldSetEnumValues` ||
|| The settings of a field of any other type were changed | `onCrmTypeUserFieldUpdate` ||
|#

These cases cannot be distinguished by the contents of `data` — all events carry the same three keys. If the handler is subscribed to both `onCrmTypeUserFieldUpdate` and `onCrmTypeUserFieldSetEnumValues`, guard against processing the same `ID` twice.

The events arrive only for actions on the field itself. When an entire smart process is deleted, its custom fields disappear as well, but `onCrmTypeUserFieldDelete` is not sent for them — track such cases with the [onCrmTypeDelete](../../events/type/on-crm-type-delete.md) event.

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onCrmTypeUserFieldAdd](./on-crm-type-user-field-add.md) | When a custom field is added manually or via the [userfieldconfig.add](../userfieldconfig-add.md) method ||
|| [onCrmTypeUserFieldUpdate](./on-crm-type-user-field-update.md) | When the settings of a custom field are changed manually or via the [userfieldconfig.update](../userfieldconfig-update.md) method ||
|| [onCrmTypeUserFieldDelete](./on-crm-type-user-field-delete.md) | When a custom field is deleted manually or via the [userfieldconfig.delete](../userfieldconfig-delete.md) method ||
|| [onCrmTypeUserFieldSetEnumValues](./on-crm-type-user-field-set-enum-values.md) | When the set of values of a list-type custom field is changed manually or via the [userfieldconfig.add](../userfieldconfig-add.md) and [userfieldconfig.update](../userfieldconfig-update.md) methods ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](../../events/type/index.md)
- [{#T}](../../events/index.md)
