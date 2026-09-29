# User Information: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The methods in this group work exclusively with the current user: they retrieve basic profile data, check access codes, and check Bitrix24 administrator permissions. The methods do not require a separate scope.

Information about other users can be obtained through the [Users](../../user/index.md) group of methods.

> Quick Navigation: [all methods](#all-methods)

## How to Choose a Method

#|
|| **If you need to** | **Use** ||
|| Retrieve profile data: name, time zone, administrator flag | [profile](./profile.md) ||
|| Check whether the user belongs to a department, group, or project by access code | [user.access](./user-access.md) ||
|| Check whether the user is a Bitrix24 administrator | [user.admin](./user-admin.md) ||
|#

Access code formats are described on the [user.access](./user-access.md) page, and their names are retrieved by the [access.name](../system/access-name.md) method.

## Overview of Methods      {#all-methods}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [user.admin](./user-admin.md) | Checks if the current user is a Bitrix24 administrator ||
|| [user.access](./user-access.md) | Checks if the current user has at least one of the specified access codes (`ACCESS`) ||
|| [profile](./profile.md) | Retrieves basic data of the current user ||
|#