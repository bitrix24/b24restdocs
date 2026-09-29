# General Methods and Events: Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

General methods and events help verify the application context, retrieve data about the current user, save integration settings, and manage the application lifecycle.

No separate scopes are needed for the methods in this section. The exception is the [onUserAdd](./events/on-user-add.md) event, which requires the `user`, `user_basic`, or `user_brief` scope.

> Quick navigation: [all methods and events](#all-methods)

## How to Choose a Group of Methods

#|
|| **If you need** | **Open the group of methods** ||
|| Check the availability of a method, application scope, `ACCESS` codes, and server time | [System methods](./system/index.md) ||
|| Retrieve the profile of the current user and check their permissions | [User information](./users/index.md) ||
|| Save general or personal application settings | [Application settings](./settings/index.md) ||
|| Respond to installation, updates, deletion, and payment of the application | [Events](./events/index.md) ||
|#

## How to Get Started

1. [Install the application](../../settings/app-installation/index.md) and handle the [onAppInstall](./events/on-app-install.md) event or retrieve the tokens on the installation page. If a mass-market application needs to work without an employee's involvement, use [ONAPPUSERREADY](./events/on-app-user-ready.md): it passes the authorization of the application's system user.
2. Check the current user's permissions with the [user.admin](./users/user-admin.md) method.
3. Save general application settings with the [app.option.set](./settings/app-option-set.md) method.
4. Store personal employee settings with [user.option.set](./settings/user-option-set.md) and read them with [user.option.get](./settings/user-option-get.md).

## What to Consider Before Getting Started

**Application Context.** The methods `app.option.*`, `user.option.*`, and the `onApp*` events work only within the context of the [application](../../settings/app-installation/index.md). The onUserAdd event is also available to an outbound webhook. System methods and methods of the User Information group can also be called via a webhook.

**User Permissions.** The [app.option.set](./settings/app-option-set.md) method can be executed only by a Bitrix24 administrator; for other users, it returns `ACCESS_DENIED` with the text `Access denied! Administrator authorization required`. The other methods in this section are available to any authorized user.

## Relationships with Other Objects

This section is connected with the application and the current user.

**Application.** The methods [app.info](./system/app-info.md), [app.option.get](./settings/app-option-get.md), and [app.option.set](./settings/app-option-set.md) work with the data and settings of the current application.

**User.** The method [profile](./users/profile.md) retrieves the ID, first name, last name, and other data of the current user. The methods [user.admin](./users/user-admin.md) and [user.access](./users/user-access.md) check their permissions before calling methods with restrictions.

## Overview of Methods and Events {#all-methods}

> Scope: [`basic`](../scopes/permissions.md), for the `onUserAdd` event — [`user`, `user_basic`, or `user_brief`](../scopes/permissions.md)
>
> Who can execute the method: any user, `app.option.set` — administrator

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [method.get](./system/method-get.md) | Checks the existence of a method in Bitrix24 and the availability of a call for the application ||
    || [scope](./system/scope.md) | Retrieves the list of scopes available to the current application ||
    || [app.info](./system/app-info.md) | Returns information about the application ||
    || [access.name](./system/access-name.md) | Retrieves the names of the `ACCESS` codes ||
    || [feature.get](./system/feature-get.md) | Checks the availability of functionality in Bitrix24 ||
    || [server.time](./system/server-time.md) | Returns the current server time ||
    || [user.admin](./users/user-admin.md) | Checks if the current user is a Bitrix24 administrator ||
    || [user.access](./users/user-access.md) | Checks if the current user has at least one of the specified access codes (`ACCESS`) ||
    || [profile](./users/profile.md) | Retrieves information about the current user ||
    || [app.option.set](./settings/app-option-set.md) | Saves general application settings ||
    || [app.option.get](./settings/app-option-get.md) | Retrieves general application settings ||
    || [user.option.set](./settings/user-option-set.md) | Saves the current user's settings for the application ||
    || [user.option.get](./settings/user-option-get.md) | Retrieves the current user's settings for the application ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onAppInstall](./events/on-app-install.md) | Upon successful installation of the application ||
    || [ONAPPUSERREADY](./events/on-app-user-ready.md) | Upon creation or reactivation of the application's system user ||
    || [onAppUpdate](./events/on-app-update.md) | Upon updating the application ||
    || [onAppUninstall](./events/on-app-uninstall.md) | Upon uninstalling the application ||
    || [onAppPayment](./events/on-app-payment.md) | Upon payment for the application ||
    || [onAppMethodConfirm](./events/on-app-method-confirm.md) | Upon an administrator's decision on a request for a method requiring confirmation ||
    || [onUserAdd](./events/on-user-add.md) | Upon adding a user to Bitrix24 ||
    |#

{% endlist %}