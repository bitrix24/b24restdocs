# CRM Automation: Overview of Methods

The methods in this section allow external events to be transmitted to CRM automation through triggers. When a trigger is configured for an object, it transitions the object to another stage or status.

This section includes two scenarios. In the first, a pre-configured webhook trigger is initiated using the [crm.automation.trigger](./crm-automation-trigger.md) method — the trigger is set up in the Bitrix24 interface and then called via the API. In the second scenario, the application registers the trigger and manages it through the methods in the [CRM Automation Triggers](./triggers/index.md) section.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation:
> - [Triggers in CRM](https://helpdesk.bitrix24.com/open/24564704/)
> - [Sales Automation in CRM](https://helpdesk.bitrix24.com/open/24653112/)

## Getting Started

**If the webhook trigger is already configured in Bitrix24:**

1. Retrieve the `code` value from the URL of the "Track Incoming Webhook" trigger in the CRM automation settings.
2. Construct the `target` for the target object, for example, `DEAL_25`.
3. Call [crm.automation.trigger](./crm-automation-trigger.md), passing the `target` and `code`.
4. If necessary, check the result using the [crm.item.get](../universal/crm-item-get.md) method.

**If you need to manage triggers from the application:**

The complete procedure is outlined in the [CRM Automation Triggers](./triggers/index.md) section: registration, binding to a stage, and execution.

## Interaction with Other Objects

**CRM Automation.** In both scenarios, the trigger must be pre-bound to a stage or status in the Bitrix24 automation settings.

**CRM Objects.** The trigger is initiated for a specific CRM object. In the webhook scenario, the `target` parameter passes the string identifier of the object in the format [`TYPENAME_ID`](../../data-types.md), for example, `DEAL_25`, where `DEAL` is the CRM object type and `25` is its numeric identifier from [crm.item.list](../universal/crm-item-list.md). In the application scenario, the object type `OWNER_TYPE_ID` is obtained using the [crm.enum.ownertype](../auxiliary/enum/crm-enum-owner-type.md) method, and the object identifier `OWNER_ID` is retrieved using the [crm.item.list](../universal/crm-item-list.md) method.

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
|| [crm.automation.trigger.execute](./triggers/crm-automation-trigger-execute.md) | Executes the trigger for a CRM object ||
|| [crm.automation.trigger.delete](./triggers/crm-automation-trigger-delete.md) | Deletes an application trigger ||
|#