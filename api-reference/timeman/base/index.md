# Workday: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Time tracking in Bitrix24 records the hours worked by an employee. To do this, the employee marks the start and end of the workday in the system.

The methods in this section open, pause, and close an employee's workday, and return its state and the working time settings. To work with another employee's workday, the user needs the "Anybody's Clock-in And Clock-out" or "Edit All Records" permission.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Worktime](https://helpdesk.bitrix24.com/open/24856218/), [Work Schedules](https://helpdesk.bitrix24.com/open/18039560)

## How to Get Started

1. Obtain the employee's `USER_ID` if you need to manage another employee's workday
2. Check the current workday status using [timeman.status](./timeman-status.md)
3. If the workday is not open, start it using [timeman.open](./timeman-open.md)
4. If the employee goes on a break, pause the day using [timeman.pause](./timeman-pause.md)
5. When work is completed, close the workday using [timeman.close](./timeman-close.md)
6. If you need to account for the work schedule and change approval rules, retrieve the working time settings using [timeman.settings](./timeman-settings.md)

## Linking to Other Objects

A workday always belongs to a specific user.

**User.** The methods in this section work with the workday of the employee passed in the `USER_ID` parameter. If the parameter is not passed, the owner of the token or webhook is used. The employee ID can be obtained using the [user.get](../../user/user-get.md) method.

## General Rules for Working with the Workday

**Method response.** The methods `timeman.open`, `timeman.pause`, `timeman.close`, and `timeman.status` return the same workday object. Its fields are described on the [timeman.status](./timeman-status.md) page. Key fields:

- `STATUS` — the state of the day: `OPENED`, `PAUSED`, `CLOSED`, or `EXPIRED`. If an action does not apply to the current state, `timeman.open` and `timeman.pause` do not return an error but return the day unchanged, so check `STATUS` in the response
- `DURATION` — the duration of the day, equal to `00:00:00` until the day is closed
- `TIME_LEAKS` — the total duration of breaks
- `ACTIVE` — the value `false` means that the change to the day is awaiting approval by a supervisor

**Resuming the day.** The `timeman.open` method continues a paused or closed workday. The time between the pause or closure and the resumption is counted as a break in the `TIME_LEAKS` field.

**Common errors.** Any method in this section returns `TIMEMAN_TOOL_DISABLED` if the Time Tracking tool is disabled. When the `USER_ID` of another employee is passed, the method returns the error `User not found` if the employee is not found, and `User does not have access to managing other users work time` if the user has no permission to manage another employee's workday.

**Time and time zone.** The `TIME` parameter is passed in the [ATOM](https://www.php.net/manual/en/class.datetimeinterface.php#datetimeinterface.constants.atom) format (ISO-8601), for example `2025-02-12T15:52:01+00:00`. The value is converted to the employee's time zone. The opening date must match the employee's current date, and the closing date must match the start date of the day.

## Overview of Methods {#all-methods}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can perform the method: any user

#|
|| **Method** | **Description** ||
|| [timeman.open](./timeman-open.md) | Start a new workday or resume a closed one ||
|| [timeman.pause](./timeman-pause.md) | Pause the workday ||
|| [timeman.close](./timeman-close.md) | Close the workday ||
|| [timeman.status](./timeman-status.md) | Get information about the user's current workday ||
|| [timeman.settings](./timeman-settings.md) | Get the user's working time settings ||
|#
