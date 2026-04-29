# Interactivity in Applications: Overview of Scenarios and Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Interactivity allows the application to function without reloading the page: it shows changes in the interface, updates status, and sends push notifications to users.

This is essential so that users can immediately see changes without having to refresh the page manually. For example, an employee can initiate a document check and see the new status upon completion. If the employee is away from their computer, the application can send a push notification about the result to their phone.

{% note warning %}

In the on-premise version, interactivity in applications is available starting from the [version](../cloud-and-on-premise/on-premise/versions.md) of the Push&Pull module 18.5.500. If channels or methods are unavailable, first check the module version.

{% endnote %}

> Quick Navigation: [Push&Pull Methods](#rest-methods)

## How to Choose a Scenario

In Bitrix24, there are two main scenarios for working with interactivity: the application can receive events through the built-in Push&Pull in the browser or through a custom client. The choice depends on where the application should operate and how it will receive events.

**Push&Pull in the Browser.** This is suitable if the application is open in the Bitrix24 interface and needs to receive events in the browser. For example, a user is working on a document in the application. The server-side completes the check and immediately sends an event to the browser so that the new status appears on the screen. [Push&Pull in the Browser](./push-and-pull-in-browser.md).

**Custom Push&Pull Client.** This is used if the built-in browser functionality is insufficient for the application. This option is appropriate if the application operates separately from the Bitrix24 interface and needs to maintain a connection with real-time servers, receive events, and recover the connection in case of a drop. [Custom Push&Pull Client](./custom-push-and-pull-client.md).

## Relationships with Other Sections

**Applications.** Installation and launch scenarios are described in the section [Application Installation Options in Bitrix24](../app-installation/index.md).

**Push&Pull.** Connection parameters to channels and event sending are described in the section [Push&Pull](./push-and-pull/index.md).

## Push&Pull Methods {#rest-methods}

Both scenarios utilize the application's REST methods — for sending events to the channel, obtaining connection parameters, and push notifications to mobile devices.

> Scope: [`pull`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [pull.application.config.get](./push-and-pull/pull-application-config-get.md) | Returns the connection configuration to Push&Pull servers, channels, and the application API ||
|| [pull.application.event.add](./push-and-pull/pull-application-event-add.md) | Sends an event to the application channel ||
|| [pull.application.push.add](./push-and-pull/pull-application-push-add.md) | Sends a push notification to a mobile device ||
|#