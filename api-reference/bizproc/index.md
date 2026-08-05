# Workflows and Automation Rules: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Workflows automate operations with documents, CRM objects, files, and list items. The `bizproc.*` methods start processes from templates, manage active instances, process user tasks, and add custom application actions or automation rules to automation.

A process can start automatically when an event occurs in Bitrix24 or manually at the user's initiative. Through the REST API, it is started using the [bizproc.workflow.start](./bizproc-workflow-start.md) method.

For new development, application automation rules are usually preferred. Automation rules are available in CRM automation, workflows, and smart scripts, while application actions are useful for supporting existing integrations in the workflow designer.

> Quick navigation: [all methods](#all-methods)
>
> User documentation:
> - [Workflows: New Interface and Features](https://helpdesk.bitrix24.com/open/21600812/)
> - [Automation Rules in CRM](https://helpdesk.bitrix24.com/open/24545552/)

## How to Choose a Section

#|
|| **If You Need To** | **Open the Section** ||
|| Start a process, retrieve a list of active processes, or stop execution | [Main Workflow Methods](#workflow) ||
|| Add, update, retrieve, or delete a workflow template | [Workflow Templates](./template/index.md) ||
|| Retrieve, complete, or delegate a user task | [Workflow Tasks](./bizproc-task/index.md) ||
|| Add an external step for CRM automation, workflows, or smart scripts | [Application Automation Rules](./bizproc-robot/index.md) ||
|| Support an external step that is already used in the workflow designer | [Application Actions](./bizproc-activity/index.md) ||
|#

## How to Start

1. Prepare a workflow template in the Bitrix24 interface or add it using [bizproc.workflow.template.add](./template/bizproc-workflow-template-add.md)
2. Define the document for which the process will run. To start a process, you need `DOCUMENT_ID`, for example `['crm', 'CCrmDocumentLead', 'LEAD_1']`
3. Start the process using [bizproc.workflow.start](./bizproc-workflow-start.md), and pass `TEMPLATE_ID`, `DOCUMENT_ID`, and the template input parameters
4. Monitor active processes using [bizproc.workflow.instances](./bizproc-workflow-instances.md)
5. Process user tasks using the [bizproc.task.*](./bizproc-task/index.md) method group if the scenario requires approval, acknowledgment, or additional information

## Relationship with Other Objects

Workflows are linked to Bitrix24 documents, templates, user tasks, and external application handlers.

**Document.** A process is started for a specific document: a lead, deal, list item, Drive file, or another supported object. The link is passed in the `DOCUMENT_ID` parameter of the [bizproc.workflow.start](./bizproc-workflow-start.md) method. The document type must match the type for which the template was created.

**Workflow Template.** A template defines the process logic and the set of input parameters. Templates can be managed using the [bizproc.workflow.template.*](./template/index.md) method group. The template identifier `TEMPLATE_ID` is used when starting a process and filtering the list of active processes.

**Workflow Task.** A task appears when a process needs a user action: approval, acknowledgment, or additional data input. Tasks can be retrieved using [bizproc.task.list](./bizproc-task/bizproc-task-list.md), completed using [bizproc.task.complete](./bizproc-task/bizproc-task-complete.md), and delegated using [bizproc.task.delegate](./bizproc-task/bizproc-task-delegate.md).

**Application.** Application automation rules and actions call the external `HANDLER`. If the handler must return a result to the process, specify output parameters and the response waiting mode during registration, then pass the values using [bizproc.event.send](./bizproc-robot/bizproc-event-send.md).

## What to Consider

- Methods for managing templates, automation rules, and actions are executed by an administrator
- Automation rule and action methods work only in the context of an installed application
- An automation rule or action that waits for a result receives `EVENT_TOKEN` during scenario execution. This key is required for the [bizproc.event.send](./bizproc-robot/bizproc-event-send.md) method
- A started process uses a copy of the template as it was at the start time. Changes to the template do not affect an already active process

## Overview of Methods {#all-methods}

> Scope: [`bizproc`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Main Workflow Methods {#workflow}

#|
|| **Method** | **Description** ||
|| [bizproc.workflow.start](./bizproc-workflow-start.md) | Starts a workflow ||
|| [bizproc.workflow.instances](./bizproc-workflow-instances.md) | Retrieves a list of started workflows ||
|| [bizproc.workflow.kill](./bizproc-workflow-kill.md) | Deletes a started workflow along with its data ||
|| [bizproc.workflow.terminate](./bizproc-workflow-terminate.md) | Stops workflow execution ||
|#

### Workflow Templates

#|
|| **Method** | **Description** ||
|| [bizproc.workflow.template.add](./template/bizproc-workflow-template-add.md) | Adds a workflow template from a file ||
|| [bizproc.workflow.template.update](./template/bizproc-workflow-template-update.md) | Updates a workflow template ||
|| [bizproc.workflow.template.list](./template/bizproc-workflow-template-list.md) | Retrieves a list of workflow templates ||
|| [bizproc.workflow.template.delete](./template/bizproc-workflow-template-delete.md) | Deletes a workflow template ||
|#

### Workflow Tasks

#|
|| **Method** | **Description** ||
|| [bizproc.task.list](./bizproc-task/bizproc-task-list.md) | Retrieves a list of workflow tasks ||
|| [bizproc.task.complete](./bizproc-task/bizproc-task-complete.md) | Completes a workflow task ||
|| [bizproc.task.delegate](./bizproc-task/bizproc-task-delegate.md) | Delegates a workflow task ||
|#

### Application Automation Rules

#|
|| **Method** | **Description** ||
|| [bizproc.robot.add](./bizproc-robot/bizproc-robot-add.md) | Registers an application automation rule ||
|| [bizproc.robot.update](./bizproc-robot/bizproc-robot-update.md) | Updates an application automation rule ||
|| [bizproc.robot.list](./bizproc-robot/bizproc-robot-list.md) | Retrieves a list of application automation rules ||
|| [bizproc.robot.delete](./bizproc-robot/bizproc-robot-delete.md) | Deletes an application automation rule ||
|| [bizproc.event.send](./bizproc-robot/bizproc-event-send.md) | Passes automation rule or action output values to the process ||
|#

### Application Actions

#|
|| **Method** | **Description** ||
|| [bizproc.activity.add](./bizproc-activity/bizproc-activity-add.md) | Adds an application action ||
|| [bizproc.activity.update](./bizproc-activity/bizproc-activity-update.md) | Updates an application action ||
|| [bizproc.activity.list](./bizproc-activity/bizproc-activity-list.md) | Retrieves a list of application actions ||
|| [bizproc.activity.delete](./bizproc-activity/bizproc-activity-delete.md) | Deletes an application action ||
|| [bizproc.activity.log](./bizproc-activity/bizproc-activity-log.md) | Writes a message to the workflow log ||
|#
