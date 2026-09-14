# Stages: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of methods rpa.stage.* has been halted.  
Please refer to the [SPA CRM](../../../crm/universal/user-defined-object-types/index.md) section.

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)  
> Who can execute the method: any user

#|  
|| **Method** | **Description** ||  
|| [rpa.stage.add](./rpa-stage-add.md) | Adds a new stage ||  
|| [rpa.stage.update](./rpa-stage-update.md) | Updates a stage by `id` ||  
|| [rpa.stage.get](./rpa-stage-get.md) | Retrieves information about a stage by its `id` ||  
|| [rpa.stage.listForType](./rpa-stage-list-for-type.md) | Gets a list of stages for the process, sorted in order with final stages at the end ||  
|| [rpa.stage.delete](./rpa-stage-delete.md) | Deletes a stage ||  
|#