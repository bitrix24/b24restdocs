# User Management: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Users' SIP settings define the parameters for connecting an employee to a SIP phone or softphone. The `voximplant.user.*` methods allow you to:

- view an employee's current configuration
- enable the SIP device indicator

The [voximplant.user.get](./voximplant-user-get.md) method returns the main parameters for connections and calls: the default line, device status, server, credentials, and internal number.

## Access Permissions {#permissions}

Both methods check the `User Settings — Modify` permission. Although [voximplant.user.get](./voximplant-user-get.md) only reads settings, this permission is also required to call it.

The permission level determines which users can be managed:

- `Personal` — the current user only
- `Personal and department` — the current user and employees in their department
- `Any` — all users

[voximplant.user.get](./voximplant-user-get.md) returns the settings of accessible users. When called from an application, the method also requires confirmation from a Bitrix24 administrator. [voximplant.user.activatePhone](./voximplant-user-activate-phone.md) activates a SIP device only for a user who is available under the configured permission level.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Telephony Users: Basic Settings for Employees](https://helpdesk.bitrix24.com/open/17359536/)

## Interaction with Other Objects

**User.** The methods operate with users by `USER_ID`. The user identifier can be obtained using the [user.get](../../../user/user-get.md) method.

{% note tip "User Documentation" %}

- [How to Set Access Permissions in Telephony](https://helpdesk.bitrix24.com/open/18216960/)

{% endnote %}

## Getting Started

1. Obtain the `USER_ID` of the employee via [user.get](../../../user/user-get.md)
2. Check the current SIP settings using the [voximplant.user.get](./voximplant-user-get.md) method
3. Enable the SIP device using the [voximplant.user.activatePhone](./voximplant-user-activate-phone.md) method

## Overview of Methods {#all-methods}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the methods: a user with the User Settings — Modify permission within the configured [access level](#permissions)

#| 
|| **Method** | **Description** ||
|| [voximplant.user.get](./voximplant-user-get.md) | Returns user settings ||
|| [voximplant.user.activatePhone](./voximplant-user-activate-phone.md) | Sets the SIP device indicator for the employee ||
|#
