# Application Settings: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Application settings store general integration parameters and personal values for each user.

{% note info "" %}

The methods in this section work only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}

> Quick navigation: [all methods](#all-methods)

## How to Choose a Group of Methods

If the setting is the same for the entire application, use the method group `app.option.*`. This is suitable for general integration parameters that do not depend on the user.

If each user should have their own values, use the method group `user.option.*`. It stores the settings of the user whose token was used for the call, such as personal interface settings.

## What You Can Store

Settings are stored as key-value pairs. A key is a string that you pass to the `*.set` methods and then in the `option` parameter of the `*.get` methods.

- **Reserved keys.** Do not use the keys `next` and `total`: the REST API moves them from `result` to the root of the response.
- **Size.** The methods do not check the size of values, but all settings of an application or a user are stored as a single record and read in full on every `*.get` call. Store integration parameters in settings, not working data.

## Access Permissions

Calling any method in this section via an inbound webhook returns the `ACCESS_DENIED` error with the text `Access denied! Application context required`.

The [app.option.set](./app-option-set.md) method is available only to a Bitrix24 administrator; for other users, it returns `ACCESS_DENIED` with the text `Access denied! Administrator authorization required`. The [app.option.get](./app-option-get.md), [user.option.set](./user-option-set.md), and [user.option.get](./user-option-get.md) methods are available to any authorized user of the application.

## Relationships with Other Objects

**Application.** [System Methods](../system/index.md) check the application context, available scopes, and the presence of the method in Bitrix24. For example, [app.info](../system/app-info.md) retrieves information about the application, while [scope](../system/scope.md) returns a list of available scopes.

**User.** Check permissions with [user.admin](../users/user-admin.md) before calling `app.option.set`. The basic data of the current user is retrieved by [profile](../users/profile.md).

## Overview of Methods {#all-methods}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Application Settings

#| 
|| **Method** | **Description** ||
|| [app.option.set](./app-option-set.md) | Saves general application settings ||
|| [app.option.get](./app-option-get.md) | Retrieves the value of a single setting or an object with all application settings ||
|#

### Current User Settings

#| 
|| **Method** | **Description** ||
|| [user.option.set](./user-option-set.md) | Saves the current user's settings for the application ||
|| [user.option.get](./user-option-get.md) | Retrieves the value of a single setting or an object with all settings of the current user ||
|#