# Timeline Record Bindings with CRM Entities: Overview of Methods

The group of methods `crm.timeline.bindings.*` manages the bindings of timeline records with CRM entities.

For instance, you may want to display a comment in the timeline of a deal instead of a lead. To achieve this, you obtain the comment ID, determine the structure of the binding fields, add the binding to the deal, remove the binding from the lead, and if necessary, check the current bindings of the record.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official documentation.

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

## Getting Started

1. Obtain the `OWNER_ID` of the timeline record.
2. Determine the `ENTITY_TYPE` and `ENTITY_ID` of the CRM entity.
3. To check the structure of the binding fields, use the method [crm.timeline.bindings.fields](./crm-timeline-bindings-fields.md).
4. Add the binding of the timeline record to the CRM entity using the method [crm.timeline.bindings.bind](./crm-timeline-bindings-bind.md).
5. To check the current bindings of the timeline record, use the method [crm.timeline.bindings.list](./crm-timeline-bindings-list.md) with the filter `OWNER_ID`.
6. Remove the necessary binding using the method [crm.timeline.bindings.unbind](./crm-timeline-bindings-unbind.md).

## Binding with Other Objects

**Timeline Comments.** All methods in this section accept the timeline record ID `OWNER_ID`. Obtain the `OWNER_ID` from the response of the method [crm.timeline.comment.add](../comments/crm-timeline-comment-add.md) or retrieve it from the list using the method [crm.timeline.comment.list](../comments/crm-timeline-comment-list.md).

**CRM Entities.** To bind a timeline record with a CRM entity, pass the string type `ENTITY_TYPE` and the identifier `ENTITY_ID`. The acceptable values for `ENTITY_TYPE` are `lead`, `deal`, `contact`, `company`, `order`. The identifier of the required entity `ENTITY_ID` can be obtained using the universal method [crm.item.list](../../universal/crm-item-list.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [crm.timeline.bindings.bind](./crm-timeline-bindings-bind.md) | Adds a binding of the timeline record with a CRM entity ||
|| [crm.timeline.bindings.list](./crm-timeline-bindings-list.md) | Returns a list of bindings for the timeline record ||
|| [crm.timeline.bindings.unbind](./crm-timeline-bindings-unbind.md) | Removes the binding of the timeline record with a CRM entity ||
|| [crm.timeline.bindings.fields](./crm-timeline-bindings-fields.md) | Returns the description of the binding fields ||
|#