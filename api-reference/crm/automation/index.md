# CRM Automation: Overview of Methods

The methods in this section allow external events to be transmitted to CRM automation through triggers. A configured trigger can move an object to another stage or status if its conditions are met.

A trigger is launched in one of two ways. A pre-configured webhook trigger is set up in the Bitrix24 interface and called using the [crm.automation.trigger](./crm-automation-trigger.md) method. A custom trigger is registered and launched by the application through the [crm.automation.trigger.*](./triggers/index.md) group of methods.

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation:
> - [Triggers in CRM](https://helpdesk.bitrix24.com/open/24564704/)
> - [Sales Automation in CRM](https://helpdesk.bitrix24.com/open/24653112/)

## Getting Started

**If the webhook trigger is already configured in Bitrix24:**

1. Retrieve the `code` value from the URL of the "Track Inbound Webhook" trigger in the CRM automation settings.
2. Construct the `target` for the target object, for example, `DEAL_25`.
3. Call [crm.automation.trigger](./crm-automation-trigger.md), passing the `target` and `code`.
4. Check the object's stage using the [crm.&lt;type&gt;.get](../rest-v3/items/crm-entity-get-rest-v3.md) method.

The method returns `true` as soon as it has checked the permissions for the object and passed the call to automation. A `true` response does not mean the object has changed its stage: the call has no effect if, for example, there is no matching trigger, no object with this ID exists, or the trigger conditions are not met.

**If you need to manage triggers from the application:**

The complete procedure is outlined in the [CRM Automation Triggers](./triggers/index.md) section: registration, binding to a stage, and execution. Application trigger methods are called by an administrator and only in the application context: they do not work via an inbound webhook.

## Interaction with Other Objects

**CRM Automation.** In both scenarios, the trigger must be pre-bound to a stage or status in the Bitrix24 automation settings.

**CRM Objects.** The trigger is initiated for a specific CRM object. In the webhook scenario, the `target` parameter is passed in the format [`TYPENAME_ID`](../data-types.md#object_type): the CRM object type and its numeric ID. The ID is retrieved using the [crm.&lt;type&gt;.list](../rest-v3/items/crm-entity-list-rest-v3.md) method. In the application scenario, the object type `OWNER_TYPE_ID` is obtained using the [crm.enum.ownertype](../auxiliary/enum/crm-enum-owner-type.md) method, and the object identifier `OWNER_ID` is retrieved using the [crm.&lt;type&gt;.list](../rest-v3/items/crm-entity-list-rest-v3.md) method.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Launching a Configured Trigger

#| 
|| **Method** | **Description** ||
|| [crm.automation.trigger](./crm-automation-trigger.md) | Launches the webhook trigger configured in CRM automation ||
|#

### Application Triggers

#| 
|| **Method** | **Description** ||
|| [crm.automation.trigger.add](./triggers/crm-automation-trigger-add.md) | Adds an application trigger ||
|| [crm.automation.trigger.list](./triggers/crm-automation-trigger-list.md) | Retrieves the list of application triggers ||
|| [crm.automation.trigger.delete](./triggers/crm-automation-trigger-delete.md) | Deletes an application trigger ||
|| [crm.automation.trigger.execute](./triggers/crm-automation-trigger-execute.md) | Executes an application trigger for a CRM object ||
|#