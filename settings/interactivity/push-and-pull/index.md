# Push&Pull: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Push&Pull methods assist applications in transmitting and receiving data in real-time. They enable the application to obtain connection parameters, send events to a channel, and push notifications to the application user.

The group of methods `pull.application.*` helps update the interface without reloading the page and sends notifications to mobile devices.

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Interactivity in Applications](https://helpdesk.bitrix24.com/courses/index.php?COURSE_ID=268&LESSON_ID=26036)

{% note info "" %}

The methods in this section work only in the context of the [application](../../app-installation/index.md).

{% endnote %}

## How to Choose a Scenario

The methods in this section are used in two scenarios:

**Update the interface of an open application.** If the application is already open in the Bitrix24 browser and needs to receive events without reloading the page, refer to the article [Push&Pull in the Browser](../push-and-pull-in-browser.md).

**Connect your own client to Push&Pull servers.** If you are creating your own client and want to work directly with the Push&Pull servers, refer to the article [Custom Push&Pull Client](../custom-push-and-pull-client.md).

## Getting Started

1. Obtain the application connection parameters using the method [pull.application.config.get](./pull-application-config-get.md).
2. Connect the application to Push&Pull using the server and channel data from the response of [pull.application.config.get](./pull-application-config-get.md).
3. If you need to update the interface of the open application, send an event to the channel using the method [pull.application.event.add](./pull-application-event-add.md).
4. If you need to notify the user on a mobile device, use the method [pull.application.push.add](./pull-application-push-add.md).

## Interaction with Other Objects

**User.** In the method [pull.application.event.add](./pull-application-event-add.md), the parameter `USER_ID` specifies the user or list of users to whose channels the event is sent. In the method [pull.application.push.add](./pull-application-push-add.md), the parameter `USER_ID` specifies the user or list of users who will receive the push notification. You can obtain the user ID using the methods [user.get](../../../api-reference/user/user-get.md) and [user.current](../../../api-reference/user/user-current.md).

## Overview of Methods {#all-methods}

> Scope: [`pull`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [pull.application.config.get](./pull-application-config-get.md) | Returns the connection configuration to the Push&Pull servers for the current application ||
|| [pull.application.event.add](./pull-application-event-add.md) | Sends an event to the application channel ||
|| [pull.application.push.add](./pull-application-push-add.md) | Sends a push notification to a mobile device within the application ||
|#