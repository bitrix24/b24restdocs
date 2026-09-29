# Overview of Events When Working with the Application

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in near real-time: receiving notifications about the application lifecycle — installation, system user creation, updates, deletion, and payment — about the administrator's decision on methods requiring confirmation, and about adding a user.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to the onUserAdd event through:

- an [outbound webhook](../../../local-integrations/local-webhooks.md)
- the [application](../../../settings/app-installation/index.md) and the [event.bind](../../events/event-bind.md) method

You can subscribe to the other events in this section only through the [application](../../../settings/app-installation/index.md) and the [event.bind](../../events/event-bind.md) method.

To receive events:

- install the application and specify the public URL of the handler
- if the application has an interface, complete the installation with the `installFinish` method — events are not sent to the application until then. For more details, see the article [{#T}](../../../settings/app-installation/installation-finish.md)

Bitrix24 registers the [ONAPPUSERREADY](./on-app-user-ready.md) handler on the application installation URL automatically, and for applications without an interface, the [onAppInstall](./on-app-install.md) handler as well. You do not need to call `event.bind` for these events. ONAPPUSERREADY is not sent to local applications.

Bitrix24 does not send events to an application if its paid or trial period has expired.

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../events/test-handler.md).

## Tokens in Events

The format of the request to the handler is described in the article [Concept and Benefits of Event Processing](../../events/index.md#auth).

#|
|| **Event** | **Tokens in auth** ||
|| onAppInstall, ONAPPUSERREADY, onAppUpdate | `access_token` and `refresh_token` ||
|| onAppPayment, onUserAdd | `access_token` without `refresh_token` ||
|| onAppUninstall, onAppMethodConfirm | No tokens ||
|#

## Interaction with Other Objects

The events in this section are related to methods for working with users and the application.

**User.** Data from the [onUserAdd](./on-user-add.md) event can be used together with the [user.get](../../user/user-get.md) method if additional information about the user is needed after registration or to configure access.

**Application.** After the [onAppPayment](./on-app-payment.md) event, you can retrieve the current application status and payment period with the [app.info](../system/app-info.md) method.

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`basic`](../../scopes/permissions.md), for `onUserAdd` — [`user`](../../scopes/permissions.md), [`user_brief`](../../scopes/permissions.md), or [`user_basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

#| 
|| **Event** | **Triggered** ||
|| [onAppInstall](./on-app-install.md) | When the application is successfully installed ||
|| [ONAPPUSERREADY](./on-app-user-ready.md) | When an application system user is created or reactivated ||
|| [onAppUpdate](./on-app-update.md) | When the application is updated ||
|| [onAppUninstall](./on-app-uninstall.md) | When the application is uninstalled ||
|| [onAppPayment](./on-app-payment.md) | When the application is paid for ||
|| [onAppMethodConfirm](./on-app-method-confirm.md) | When the administrator decides on a request for a method requiring confirmation ||
|| [onUserAdd](./on-user-add.md) | When a user is added to Bitrix24 ||
|#
