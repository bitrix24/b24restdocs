# Stages: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of methods rpa.task.* has been halted.  
Please use the section [Smart Processes CRM](../../../crm/universal/user-defined-object-types/index.md).

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)  
> Who can execute the method: any user

A set of methods for working with tasks.

#|  
|| **Method** | **Description** ||  
|| [rpa.task.addUser](./rpa-task-add-user.md) | Adds a user to an existing task ||  
|| [rpa.task.do](./rpa-task-do.md) | Executes the task ||  
|| [rpa.task.delete](./rpa-task-delete.md) | Deletes the Automation rule named `robotName` from the process with the identifier `typeId` at the stage with the identifier `stageId` ||  
|#