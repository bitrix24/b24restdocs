# Workday: Overview of Methods

Time tracking in Bitrix24 records the hours worked by an employee. To do this, the employee marks the start and end of the workday in the system.

The methods `timeman.*` are used to manage the workday of the token holder or webhook. To manage the workdays of other employees, the user needs access permission to edit others' workdays.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [How to track working hours in Bitrix24](https://helpdesk.bitrix24.com/open/21658854)

## Linking the Workday to Other Objects

**User.** The workday is linked to the user by the identifier `USER_ID`. The user ID can be obtained using the [user.get](../../user/user-get.md) method.

## Managing the Workday

The [timeman.open](./timeman-open.md) method starts the workday for the employee `USER_ID`. The system determines the time zone of the current workday based on the `TIME` parameter of the method. If the `TIME` parameter is not specified, the default time zone of the user set in the profile is used.

To set a break or lunch period, use the [timeman.pause](./timeman-pause.md) method.

The workday can be ended using the [timeman.close](./timeman-close.md) method:
- by default, the workday ends at the current moment
- the time zone of the start of the day is used
- the end date must match the start date

If the time zone of the end of the day differs from the start time zone, the end time is automatically recalculated to the time zone in which the day was started.

To continue the workday after a break or closure, use the [timeman.open](./timeman-open.md) method again.

The current status can be checked using the [timeman.status](./timeman-status.md) method.

## Time Format

The time parameter `TIME` is accepted in the [ATOM](https://www.php.net/manual/en/class.datetimeinterface.php#datetimeinterface.constants.atom) standard format (ISO-8601), for example, `2025-02-12T15:52:01+00:00`. The system considers the time zone specified in the parameter and treats it as the user's time zone.

## Work Schedule Settings

Work schedule settings define the mode and duration of the employee's work. The settings can be obtained using the [timeman.settings](./timeman-settings.md) method.

{% note tip "User documentation" %}

-  [Workflow designer](https://helpdesk.bitrix24.com/open/23379262/)

{% endnote %}

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