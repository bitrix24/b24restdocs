# CRM Automation Triggers: Overview of Methods

CRM automation triggers help the application transmit external events to the CRM. If a trigger is configured for an object, the event can move it to the appropriate stage or status.

The group of methods `crm.automation.trigger.*` allows you to register an application trigger, retrieve a list of registered triggers, send an event to CRM automation, and delete an unnecessary trigger.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Triggers in CRM](https://helpdesk.bitrix24.com/open/24473054/)

{% note info "" %}

The methods in this section only work in the context of an [application](../../../../settings/app-installation/index.md).

{% endnote %}

## Getting Started

1. Register a trigger using the [crm.automation.trigger.add](./crm-automation-trigger-add.md) method.

2. Configure the application trigger using the methods in the [CRM Automation](../index.md) section for the desired object, stage, or status.

3. If necessary, retrieve the list of application triggers using the [crm.automation.trigger.list](./crm-automation-trigger-list.md) method.

4. Prepare the CRM object identifiers: `OWNER_TYPE_ID` and `OWNER_ID`.

5. Execute the trigger using the [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) method.

6. Delete the unnecessary trigger using the [crm.automation.trigger.delete](./crm-automation-trigger-delete.md) method.

## Key Parameters

**CODE.** The identifier of the trigger within the application. The application assigns this when registering the trigger using the [crm.automation.trigger.add](./crm-automation-trigger-add.md) method. This identifier is then used in the [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) and [crm.automation.trigger.delete](./crm-automation-trigger-delete.md) methods.

If an existing `CODE` is passed to the `add` method, it will update the `NAME` parameter of the trigger.

**OWNER_TYPE_ID.** The identifier of the CRM object type. Used in the [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) method. The object type must support CRM automation.

**OWNER_ID.** The identifier of a specific CRM object. Used in the [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) method.

## Important Considerations

- The [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) method sends an event to CRM automation but does not confirm the change of stage or status.

- The change of stage or status will only occur if a trigger for the desired object has already been configured with the same `CODE` in the current application.

## Relationship with Other Objects

**CRM Automation.** After registering the application trigger, it can be selected using the methods in the [CRM Automation](../index.md) section and linked to the desired stage or status.

**CRM Objects.** In the [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) method, the parameters `OWNER_TYPE_ID` and `OWNER_ID` define the type of CRM object and the specific record for triggering the action. The value of `OWNER_TYPE_ID` can be obtained using the [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) method. The `OWNER_ID` identifier is obtained using methods from the CRM section that corresponds to the desired object, such as [crm.lead.*](../../leads/index.md), [crm.deal.*](../../deals/index.md), [crm.quote.*](../../quote/index.md), or [crm.item.*](../../universal/index.md).

## Overview of Methods  {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: an administrator with access to CRM in the context of the application

#| 
|| **Method** | **Description** ||
|| [crm.automation.trigger.add](./crm-automation-trigger-add.md) | Adds an application trigger ||
|| [crm.automation.trigger.list](./crm-automation-trigger-list.md) | Retrieves a list of application triggers ||
|| [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) | Executes the trigger for a CRM object ||
|| [crm.automation.trigger.delete](./crm-automation-trigger-delete.md) | Deletes the trigger ||
|#