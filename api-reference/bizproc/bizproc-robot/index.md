# Application Automation Rules: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Application Automation Rules add a step to Bitrix24 automation that invokes an application handler. This step accepts input parameters, can wait for a response from an external system, and return output values to the workflow.

In the REST API, Application Automation Rules and [application actions](../bizproc-activity/index.md) are similar in model. Each type has separate methods for registration, updating, deletion, and listing. A common execution mechanism is used within the platform.

Automation Rules can be utilized in CRM automation, workflows, and Smart scripts. Therefore, for new development, it is recommended to choose Automation Rules, while application actions should primarily be used to support existing integrations.

> Quick navigation: [all methods](#all-methods)
>
> User documentation:
> - [Automation rules in CRM](https://helpdesk.bitrix24.com/open/24545552/)
> - [Smart scripts in CRM](https://helpdesk.bitrix24.com/open/25071006/)

## What Tasks Do Application Automation Rules Solve

- Perform external actions in CRM automation and workflows.
- Return computed data to the process for subsequent steps.
- Restrict availability based on document type and Bitrix24 edition.
- Log messages during the execution of the script.

## How Result Waiting Works

If the Automation Rule needs to wait for a response from an external system, specify `RETURN_PROPERTIES` and `USE_SUBSCRIPTION = 'Y'` when registering or updating via [bizproc.robot.add](./bizproc-robot-add.md) and [bizproc.robot.update](./bizproc-robot-update.md).

The script then operates as follows:

1. The Automation Rule is triggered in the automation and sends data to the application handler `HANDLER`.
2. The application receives a unique key `EVENT_TOKEN` and calculates the result.
3. The application returns values to the process using the method [bizproc.event.send](./bizproc-event-send.md).
4. If necessary, the application adds a note to the process log via `LOG_MESSAGE`.

{% note tip "User Documentation" %}

- [Workflow test log](https://helpdesk.bitrix24.com/open/22095380/)

{% endnote %}

## How to Get Started

1. Register the Automation Rule using the method [bizproc.robot.add](./bizproc-robot-add.md).
2. Place the Automation Rule in the desired CRM automation or workflow and start the script.
3. Update the Automation Rule settings using the method [bizproc.robot.update](./bizproc-robot-update.md).
4. Check the list of registered Automation Rules via [bizproc.robot.list](./bizproc-robot-list.md).
5. Remove outdated Automation Rules using the method [bizproc.robot.delete](./bizproc-robot-delete.md).

## Important Considerations

- The methods for managing Automation Rules [bizproc.robot.add](./bizproc-robot-add.md), [bizproc.robot.update](./bizproc-robot-update.md), [bizproc.robot.list](./bizproc-robot-list.md), [bizproc.robot.delete](./bizproc-robot-delete.md) only work in the context of the installed application.
- For [bizproc.event.send](./bizproc-event-send.md), a valid `EVENT_TOKEN` is required; otherwise, the method will return an access error.

## Connection with Other Objects

- **CRM Objects.** Through `DOCUMENT_TYPE` and `FILTER`, the Automation Rule connects with leads, deals, and other document types where it should be available.
- **Application Actions.** Automation Rules and actions use a common internal mechanism, so it is important to choose a primary path for new scenarios when designing the integration.

## Overview of Methods {#all-methods}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [bizproc.robot.add](./bizproc-robot-add.md) | Registers a new Automation Rule ||
|| [bizproc.robot.update](./bizproc-robot-update.md) | Updates the fields of the Automation Rule ||
|| [bizproc.robot.list](./bizproc-robot-list.md) | Retrieves a list of Automation Rules registered by the application ||
|| [bizproc.robot.delete](./bizproc-robot-delete.md) | Deletes a registered Automation Rule ||
|| [bizproc.event.send](./bizproc-event-send.md) | Sends the output values of the Automation Rule or action to the process ||
|#