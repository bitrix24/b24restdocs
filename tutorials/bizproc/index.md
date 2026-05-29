# Workflows and Automation rules: typical scenarios

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A scenario is a sequence of requests for a single task. Scenarios help extend Bitrix24 automation via the REST API: adding an application workflow action or Automation rule, embedding your own interface into the Automation rule settings, finding active processes, and terminating outdated processes.

A workflow is an automation launched based on a template. An Automation rule or a workflow action is a step within the automation that calls an application handler, accepts parameters, and can return a result to the process.

> Quick links: [all scenarios](#choose-tutorial)
>
> User documentation: [Workflows: new interface and features](https://helpdesk.bitrix24.com/open/21600812/)

## Connection with Other Objects

Scenarios are linked to Automation rules, workflow actions, tasks, running processes, users, and the application interface.

- **Application Automation rules.** An application Automation rule adds a step to CRM automation, workflows, or Smart scripts. For new development, it is recommended to choose Automation rules: they work in CRM automation and workflows. You can register an Automation rule using the [bizproc.robot.add](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md) method. Bitrix24 will call the application handler every time the automation reaches the step with this Automation rule.
- **Application workflow actions.** An application workflow action adds a step to the workflow designer. It is suitable for existing integrations or scenarios that work through workflow templates. You can register a workflow action using the [bizproc.activity.add](../../api-reference/bizproc/bizproc-activity/bizproc-activity-add.md) method. Bitrix24 will call the application handler when the workflow executes this step.
- **Application handler.** A handler is a public URL on the application side. Bitrix24 sends data to it when executing an Automation rule, a workflow action, or opening an embedded settings interface.
- **Automation rule or workflow action parameters.** An embedded application interface opens in the Automation rule or workflow action settings if you pass `USE_PLACEMENT` and `PLACEMENT_HANDLER` during registration or update: [bizproc.robot.add](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md), [bizproc.robot.update](../../api-reference/bizproc/bizproc-robot/bizproc-robot-update.md), [bizproc.activity.add](../../api-reference/bizproc/bizproc-activity/bizproc-activity-add.md), [bizproc.activity.update](../../api-reference/bizproc/bizproc-activity/bizproc-activity-update.md). Parameter values are retained by calling [BX24.placement.call](../../api-reference/widgets/ui-interaction/bx24-placement-call.md).
- **Tasks and running processes.** A workflow task stores `WORKFLOW_ID` — the identifier of the process that created the task. User tasks can be retrieved using the [bizproc.task.list](../../api-reference/bizproc/bizproc-task/bizproc-task-list.md) method, and active processes using the [bizproc.workflow.instances](../../api-reference/bizproc/bizproc-workflow-instances.md) method. To stop a process and retain the data, use [bizproc.workflow.terminate](../../api-reference/bizproc/bizproc-workflow-terminate.md). To delete a process along with all process data, use [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md).
- **Users.** In a scenario involving a terminated employee, the [user.get](../../api-reference/user/user-get.md) method helps find the employee's `ID`. This identifier is passed to the `USER_ID` filter of the [bizproc.task.list](../../api-reference/bizproc/bizproc-task/bizproc-task-list.md) method to retrieve their uncompleted tasks.

## Getting Started

1. Define the task: add automation, configure the Automation rule interface, or complete workflows
2. Select a scenario from the [How to choose a scenario](#choose-tutorial) table
3. Check which permissions and scopes are specified in the selected scenario
4. Prepare the input data for the scenario: a public application handler, Automation rule or workflow action code, a filter date, or a user identifier
5. Execute the methods in the order described in the scenario
6. Verify the result: a list of Automation rules can be retrieved using the [bizproc.robot.list](../../api-reference/bizproc/bizproc-robot/bizproc-robot-list.md) method, a list of actions using the [bizproc.activity.list](../../api-reference/bizproc/bizproc-activity/bizproc-activity-list.md) method, and a list of active workflows using the [bizproc.workflow.instances](../../api-reference/bizproc/bizproc-workflow-instances.md) method

## How to choose a scenario {#choose-tutorial}

#|
|| **If needed** | **Open** ||
|| Create a workflow action or Automation rule that generates an invoice based on a lead or a deal | [How to add an action to create an invoice based on a lead or a deal](./activity.md) ||
|| Add an application interface to the Automation rule settings and save parameters via `BX24.placement.call` | [How to embed your own UI into Automation rule parameters](./setting-robot.md) ||
|| Find tasks of a terminated employee and complete associated workflows | [How to complete the workflows of a terminated employee](./how-to-kill-workflows.md) ||
|| Find active workflows by start date and complete them in bulk | [How to complete workflows in bulk using a date filter](./how-to-filter-and-kill-workflows.md) ||
|| View the reference guide for workflow and Automation rule methods | [Workflows and Automation rules](../../api-reference/bizproc/index.md) ||
|#
