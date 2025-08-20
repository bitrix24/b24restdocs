# Workflows and Automation Rules

> Quick navigation: [all methods and events](#all-methods)

Workflows in Bitrix24 are a low-code tool that allows you to automate everyday tasks without needing deep programming knowledge. Users can easily configure various operations using ready-to-use actions.

![Workflow Designer](_images/sequence_templ3_sm.png "Workflow Designer")

You will find these automated processes and can launch them directly from sections such as [Feed](../log/index.md), [CRM](../crm/index.md), [Drive](../disk/index.md), [SPAs](../crm/universal/user-defined-object-types/index.md), and [Universal Lists](../lists/index.md).

Workflows can be automatically triggered by specific events (for example, when a new deal is added to CRM) or initiated by the user (like the vacation approval process through the Feed).

In addition to workflows, Bitrix24 offers [automation rules](https://helpdesk.bitrix24.com/open/21817474/) — an even simpler way to automate tasks, especially convenient for regular users. Automation rules are actively used in CRM and task management.

![Setting Up Automation Rules](_images/robots.png "Setting Up Automation Rules")

## Popular Scenarios Using REST API

With the appropriate methods, you can:

- Add ready-made industry [workflow templates](./template/bizproc-workflow-template-add.md) accepted in specific business sectors or types of companies
- Propose your scenarios for automatic [workflow launches](bizproc-workflow-start.md) (for example, generating an explanatory note in case of a serious project deadline violation, etc.)
- Offer workflow launches in your [widgets](../widgets/index.md) if standard interfaces do not provide such an option for the user

You can expand the capabilities of user automation in Bitrix24 by adding your own [workflow actions](bizproc-activity/bizproc-activity-add.md) and [automation rules](bizproc-robot/bizproc-robot-add.md) that can:

- Send documents to external systems
- Create or modify orders in an external online store
- Transfer data from external systems to Bitrix24
- Launch ready-made external processes (for example, connecting a special external chatbot to a conversation with a client when the deal moves to the delivery approval stage)
- Transfer payment data to external Sales Intelligence
- Execute a completed business scenario (for example, generating a complex invoice or collecting data from all deals of a single client and returning the total for further use)

{% note info "Recommendations" %}

- Add automation rules instead of workflow actions, as each registered automation rule can also be used as a workflow action. Additionally, your automation rules will be available in [Smart Scenarios](https://helpdesk.bitrix24.com/open/21319214/)
- If you want to send changes to an external system when deal or lead statuses change, it’s better to [register an automation rule](bizproc-robot/bizproc-robot-add.md) that the user will place at the desired stage, instead of intercepting the event on [deal change](../crm/deals/events/on-crm-deal-add.md) or [lead](../crm/leads/events/on-crm-lead-add.md). This will save you from having to create an interface for matching lead and deal stages with statuses or events of the external system and significantly simplify development.

{% endnote %}

## Features of Workflow Functionality

It is important to understand the terminology and relationships between the objects that implement workflow functionality.

### Workflow Template

This is essentially a logical scheme, an algorithm that implements the required business logic using individual actions and operations available in the workflow designer.

A [template](./template/bizproc-workflow-template-add.md) is always tied to a specific **base object**, the data of which it will operate on. For example, a template may be linked to CRM deals. In this case, the base object will be a specific deal for which the workflow will be launched.

The binding to the base object also defines the context for launching the workflow: for instance, you cannot launch a workflow for a lead based on a template designed for a deal.

Objects can include leads, contacts, companies, invoices, and estimates in CRM, elements of user-defined object types (SPAs), files in Bitrix24 Drive, and others.

A workflow template can have input parameters that the user (or your code) must fill in when launching the workflow based on the selected template.

In addition to input parameters, internal variables can also be used. The template developer must add them in the template settings.

It is important to understand that when we talk about input parameters and variables, we are referring to "descriptions." However, actual values for parameters and variables only arise when a real workflow is launched.

### Workflows

A workflow is, in fact, a "live" instance created from a template at the moment of [launch](./bizproc-workflow-start.md) for a specific base object. Clearly, multiple workflows can be launched simultaneously for different base objects.

When a workflow is launched, a copy of the workflow template is created, which remains unchanged until the workflow is completed. If the user makes changes to the template during the execution of the workflow, it will not affect the logic of the already launched workflow.

The values of input parameters and internal variables also have specific values for each individual workflow and change only for it.

### Workflow Tasks

During execution, a workflow can use special actions — tasks for workflow participants, when the logic of the workflow requires obtaining additional information from the user, such as approving a document, uploading a file, and so on.

Such actions generate **workflow tasks**. Users who receive tasks see the corresponding information in Bitrix24 and also receive notifications. The [current tasks](bizproc-task/bizproc-task-list.md) for a user can also be retrieved using the corresponding REST API method for automatic processing.

### Workflow Actions

**Actions** of a workflow are the "blocks" from which the workflow template is built. Typically, they represent isolated atomic operations like "Create Task," "Generate Invoice," "Change Document Field Value," and so on.

Workflow actions can have input parameters that the user can specify in the workflow designer during the development of the desired template.

Additionally, actions can return resulting data "back" to the workflow. This data can be used by the workflow designer as input parameters for subsequent actions.

Using the REST API, you can [add your own actions](bizproc-activity/bizproc-activity-add.md) to workflows in Bitrix24.

### Automation Rules

**Automation rules** are objects similar to workflow actions, with the main difference being that they are primarily used not in the workflow designer but in special interfaces of CRM and tasks.

Automation rules can also have input parameters, the values of which will be set by the user configuring the rules in lead funnels, deals, or smart scenarios. They can also return values for use as input parameters for subsequent automation rules.

Using the REST API, you can [add your own automation rules](bizproc-robot/bizproc-robot-add.md) to Bitrix24. We recommend adding automation rules instead of workflow actions, as automation rules can be used within the workflow designer, meaning you will add your functionality to both the automation rules settings and the workflow designer at once.

## Recommended Learning Sequence

- Add [your automation rule](bizproc-robot/bizproc-robot-add.md) to Bitrix24
- Learn how to [start a ready-made workflow](bizproc-workflow-start.md) or [stop](bizproc-workflow-kill.md) an already launched one
- Discover how to [add your template](./template/bizproc-workflow-template-add.md)
- Learn how to get a list of current [workflow tasks](bizproc-task/bizproc-task-list.md), automatically [complete](bizproc-task/bizproc-task-complete.md) them for the user, and [delegate](./bizproc-task/bizproc-task-delegate.md) to another user

## Overview of Methods {#all-methods}

#|
|| **Method** | **Description** ||
|| [bizproc.workflow.start](./bizproc-workflow-start.md) | Starts a new workflow ||
|| [bizproc.workflow.instances](./bizproc-workflow-instances.md) | Returns a list of launched workflows ||
|| [bizproc.workflow.kill](./bizproc-workflow-kill.md) | Deletes a launched workflow along with all process data ||
|| [bizproc.workflow.terminate](./bizproc-workflow-terminate.md) | Interrupts the execution of a workflow ||
|#

### Workflow Templates

#|
|| **Method** | **Description** ||
|| [bizproc.workflow.template.add](./template/bizproc-workflow-template-add.md) | Adds a new workflow template ||
|| [bizproc.workflow.template.update](./template/bizproc-workflow-template-update.md) | Updates an existing workflow template ||
|| [bizproc.workflow.template.delete](./template/bizproc-workflow-template-delete.md) | Deletes a workflow template ||
|| [bizproc.workflow.template.list](./template/bizproc-workflow-template-list.md) | Returns a list of workflow templates ||
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
|| [bizproc.robot.add](./bizproc-robot/bizproc-robot-add.md) | Registers a new automation rule ||
|| [bizproc.robot.update](./bizproc-robot/bizproc-robot-update.md) | Updates fields of an already added automation rule ||
|| [bizproc.robot.list](./bizproc-robot/bizproc-robot-list.md) | Returns a list of automation rules registered by the application ||
|| [bizproc.robot.delete](./bizproc-robot/bizproc-robot-delete.md) | Deletes a registered automation rule ||
|| [bizproc.event.send](./bizproc-robot/bizproc-event-send.md) | Returns output parameters for the action specified in the action description ||
|#

### Application Actions

#|
|| **Method** | **Description** ||
|| [bizproc.activity.add](./bizproc-activity/bizproc-activity-add.md) | Adds a new action for use in workflows ||
|| [bizproc.activity.update](./bizproc-activity/bizproc-activity-update.md) | Updates fields of an already added action ||
|| [bizproc.activity.list](./bizproc-activity/bizproc-activity-list.md) | Returns a list of actions installed by the application ||
|| [bizproc.activity.log](./bizproc-activity/bizproc-activity-log.md) | Records information in the workflow log ||
|| [bizproc.activity.delete](./bizproc-activity/bizproc-activity-delete.md) | Deletes an action installed by the application ||
|#