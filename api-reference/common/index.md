# General Methods and Events: Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

General methods and events help verify the application context, retrieve data about the current user, save integration settings, and manage the application lifecycle.

No separate scopes are needed for the methods in this section. Additional restrictions depend on the application context and the access permissions of the current user.

> Quick navigation: [all methods and events](#all-methods)

## How to Choose a Group of Methods

#|
|| **If you need** | **Open the group of methods** ||
|| Check the availability of a method, application scope, `ACCESS` codes, and server time | [System methods](./system/index.md) ||
|| Retrieve the profile of the current user and check their permissions | [User information](./users/index.md) ||
|| Save general or personal application settings | [Application settings](./settings/index.md) ||
|| Respond to installation, updates, deletion, and payment of the application | [Events](./events/index.md) ||
|#

## What to Consider Before Getting Started

**Application Context.** The methods `app.*`, `user.option.*`, events in this section, and some system methods work only within the context of the [application](../../settings/app-installation/index.md). This is important to consider before the first call.

**Availability Check.** Before calling a method, check its availability via [method.get](./system/method-get.md), and obtain the list of scopes through [scope](./system/scope.md).

**Access Codes.** To verify the permissions of the current user, use [user.access](./users/user-access.md). Decoding `ACCESS` codes can be assisted by [access.name](./system/access-name.md).

**Application Settings.** Store general integration parameters through `app.option.*`, and personal user settings through `user.option.*`.

## Relationships with Other Objects

This section is connected with the application, the current user, and access permissions.

**Application.** The methods [app.info](./system/app-info.md), [app.option.get](./settings/app-option-get.md), and [app.option.set](./settings/app-option-set.md) work with the data and settings of the current application.

**User.** The method [profile](./users/profile.md) retrieves the ID, first name, last name, and other data of the current user. The methods [user.admin](./users/user-admin.md) and [user.access](./users/user-access.md) check their permissions before calling methods with restrictions.

**Permissions.** The method [scope](./system/scope.md) returns the scope of the current application. The values of the scope codes are described on the [available scopes](../scopes/permissions.md) page.

## Overview of Methods and Events {#all-methods}

> Scope: [`basic`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

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
    || [user.admin](./users/user-admin.md) | Checks if the current user can manage application settings ||
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
    || [onAppUpdate](./events/on-app-update.md) | Upon updating the application ||
    || [onAppUninstall](./events/on-app-uninstall.md) | Upon uninstalling the application ||
    || [onAppMethodConfirm](./events/on-app-method-confirm.md) | Upon receiving an administrator's decision on a request to use methods requiring confirmation ||
    || [onAppPayment](./events/on-app-payment.md) | Upon payment for the application ||
    || [onAppUserAdd](./events/on-user-add.md) | Upon adding a user to Bitrix24 ||
    |#

{% endlist %}