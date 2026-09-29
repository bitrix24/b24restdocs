# App deletion

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

When an administrator uninstalls an application, Bitrix24 removes its event subscriptions, widgets, and other registrations the application works through. What happens to the application data depends on its type. Bitrix24 always deletes a [Local Application](../local-integrations/local-apps.md) together with its data. When uninstalling a [Mass-Market Application](./app-installation/mass-market-apps/index.md), the administrator decides whether to delete the data or retain it in case the application is reinstalled.

For example, if an administrator uninstalls a mass-market payment system application without selecting data deletion, the application stops working, but its payment system remains in Bitrix24.

## How an Application Learns About Uninstallation

An application learns about its uninstallation from the [OnAppUninstall](../api-reference/common/events/on-app-uninstall.md) event if it has subscribed to it in advance. A mass-market application needs this event to stop sending requests to that Bitrix24 in time: after uninstallation, the application loses access to the REST API. For example, Bitrix24 responds to a call with the old token with the `APPLICATION_NOT_FOUND` error.

In the `CLEAN` field of the event, Bitrix24 passes the data cleanup flag: for a mass-market application, it depends on the administrator's choice; for a local application, cleanup is always enabled.

Before the application stops operating in response to this event, verify that the event came from Bitrix24: compare the `application_token` from the event with the current value that the application retains at installation and updates on the [ONAPPUPDATE](../api-reference/common/events/on-app-update.md) event. How to do this is described in [{#T}](../api-reference/events/safe-event-handlers.md).

## What Is Always Deleted {#always}

Regardless of the application type and the data cleanup choice, Bitrix24 deletes:

- subscriptions to [Events](../api-reference/events/index.md)
- [Widgets](../api-reference/widgets/index.md) and [Custom Field Types](../api-reference/widgets/user-field/index.md)
- [Automation Rules](../api-reference/bizproc/bizproc-robot/index.md) and [Workflow Actions](../api-reference/bizproc/bizproc-activity/index.md)
- [Chatbots](../api-reference/chat-bots/index.md)
- [Open Channels connectors](../api-reference/imopenlines/imconnector/index.md)
- [Message Providers](../api-reference/messageservice/index.md)
- [External Telephony Lines](../api-reference/telephony/index.md#external-telephony)

If a mass-market application has a [System User](./system-user.md), Bitrix24 deactivates it regardless of the data cleanup choice.

## What Depends on Data Cleanup

When a mass-market application is uninstalled, Bitrix24 offers the *Delete application settings and data* option. For payment systems and delivery services, cleanup applies to the application's handlers and all objects that use them.

#|
|| **Application data and objects** | **Option not selected** | **Option selected or the application is local** ||
|| [Data Stores](../api-reference/entity/index.md) | Remain | Deleted ||
|| [Application settings](../api-reference/common/settings/index.md) | Remain | Deleted ||
|| [User settings](../api-reference/common/settings/index.md) | Remain | Deleted only for the user who uninstalled the application. Remain for other users ||
|| [Payment systems](../api-reference/pay-system/index.md) | Remain | Deleted ||
|| [Delivery services](../api-reference/sale/delivery/index.md) | Remain | Deleted ||
|#

## What Remains After Uninstallation

Uninstalling an application does not affect the objects it created in Bitrix24 tools:

- custom fields and their values
- [CRM Activity Types](../api-reference/crm/timeline/activities/types/index.md)
- [Smart Processes](../api-reference/crm/universal/user-defined-object-types/index.md)

Fields of the application's own type remain but are no longer displayed in the card. For details, see the [userfieldtype.delete](../api-reference/widgets/user-field/userfieldtype-delete.md) method description.

## Reinstallation

Uninstallation cannot be undone.

A mass-market application can be reinstalled on the same Bitrix24. If the administrator retained the data during uninstallation, the application regains access to its data stores and settings. The application must re-create event subscriptions, widgets, and other registrations from the [What Is Always Deleted](#always) section.

A local application can only be created again after deletion. The new application receives a new `client_id` and `client_secret` pair for [OAuth Authorization](./oauth/index.md), even if the handler URLs match the old ones.

## Continue Learning

- [{#T}](../api-reference/common/events/on-app-uninstall.md)
- [{#T}](system-user.md)
