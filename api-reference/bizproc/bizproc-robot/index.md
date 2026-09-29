# Application Automation Rules: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Application Automation Rules add a step to Bitrix24 automation that invokes an application handler. This step accepts input parameters, can wait for a response from an external system, and return output values to the workflow. For example, an Automation Rule at a deal stage can pass an order to an external accounting system and return the order number for the next automation steps.

In the REST API, Application Automation Rules and [application actions](../bizproc-activity/index.md) are similar in model. Each type has separate methods for registration, updating, deletion, and listing. A common execution mechanism is used within the platform.

Automation Rules can be utilized in CRM automation, workflows, and Smart scripts. Therefore, for new development, it is recommended to choose Automation Rules, while application actions should primarily be used to support existing integrations.

> Quick navigation: [all methods](#all-methods)
>
> User documentation:
> - [Automation rules in CRM](https://helpdesk.bitrix24.com/open/24545552/)
> - [Smart scripts in CRM](https://helpdesk.bitrix24.com/open/25071006/)

## What Tasks Do Application Automation Rules Solve

- Perform external actions in CRM automation and workflows
- Return computed data to the process for subsequent steps
- Restrict availability based on document type and Bitrix24 edition
- Write messages to the process log during the execution of the script

## How Result Waiting Works {#handler}

If the Automation Rule needs to wait for a response from an external system, specify `RETURN_PROPERTIES` and `USE_SUBSCRIPTION = 'Y'` when registering or updating via [bizproc.robot.add](./bizproc-robot-add.md) and [bizproc.robot.update](./bizproc-robot-update.md).

The script then operates as follows:

1. The Automation Rule is triggered in the automation and sends data to the application handler `HANDLER`
2. The application receives a unique key `EVENT_TOKEN` and calculates the result
3. The application returns values to the process using the method [bizproc.event.send](./bizproc-event-send.md)
4. If necessary, the application adds a note to the process log via `LOG_MESSAGE`

Bitrix24 sends the request to `HANDLER` with the POST method through the queue server. The data arrives in the `application/x-www-form-urlencoded` format; the example shows it in JSON format:

```json
{
    "workflow_id": "6ab5bcfde8e8b0.39728274",
    "code": "test_robot",
    "document_id": [
        "crm",
        "CCrmDocumentDeal",
        "DEAL_17"
    ],
    "document_type": [
        "crm",
        "CCrmDocumentDeal",
        "DEAL"
    ],
    "event_token": "6ab5bcfde8e8b0.39728274|A1|rKrsZ31xjG1jMJL2iEjmSTP3h2K10FiI.e02ccd45c3520894e790a7b58f4f04932f5e9805a03cdc041b9c5dc5aff49653",
    "properties": {
        "text": "Remind the client about the meeting"
    },
    "use_subscription": "Y",
    "timeout_duration": "0",
    "ts": "1790295294",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "refresh_token": "t5o5dbkqauh5cz11es8bg83djqda41ku",
        "domain": "some-domain.bitrix24.com",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "application_token": "51856fefc120afa4b628cc82d3935cce"
    }
}
```

Key fields in the request:

- `event_token` — the key of this Automation Rule run. It is passed to the [bizproc.event.send](./bizproc-event-send.md) method
- `properties` — values of the input parameters from `PROPERTIES` that were set in the Automation Rule settings
- `document_id` — the document for which the Automation Rule was started. In the example, it is the deal with ID 17
- `timeout_duration` — how many seconds the process will wait for a response. The value `0` means there is no time limit
- `auth` — the `access_token` and `refresh_token` tokens of the user on whose behalf the Automation Rule runs. This is the user from the `AUTH_USER_ID` parameter, and an administrator can select a different user in the Automation Rule settings. The other `auth` keys are described in the [Events](../../events/index.md#auth) article

To return the result, the application calls [bizproc.event.send](./bizproc-event-send.md) with the token from the request. In `RETURN_VALUES`, pass the values with keys from `RETURN_PROPERTIES`; in `LOG_MESSAGE`, pass the text for the process log. In the example, the output parameter `outputString` of the `string` type was declared when the Automation Rule was registered:

```json
{
    "EVENT_TOKEN": "6ab5bcfde8e8b0.39728274|A1|rKrsZ31xjG1jMJL2iEjmSTP3h2K10FiI.e02ccd45c3520894e790a7b58f4f04932f5e9805a03cdc041b9c5dc5aff49653",
    "RETURN_VALUES": {
        "outputString": "ORDER-1024"
    },
    "LOG_MESSAGE": "Order sent to the accounting system"
}
```

If the step with this token is still waiting for a response, the Automation Rule completes, and the next automation steps can use the `outputString` value. Bitrix24 does not retain keys that are not in `RETURN_PROPERTIES`.

{% note warning "" %}

A `true` response from the [bizproc.event.send](./bizproc-event-send.md) method does not confirm that the process accepted the result. The method also returns `true` for the token of an Automation Rule that has already completed or is not waiting for a response. In this case, the process does not change.

{% endnote %}

{% note tip "User Documentation" %}

- [Workflow test log](https://helpdesk.bitrix24.com/open/22095380/)

{% endnote %}

## How to Get Started

1. Prepare a handler — an application page with an external URL to which Bitrix24 will send the Automation Rule data
2. Register the Automation Rule with the [bizproc.robot.add](./bizproc-robot-add.md) method. Pass the code in `CODE`, the name in `NAME`, and the handler URL in `HANDLER`
3. Add the Automation Rule to a stage in CRM automation or to a workflow template and start the script
4. Receive the request in the handler and, if the Automation Rule waits for a response, return the result with the [bizproc.event.send](./bizproc-event-send.md) method. The data exchange is described in the [How Result Waiting Works](#handler) section
5. Check the application's Automation Rules with the [bizproc.robot.list](./bizproc-robot-list.md) method, change them with the [bizproc.robot.update](./bizproc-robot-update.md) method, and delete the ones you no longer need with the [bizproc.robot.delete](./bizproc-robot-delete.md) method

## Important Considerations

- The methods for managing Automation Rules [bizproc.robot.add](./bizproc-robot-add.md), [bizproc.robot.update](./bizproc-robot-update.md), [bizproc.robot.list](./bizproc-robot-list.md), [bizproc.robot.delete](./bizproc-robot-delete.md) work only in the application context and only on behalf of an administrator. When called through a webhook, they return the `ACCESS_DENIED` error with the message `Access denied! Application context required`
- The [bizproc.event.send](./bizproc-event-send.md) method does not require the application context: Bitrix24 validates the `event_token` token. With an invalid token, the method returns the `ACCESS_DENIED` error

## Relationships with Other Objects

**Document Types.** The `FILTER` parameter of the [bizproc.robot.add](./bizproc-robot-add.md) method sets where the Automation Rule is shown: for example, only in deal automation. The `DOCUMENT_TYPE` parameter sets the document type that Bitrix24 uses to determine the field types for `PROPERTIES` and `RETURN_PROPERTIES`.

**Application Actions.** Automation Rules and [Actions](../bizproc-activity/index.md) of an application receive the same request in the handler and return the result with the same [bizproc.event.send](./bizproc-event-send.md) method. They share a common set of codes: if the application already has an action with the same `CODE`, the Automation Rule is not registered — the method returns the `ERROR_ACTIVITY_ALREADY_INSTALLED` error.

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
