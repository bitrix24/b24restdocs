# Timeline Records: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of methods rpa.timeline.* has been halted.  
Please use the section [Smart Processes CRM](../../../crm/universal/user-defined-object-types/index.md).

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)  
> Who can execute the method: any user

A set of methods for working with timeline records.

#|  
|| **Method** | **Description** ||  
|| [rpa.timeline.add](./rpa-timeline-add.md) | Creates a new timeline record for an entity ||  
|| [rpa.timeline.update](./rpa-timeline-update.md) | Updates the timeline record with the identifier `id` ||  
|| [rpa.timeline.updateIsFixed](./rpa-timeline-update-is-fixed.md) | Updates the attachment flag of the record ||  
|| [rpa.timeline.listForItem](./rpa-timeline-list-for-item.md) | Retrieves an array of timeline records for an entity ||  
|| [rpa.timeline.delete](./rpa-timeline-delete.md) | Deletes a timeline record ||  
|#