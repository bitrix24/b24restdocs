# Application Actions: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An application action is a workflow template step performed by the application. The template author adds it in the workflow designer alongside the standard actions. When the workflow reaches this step, Bitrix24 sends data to the application handler. The application processes the data and, if needed, returns a result to the workflow. For example, an action can check the company from a deal in an external service and return its status.

[Application Automation Rules](../bizproc-robot/index.md) work in a similar way. An Automation rule is available both in CRM automation and in the workflow designer, while an action is available only in the designer. Therefore, choose Automation rules for new development; application actions are primarily needed to support existing integrations.

> Quick navigation: [all methods](#all-methods)
>
> User documentation:
> - [How to Use the Workflow Designer](https://helpdesk.bitrix24.com/open/23379262/)
> - [Workflow Test Log: How to Enable Event Logging](https://helpdesk.bitrix24.com/open/22095380/)

## Scenarios Suitable for Application Actions

- Add a step to the workflow template that calls an external application service
- Pass parameters from the workflow document to the action and return output data to the process
- Log intermediate messages in the workflow
- Restrict the availability of the action based on document type and Bitrix24 edition
- Fine-tune the logic within an already functioning template without migrating to another type of automation

## Getting Started

1. Prepare a handler — an application page with an external URL to which Bitrix24 will send the action data
2. Register the action with the [bizproc.activity.add](./bizproc-activity-add.md) method. Pass the code in `CODE`, the name in `NAME`, and the handler URL in `HANDLER`. Describe the input parameters in `PROPERTIES` and the action results in `RETURN_PROPERTIES`
3. To make the workflow wait for a response from the application, pass `USE_SUBSCRIPTION` with the value `Y`. If the parameter is not passed, the template author decides in the action settings whether to wait for a response
4. If necessary, restrict where the action is shown with the `FILTER` parameter. Use `INCLUDE` and `EXCLUDE` rules, for example `b24` for cloud Bitrix24 and `box` for on-premise Bitrix24
5. Add the action to a template in the workflow designer, then start the workflow manually or with the [bizproc.workflow.start](../bizproc-workflow-start.md) method. The application can also upload a template containing the action from a `.bpt` file with the [bizproc.workflow.template.add](../template/bizproc-workflow-template-add.md) method
6. Receive the data in the handler and return the result with the [bizproc.event.send](../bizproc-robot/bizproc-event-send.md) method. The data exchange is described in the [How an Action Exchanges Data with the Application](#handler) section
7. Check the application's actions with the [bizproc.activity.list](./bizproc-activity-list.md) method, change them with the [bizproc.activity.update](./bizproc-activity-update.md) method, and delete the ones you no longer need with the [bizproc.activity.delete](./bizproc-activity-delete.md) method

## How an Action Exchanges Data with the Application {#handler}

When the workflow reaches the action, Bitrix24 sends a POST request to the `HANDLER` URL through the queue server. The request arrives with the content type `application/x-www-form-urlencoded`; the example shows its structure in JSON format.

```json
{
    "workflow_id": "6ab5bcfde8e8b0.39728274",
    "code": "md5_action",
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
        "inputString": "Test"
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

- `event_token` — the key of this action run. It is passed to the [bizproc.event.send](../bizproc-robot/bizproc-event-send.md) and [bizproc.activity.log](./bizproc-activity-log.md) methods
- `properties` — values of the input parameters from `PROPERTIES` that the template author set in the action settings
- `document_id` — the document for which the workflow was started. In the example, it is the deal with ID 17
- `timeout_duration` — how many seconds the workflow will wait for a response. The value `0` means there is no time limit
- `auth` — authorization data with the `access_token` and `refresh_token` tokens of the user from `AUTH_USER_ID` or the user selected by the template author in the action settings. The other `auth` keys are described in the [Events](../../events/index.md#auth) article

To return the result, call [bizproc.event.send](../bizproc-robot/bizproc-event-send.md) with the token from the request. In `RETURN_VALUES`, pass the values with keys from `RETURN_PROPERTIES`; in `LOG_MESSAGE`, pass the text for the workflow log:

```json
{
    "EVENT_TOKEN": "6ab5bcfde8e8b0.39728274|A1|rKrsZ31xjG1jMJL2iEjmSTP3h2K10FiI.e02ccd45c3520894e790a7b58f4f04932f5e9805a03cdc041b9c5dc5aff49653",
    "RETURN_VALUES": {
        "outputString": "b4e5c9a2f7d3"
    },
    "LOG_MESSAGE": "Result received"
}
```

If the step with this token is waiting for a response, it completes, and the workflow moves on. The `outputString` value becomes available to the next actions in the template. Bitrix24 does not retain keys that are not in `RETURN_PROPERTIES`.

While the application processes the data, it can write intermediate messages to the log with the [bizproc.activity.log](./bizproc-activity-log.md) method using the same token. The step keeps waiting for a response.

{% note warning "" %}

A `true` response from the [bizproc.event.send](../bizproc-robot/bizproc-event-send.md) method does not confirm that the workflow accepted the result. The method also returns `true` for the token of a step that has already completed, for example, when the response is sent again. In this case, the workflow does not change.

{% endnote %}

For a step-by-step example with handler code in JS, PHP, and Python, see the tutorial [How to Create Your Own Workflow Action](../../../tutorials/bizproc/how-to-create-custom-activity.md).

## Important Considerations

- The methods [bizproc.activity.add](./bizproc-activity-add.md), [bizproc.activity.update](./bizproc-activity-update.md), [bizproc.activity.list](./bizproc-activity-list.md), and [bizproc.activity.delete](./bizproc-activity-delete.md) work only in the application context and only on behalf of an administrator. When called through a webhook, they return the `ACCESS_DENIED` error with the message `Access denied! Application context required`
- The methods [bizproc.event.send](../bizproc-robot/bizproc-event-send.md) and [bizproc.activity.log](./bizproc-activity-log.md) do not require the application context: Bitrix24 validates the `event_token` token. With an invalid token, they return the `ACCESS_DENIED` error

## Relationships with Other Objects

Application actions are related to workflow templates, document types, and application Automation rules.

**Workflow Templates.** After registration with the [bizproc.activity.add](./bizproc-activity-add.md) method, the action appears in the workflow designer, and the template author can add it as a regular step.

**Document Type.** The `DOCUMENT_TYPE` parameter of the [bizproc.activity.add](./bizproc-activity-add.md) method sets the document type that Bitrix24 uses to determine the field types for `PROPERTIES` and `RETURN_PROPERTIES`, such as a CRM address field. The `FILTER` parameter sets where the action is shown: for example, only in deal templates.

**Automation Rules.** Actions and [Automation Rules](../bizproc-robot/index.md) of the same application share a common set of codes. If the application already has an Automation rule with the same `CODE`, the action is not registered: the method returns the `ERROR_ACTIVITY_ALREADY_INSTALLED` error.

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
|| [bizproc.activity.log](./bizproc-activity-log.md) | Writes a message to the workflow log ||
|| [bizproc.event.send](../bizproc-robot/bizproc-event-send.md) | Returns the action result to the workflow ||
|#
