# User Management: Overview of Methods

This section describes working with users' SIP settings:

- how to view the current configuration of an employee
- how to enable the SIP device indicator

The response includes the main parameters for connections and calls, including the default line, device status, server, credentials, and internal number.

Calling the methods depends on the `User Settings — Modify` access permission. If the permissions are insufficient, modifying the user's SIP settings will not be available.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Telephony Users: Basic Settings for Employees](https://helpdesk.bitrix24.com/open/17359536/)

## Interaction with Other Objects

**User.** The methods in this section operate with users by `USER_ID`. The user identifier can be obtained using the [user.get](../../../user/user-get.md) method.

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
> Who can execute the method: user with the User Settings — Modify permission

#| 
|| **Method** | **Description** ||
|| [voximplant.user.get](./voximplant-user-get.md) | Returns user settings ||
|| [voximplant.user.activatePhone](./voximplant-user-activate-phone.md) | Sets the SIP device indicator for the employee ||
|#