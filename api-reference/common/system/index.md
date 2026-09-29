# System Methods: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

System methods check the availability of methods and scope, retrieve information about the application, access codes, available features, and server time.

> Quick Navigation: [All Methods](#all-methods)

## What to Check Before Calling a Method

**Application Context.** The method [app.info](./app-info.md) returns full information about the application only within the application context.

**Method Availability.** Check a method with [method.get](./method-get.md): it shows whether the method exists in Bitrix24 and whether it can be called with the current scopes.

**Feature Availability.** The method [feature.get](./feature-get.md) checks whether a Bitrix24 feature that the application's behavior depends on is enabled, for example, the extended mode of offline events.

## Relationship with Other Objects

**Access Permissions.** The method [access.name](./access-name.md) decodes `ACCESS` codes used by, for example, [user.access](../users/user-access.md).

**Application Permissions.** The method [scope](./scope.md) returns the scope of the current application. The values of scope codes are described on the [available scopes](../../scopes/permissions.md) page.

**User.** The method [profile](../users/profile.md) retrieves basic data about the current user. The methods [user.access](../users/user-access.md) and [user.admin](../users/user-admin.md) check access codes and user permissions.

## Overview of Methods {#all-methods}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [method.get](./method-get.md) | Checks the existence of a method in Bitrix24 and its availability with the current scopes ||
|| [scope](./scope.md) | Retrieves a list of scopes available to the current application ||
|| [app.info](./app-info.md) | Returns information about the application ||
|| [access.name](./access-name.md) | Retrieves the names of `ACCESS` codes ||
|| [feature.get](./feature-get.md) | Checks the availability of a feature in Bitrix24 ||
|| [server.time](./server-time.md) | Returns the current server time ||
|| [methods](./methods.md) | Retrieves a list of available methods. Deprecated; use [method.get](./method-get.md) for new integrations ||
|#