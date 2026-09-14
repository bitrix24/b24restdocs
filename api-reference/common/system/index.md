# System Methods: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

System methods check the availability of methods and scope, retrieve information about the application, access codes, available functionality, and server time.

> Quick Navigation: [All Methods](#all-methods)

## What to Check Before Calling a Method

**Application Context.** The method [app.info](./app-info.md) only works within the context of the application. Otherwise, the method will return an `ACCESS_DENIED` error.

**Method Availability.** For new integrations, use [method.get](./method-get.md). This method shows whether the method exists in Bitrix24 and if it can be called with the current permissions.

**Functionality Availability.** The method [feature.get](./feature-get.md) is needed when behavior depends on scope and the functionality enabled in Bitrix24.

## Relationship with Other Objects

**Access Permissions.** The method [access.name](./access-name.md) decodes `ACCESS` codes used by, for example, [user.access](../users/user-access.md). User permissions, rather than application permissions, are checked by [user.access](../users/user-access.md).

**Application Permissions.** The method [scope](./scope.md) returns the scope of the current application. The values of scope codes are described on the [available scopes](../../scopes/permissions.md) page.

**User.** The method [profile](../users/profile.md) retrieves basic data about the current user. The methods [user.access](../users/user-access.md) and [user.admin](../users/user-admin.md) check access codes and user permissions.

## Overview of Methods {#all-methods}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [method.get](./method-get.md) | Checks the existence of a method in Bitrix24 and its availability for the application ||
|| [scope](./scope.md) | Retrieves a list of scopes available to the current application ||
|| [app.info](./app-info.md) | Returns information about the application ||
|| [access.name](./access-name.md) | Retrieves the names of `ACCESS` codes ||
|| [feature.get](./feature-get.md) | Checks the availability of functionality in Bitrix24 ||
|| [server.time](./server-time.md) | Returns the current server time ||
|| [methods](./methods.md) | {% note warning "DEPRECATED" %}

Development of this method has been halted. Use [method.get](./method-get.md).

{% endnote %} ||
|#