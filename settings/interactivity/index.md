# Interactivity in Applications: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Interactivity allows the application to function without reloading the page: it shows changes in the interface, updates status, and sends push notifications to users.

This is essential so that users can immediately see changes without having to refresh the page manually. For example, an employee can initiate a document check and see the new status upon completion. If the employee is away from their computer, the application can send a push notification about the result to their phone.

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Interactivity in Applications](https://helpdesk.bitrix24.com/courses/index.php?COURSE_ID=268&LESSON_ID=26036)

{% note info "" %}

Interactivity methods work only in the context of an [application](../app-installation/index.md): the request is executed with the application OAuth token and the `pull` scope. A webhook does not create this context.

{% endnote %}

## How to Choose a Scenario

In Bitrix24, there are two main scenarios for working with interactivity: the application can receive events through the built-in Push&Pull in the browser or through a custom client. The choice depends on where the application should operate and how it will receive events.

**Push&Pull in the Browser.** This is suitable if the application is open in the Bitrix24 interface and needs to receive events in the browser. For example, a user is working on a document in the application. The server-side completes the check and immediately sends an event to the browser so that the new status appears on the screen. The article [Push&Pull in the Browser](./push-and-pull-in-browser.md) describes how to connect the built-in client.

**Custom Push&Pull Client.** This is used if the built-in browser functionality is insufficient for the application. This option is appropriate if the application operates separately from the Bitrix24 interface and needs to maintain a connection with real-time servers, receive events, and recover the connection in case of a drop. The article [Custom Push&Pull Client](./custom-push-and-pull-client.md) describes the command format and how to handle connection drops.

## Channels and Commands

**Channel.** A queue of events on the Push&Pull servers. The method [pull.application.config.get](./push-and-pull/pull-application-config-get.md) returns the application channels: `shared` — the common channel of the application, `private` — the personal channel of the user. An event without the `USER_ID` parameter goes to the common channel, and with `USER_ID` — to the channels of the specified users.

**Command.** The value of the `COMMAND` parameter in the method [pull.application.event.add](./push-and-pull/pull-application-event-add.md). The client uses the command to distinguish event types and decide what to update in the interface.

**Event module.** The value of the `MODULE_ID` parameter. The client subscribes to the events of a specific module, `application` is used by default.

## Getting Started

1. Install the application in Bitrix24 — [Application Installation Options in Bitrix24](../app-installation/index.md).
2. Retrieve the connection parameters for the servers and channels using the method [pull.application.config.get](./push-and-pull/pull-application-config-get.md).
3. Connect the client to the application channel according to the selected scenario — [Push&Pull in the Browser](./push-and-pull-in-browser.md) or [Custom Push&Pull Client](./custom-push-and-pull-client.md).
4. Send an event to the channel using the method [pull.application.event.add](./push-and-pull/pull-application-event-add.md) and make sure the client has received it.
5. Send a push notification using the method [pull.application.push.add](./push-and-pull/pull-application-push-add.md) if the user has to learn about the event outside the Bitrix24 interface.

## Interaction with Other Objects

**Application.** Interactivity works within the channel of a specific application: events and push notifications are received only by the users of the application on whose behalf the request is made. The method [pull.application.config.get](./push-and-pull/pull-application-config-get.md) returns the channel ID and the server parameters. Installation and launch scenarios are described in the section [Application Installation Options in Bitrix24](../app-installation/index.md).

**User.** The `USER_ID` parameter in the methods [pull.application.event.add](./push-and-pull/pull-application-event-add.md) and [pull.application.push.add](./push-and-pull/pull-application-push-add.md) specifies the recipients — a single user or an array of IDs. Retrieve the user ID using the methods [user.get](../../api-reference/user/user-get.md) and [user.current](../../api-reference/user/user-current.md).

**Widgets.** To make the application receive events when its interface is closed, embed a background handler in the [PAGE_BACKGROUND_WORKER](../../api-reference/widgets/universal/background-worker.md) placement. The handler works without any user action and receives commands from the application channel.

**Push&Pull.** Channel connection parameters, the order of method calls, and related objects are described in the section [Push&Pull](./push-and-pull/index.md).

## Overview of Methods {#all-methods}

Both scenarios utilize the application methods — for retrieving connection parameters, sending events to the channel, and push notifications to mobile devices. Only a Bitrix24 administrator can send an event to another user's channel or a push notification.

> Scope: [`pull`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [pull.application.config.get](./push-and-pull/pull-application-config-get.md) | Returns the connection configuration to Push&Pull servers, channels, and the application API ||
|| [pull.application.event.add](./push-and-pull/pull-application-event-add.md) | Sends an event to the application channel ||
|| [pull.application.push.add](./push-and-pull/pull-application-push-add.md) | Sends a push notification to a mobile device ||
|#