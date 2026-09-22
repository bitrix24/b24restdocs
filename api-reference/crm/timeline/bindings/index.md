# Timeline Record Bindings with CRM Entities: Overview of Methods

The group of methods `crm.timeline.bindings.*` manages the bindings of timeline records with CRM entities. A binding determines which entity timelines display the record: a single record can be bound to several entities at once.

For instance, a comment from a lead needs to be displayed in the timeline of a deal. To achieve this, bind the record to the deal and remove the binding from the lead.

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

## Getting Started

1. Obtain the `OWNER_ID` using the method [crm.timeline.comment.add](../comments/crm-timeline-comment-add.md) or [crm.timeline.logmessage.add](../logmessage/crm-timeline-logmessage-add.md) — this is the identifier of the timeline record itself, not of the CRM entity.
2. Determine the CRM entity to bind the record to: its type `ENTITY_TYPE` and identifier `ENTITY_ID`.
3. Check the structure of the binding fields using the method [crm.timeline.bindings.fields](./crm-timeline-bindings-fields.md).
4. Add the binding of the timeline record to the CRM entity using the method [crm.timeline.bindings.bind](./crm-timeline-bindings-bind.md).
5. Check the current bindings of the timeline record using the method [crm.timeline.bindings.list](./crm-timeline-bindings-list.md) with the filter `OWNER_ID`.
6. Remove the necessary binding using the method [crm.timeline.bindings.unbind](./crm-timeline-bindings-unbind.md).

## How the Methods Work

- The binding with the entity in which the timeline record was created appears automatically. The methods in this section add and remove any bindings, including this one.
- Reading bindings is available to any user. Adding and removing them requires permission to modify the CRM entity specified in `ENTITY_ID`.
- The method [crm.timeline.bindings.list](./crm-timeline-bindings-list.md) returns the bindings of a single timeline record and delivers them in pages of 50.

## Binding with Other Objects

The methods in this section connect two objects — a timeline record and a CRM entity, while activities have their own group of methods for this.

**Timeline Records.** All methods in this section accept the timeline record ID `OWNER_ID`. The records themselves are created, updated, and listed by the methods of the [CRM Timeline Comments](../comments/index.md) and [Log Record Journal](../logmessage/index.md) sections.

**CRM Entities.** A CRM entity is defined by a pair of values: the type `ENTITY_TYPE` and the identifier `ENTITY_ID`. In `ENTITY_TYPE`, pass the string code of the CRM object type `entityTypeName`, for example `deal`. The acceptable codes are listed on the method pages, and the general structure of the codes is described in the section [CRM Object Type](../../data-types.md#object_type).

The identifier `ENTITY_ID` is returned by the universal method [crm.item.list](../../universal/crm-item-list.md) — it works with leads, deals, contacts, companies, quotes, invoices, and smart processes. For activities, the identifier is returned by the method [crm.activity.list](../activities/activity-base/crm-activity-list.md), and for orders — by the methods of the [Order in the Online Store](../../../sale/order/index.md) section.

**Activities.** Do not confuse the bindings of a timeline record with the bindings of an activity. The binding of an activity to CRM entities is managed by a separate group of methods [crm.activity.binding.*](../activities/binding/index.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [crm.timeline.bindings.bind](./crm-timeline-bindings-bind.md) | Adds a binding of the timeline record with a CRM entity ||
|| [crm.timeline.bindings.list](./crm-timeline-bindings-list.md) | Returns a list of bindings of the timeline record with CRM entities ||
|| [crm.timeline.bindings.unbind](./crm-timeline-bindings-unbind.md) | Removes the binding of the timeline record with a CRM entity ||
|| [crm.timeline.bindings.fields](./crm-timeline-bindings-fields.md) | Returns the description of the fields of the timeline record binding with a CRM entity ||
|#
