# Overview of Events When Working with the Application

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in near real-time: receiving notifications about installation, updates, deletions, and payments of the application, adding users, and the administrator's decision regarding requests for access to methods requiring confirmation.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../events/index.md).

> Quick navigation: [all events](#all-events)

{% note info "" %}

The events section works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}

## How to Receive Events

You can subscribe to application events through the [application](../../../settings/app-installation/index.md) and the [event.bind](../../events/event-bind.md) method.

To receive events:

- install the application and specify the public URL of the handler
- if the application requests access to methods requiring confirmation, handle the [onAppMethodConfirm](./on-app-method-confirm.md) event

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../_includes/events-index.md) %}

## Interaction with Other Objects

**User.** Data from the [onAppUserAdd](./on-user-add.md) event can be used together with the [user.get](../../user/user-get.md) method if additional information about the user is needed after registration or to configure access.

## Overview of Events {#all-events}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

#| 
|| **Event** | **Description** ||
|| [onAppInstall](./on-app-install.md) | When the application is successfully installed ||
|| [onAppUpdate](./on-app-update.md) | When the application is updated ||
|| [onAppUninstall](./on-app-uninstall.md) | When the application is uninstalled ||
|| [onAppMethodConfirm](./on-app-method-confirm.md) | When receiving the administrator's decision regarding a request for access to methods requiring confirmation ||
|| [onAppPayment](./on-app-payment.md) | When the application is paid for ||
|| [onAppUserAdd](./on-user-add.md) | When a user is added to Bitrix24 ||
|#