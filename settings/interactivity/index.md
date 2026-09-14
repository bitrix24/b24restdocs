# Interactivity in Applications: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Interactivity helps the application work without reloading the page: it shows changes in the interface, updates the status, and sends push notifications. For example, an employee starts a document check in the application and sees the result on the screen as soon as it completes. If the employee is away from the computer at that moment, the application sends a push notification to their phone.

The methods of this section deliver only the application's own events. The data flow is one-way: the server side of the application publishes an event to its own channel with `pull.application.event.add`, and the application client reads the event from the channel over websocket or long polling. The channels and servers for this exchange are provided by the Push&Pull service.

To receive notifications about data changes in Bitrix24 — a new deal or an updated task — use [Bitrix24 events](../../api-reference/events/index.md).

{% note info "" %}

The methods of this section work only in the context of an [application](../app-installation/index.md). The request is executed with the application OAuth token and the `pull` scope, and a webhook does not create such a context — the methods return the `WRONG_AUTH_TYPE` error.

{% endnote %}

> Quick Navigation: [All Methods](#all-methods)

## How to Receive Events

The way events are received depends on where the application runs.

#|
|| **If the application** | **Open the article** ||
|| Is open in the Bitrix24 interface and updates its own screen | [Push&Pull in the Browser](./push-and-pull-in-browser.md) — the built-in `BX.PullClient` ||
|| Runs outside the interface and maintains the connection itself | [Custom Push&Pull Client](./custom-push-and-pull-client.md) — connecting to the server directly ||
|| Has to receive events while its page is closed and Bitrix24 is open in the browser | [PAGE_BACKGROUND_WORKER](../../api-reference/widgets/universal/background-worker.md) — a background placement rather than a Push&Pull client ||
|#

## Terms of the Section

**Channel.** A queue of events on the Push&Pull servers. An application has two channels: `shared` — common to all users of the application, and `private` — for a single user.

**Command.** A label for the event type in the channel. In REST it is the `COMMAND` parameter of the [pull.application.event.add](./pull-application-event-add.md) method, and on the client side it is the `command` field of the `subscribe` subscription.

**Event module.** The area the client subscribes to. In REST it is the `MODULE_ID` parameter, and on the client side it is the `moduleId` field of the `subscribe` subscription.

## Getting Started

1. Install the application in Bitrix24 using one of the [installation options](../app-installation/index.md)
2. Retrieve the server parameters and the application channels with the [pull.application.config.get](./pull-application-config-get.md) method
3. Connect the [built-in client in the browser](./push-and-pull-in-browser.md) or a [custom client](./custom-push-and-pull-client.md) to the channels
4. Send an event to the channel with the [pull.application.event.add](./pull-application-event-add.md) method to update the application interface
5. Send a push notification with the [pull.application.push.add](./pull-application-push-add.md) method to notify the user outside the Bitrix24 interface

## Limits

Lifetimes of the objects of this section:

- application channel — 12 hours, the `end` field in the response of [pull.application.config.get](./pull-application-config-get.md#result)
- connection configuration — 24 hours in Bitrix24 cloud, in the self-hosted version it depends on the server settings, the `exp` field in the response of [pull.application.config.get](./pull-application-config-get.md#result)
- event in the channel — 24 hours, [pull.application.event.add](./pull-application-event-add.md)

The size limits of an event and of a push notification are different — they are described in the parameters of [pull.application.event.add](./pull-application-event-add.md) and [pull.application.push.add](./pull-application-push-add.md).

## Interaction with Other Objects

Interactivity is related to the application, the users, and the Bitrix24 mobile app.

**Application.** Push&Pull channels are bound to a specific application: an event is received only by the users of the application on whose behalf the request is executed. The [pull.application.config.get](./pull-application-config-get.md) method returns the application channel IDs in the `channels` object.

**User.** The `USER_ID` parameter specifies the recipients of an event and of a push notification. Retrieve the user ID with the [user.get](../../api-reference/user/user-get.md) and [user.current](../../api-reference/user/user-current.md) methods.

**Mobile app.** Push notifications do not go to a channel: they are delivered not by Push&Pull but by the Bitrix24 mobile app — with the [pull.application.push.add](./pull-application-push-add.md) method.

## Overview of Methods {#all-methods}

> Scope: [`pull`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: depends on the method

Interactivity in applications is served by a single family of methods — `pull.application.*`.

Any user authorized in the application can retrieve the configuration and send an event to the common channel or to their own personal channel. A Bitrix24 administrator can additionally send an event to another user's channel, as well as a push notification.

#|
|| **Method** | **Description** ||
|| [pull.application.config.get](./pull-application-config-get.md) | Returns the parameters of the Push&Pull servers, the application channels, and the protocol revisions ||
|| [pull.application.event.add](./pull-application-event-add.md) | Sends an event to the application channel ||
|| [pull.application.push.add](./pull-application-push-add.md) | Sends a push notification to the application users ||
|#
