# Application Actions: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Application actions add a step to the workflow designer that triggers an application handler. This step accepts input parameters, can wait for a response from the application, and returns output values to the process.

In the REST API, application actions and [Automation rules](../bizproc-robot/bizproc-robot-add.md) are similar in model. Registration, updating, deletion, and listing are available as separate methods for each type. Within the platform, processing is built on a common mechanism. The record type is determined by the internal system attribute `IS_ROBOT`, which distinguishes records as actions or robots. A value of `Y` indicates a robot, while `N` indicates an action. In the methods `bizproc.activity.*`, this attribute is not passed.

Application actions are primarily suitable for supporting existing integrations and enhancing previously added actions. For new development, it is recommended to use Automation rules. They operate in CRM automation and workflows, covering more scenarios.

> Quick navigation: [all methods](#all-methods)

## Scenarios Suitable for Application Actions

- Add a step to the workflow template that calls an external application service.
- Pass parameters from the workflow document to the action and return output data to the process.
- Log intermediate messages in the workflow.
- Restrict the availability of the action based on document type and Bitrix24 edition.
- Fine-tune the logic within an already functioning template without migrating to another type of automation.
- Implement auxiliary scenarios for monitoring and executing workflow tasks.

## Getting Started

1. Register the action via [bizproc.activity.add](./bizproc-activity-add.md).
2. If you need to return a result, specify `RETURN_PROPERTIES` with a description of the output parameters and set `USE_SUBSCRIPTION` to `'Y'` when registering or updating the action through [bizproc.activity.add](./bizproc-activity-add.md) and [bizproc.activity.update](./bizproc-activity-update.md). Then pass the values through [bizproc.event.send](../bizproc-robot/bizproc-event-send.md).
3. If necessary, restrict the availability of the action using `FILTER` in [bizproc.activity.add](./bizproc-activity-add.md) and [bizproc.activity.update](./bizproc-activity-update.md). Use `INCLUDE` and `EXCLUDE` rules, for example for cloud `b24` or on-premise `box`.
4. Add the action to the template and start the process:
   - through the interface in the workflow designer
   - through template methods [bizproc.workflow.template.add](../template/bizproc-workflow-template-add.md), [bizproc.workflow.template.update](../template/bizproc-workflow-template-update.md), and [bizproc.workflow.start](../bizproc-workflow-start.md).
5. For diagnostics and execution control, log stages in the log via [bizproc.activity.log](./bizproc-activity-log.md).
6. Check the installed actions through [bizproc.activity.list](./bizproc-activity-list.md).
7. Remove outdated actions via [bizproc.activity.delete](./bizproc-activity-delete.md).

{% note tip "User Documentation" %}

- [Workflow test log](https://helpdesk.bitrix24.com/open/22095380/)

{% endnote %}

## Important Considerations

- The methods [bizproc.activity.add](./bizproc-activity-add.md), [bizproc.activity.update](./bizproc-activity-update.md), [bizproc.activity.list](./bizproc-activity-list.md), [bizproc.activity.delete](./bizproc-activity-delete.md) only work in the context of the installed application.
- For [bizproc.activity.log](./bizproc-activity-log.md) and [bizproc.event.send](../bizproc-robot/bizproc-event-send.md), a unique key `EVENT_TOKEN` is required, which is sent to the `HANDLER` during the execution of the action in the workflow.

## Relationships with Other Objects

- **Workflow Templates.** The action becomes available in the designer after registration via [bizproc.activity.add](./bizproc-activity-add.md) and is used within the template when it is launched.
- **Document Type.** Through `DOCUMENT_TYPE` and `FILTER`, the action is linked to the appropriate document type and is displayed only in the relevant context.
- **Automation Rules.** Actions and robots use a common internal mechanism, so it is important to consider overlaps in codes and scenarios during registration.

## Overview of Methods {#all-methods}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [bizproc.activity.add](./bizproc-activity-add.md) | Adds a new action for use in workflows ||
|| [bizproc.activity.update](./bizproc-activity-update.md) | Updates an action ||
|| [bizproc.activity.list](./bizproc-activity-list.md) | Retrieves a list of actions installed by the application ||
|| [bizproc.activity.delete](./bizproc-activity-delete.md) | Deletes an action installed by the application ||
|| [bizproc.activity.log](./bizproc-activity-log.md) | Logs information in the workflow log ||
|#