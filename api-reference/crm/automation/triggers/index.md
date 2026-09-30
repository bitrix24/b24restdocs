# CRM Automation Triggers: Overview of Methods

CRM automation triggers help the application transmit external events to the CRM. If a trigger is configured for an object, the event can move it to the desired stage or status.

For example, a telephony application registers the "Call completed" trigger, an administrator links it to the "In Progress" stage, and after a call the application moves the deal to that stage. The methods below let you register a trigger, retrieve the list of application triggers, send an event to CRM automation, and delete an unnecessary trigger.

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Triggers in CRM](https://helpdesk.bitrix24.com/open/24564704/)

{% note info "" %}

The methods in this section work only in the context of an [application](../../../../settings/app-installation/index.md).

{% endnote %}

## Getting Started

1. Register a trigger using the [crm.automation.trigger.add](./crm-automation-trigger-add.md) method.

2. Link the registered trigger to the desired stage or status in the CRM automation settings, in the "Automation rules and triggers" section. There are no methods for linking.

3. If necessary, retrieve the list of application triggers using the [crm.automation.trigger.list](./crm-automation-trigger-list.md) method.

4. Prepare the CRM object identifiers: `OWNER_TYPE_ID` and `OWNER_ID`.

5. Execute the trigger using the [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) method and check the object's stage using the [crm.item.get](../../universal/crm-item-get.md) method.

6. When the trigger is no longer needed, delete it using the [crm.automation.trigger.delete](./crm-automation-trigger-delete.md) method and remove its binding in the automation settings.

## Key Parameters

**CODE.** The identifier of the trigger within the application. The application assigns it when registering the trigger using the [crm.automation.trigger.add](./crm-automation-trigger-add.md) method. This identifier is then used in the [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) and [crm.automation.trigger.delete](./crm-automation-trigger-delete.md) methods.

If you call the [crm.automation.trigger.add](./crm-automation-trigger-add.md) method again with the same `CODE`, it updates the trigger name `NAME`.

**OWNER_TYPE_ID.** The identifier of the CRM object type. Used in the [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) method. Triggers are available for leads, deals, estimates, invoices, and SPAs. If you pass a contact or a company, the trigger fires for the objects linked to them, such as deals.

**OWNER_ID.** The identifier of a specific CRM object. Used in the [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) method.

## Important Considerations

- The [crm.automation.trigger.list](./crm-automation-trigger-list.md) method returns only the triggers of the current application, the whole list in a single call. Each trigger has a name `NAME` and a code `CODE`.

- The [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) method sends an event to CRM automation and returns `true` even if the stage has not changed. The stage changes only if a trigger of the current application with the same `CODE` is linked to it and the trigger's conditions are met.

- An object moves to a stage that comes earlier in the pipeline than its current stage only if moving to a previous stage is allowed in the trigger settings.

- The [crm.automation.trigger.delete](./crm-automation-trigger-delete.md) method does not remove the trigger's bindings in the automation settings, and they remain even after the application is uninstalled. Remove them manually: if the same application registers a trigger with the same `CODE` again, the old binding starts firing again.

## Relationship with Other Objects

**CRM Automation.** A registered trigger appears in the CRM automation settings, in the "Automation rules and triggers" section, with the application name in square brackets, for example "[Telephony] Call completed". This is where it is linked to a stage or status. Launching a webhook trigger, which is configured without an application, is described in the [CRM Automation](../index.md) section.

**CRM Objects.** In the [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) method, the parameters `OWNER_TYPE_ID` and `OWNER_ID` define the type of CRM object and the specific object for triggering. The value of `OWNER_TYPE_ID` can be obtained using the [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) method. The `OWNER_ID` identifier can be obtained using the universal [crm.item.list](../../universal/crm-item-list.md) method.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

#| 
|| **Method** | **Description** ||
|| [crm.automation.trigger.add](./crm-automation-trigger-add.md) | Adds an application trigger ||
|| [crm.automation.trigger.list](./crm-automation-trigger-list.md) | Retrieves a list of application triggers ||
|| [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) | Executes the trigger for a CRM object ||
|| [crm.automation.trigger.delete](./crm-automation-trigger-delete.md) | Deletes an application trigger ||
|#