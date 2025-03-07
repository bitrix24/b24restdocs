# Time Tracking: Overview of Methods

Time tracking in Bitrix24 helps organize the workflow and monitor employee adherence to schedules.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [time and reports in Bitrix24](https://helpdesk.bitrix24.com/open/18041848/)

## Workday

Time tracking records the hours worked by an employee. To do this, the employee marks the start and end of the workday in the system. Managing the workday can be done using the group of methods [timeman.*](./base/index.md).

{% note tip "User Documentation" %}

-  [Worktime tracking in Bitrix24](https://helpdesk.bitrix24.com/open/21658854)

{% endnote %}

## Work Schedule

The work schedule defines the mode and duration of employees' work. You can obtain the work schedule settings using the method [timeman.schedule.get](./schedule/timeman-schedule-get.md).

{% note tip "User Documentation" %}

-  [Work Schedules](https://helpdesk.bitrix24.com/open/18039560)

{% endnote %}

## Time Control

Time tracking checks the compliance of an employee's working hours with the established schedule. The system records schedule violations, and the manager can view reports on these violations.

You can work with reports and configure time control using the group of methods [timeman.timecontrol.*](./timecontrol/index.md).

{% note tip "User Documentation" %}

-  [Working time control](https://helpdesk.bitrix24.com/open/24640398)

{% endnote %}

## Office Networks

An office network is a group of IP addresses used within the organization's local network. Working with the ranges of IP addresses in the office network is done using the methods from the group [timeman.networkrange.*](./networkrange/index.md).

## Overview of Methods {#all-methods}

> Scope: [`timeman`](../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Workday

#|
|| **Method** | **Description** ||
|| [timeman.open](./base/timeman-open.md) | Start a new workday or resume a closed one ||
|| [timeman.pause](./base/timeman-pause.md) | Pause the workday ||
|| [timeman.close](./base/timeman-close.md) | Close the workday ||
|| [timeman.status](./base/timeman-status.md) | Get information about the user's current workday ||
|| [timeman.settings](./base/timeman-settings.md) | Get the user's working time settings ||
|#

### Work Schedule

#|
|| **Method** | **Description** ||
|| [timeman.schedule.get](./schedule/timeman-schedule-get.md) | Retrieves the work schedule by ID ||
|#

### Time Control

#|
|| **Method** | **Description** ||
|| [timeman.timecontrol.report.add](./timecontrol/timeman-timecontrol-report-add.md) | Submits a report on identified absences ||
|| [timeman.timecontrol.reports.get](./timecontrol/timeman-timecontrol-reports-get.md) | Retrieves a report on identified absences ||
|| [timeman.timecontrol.settings.get](./timecontrol/timeman-timecontrol-settings-get.md) | Retrieves the settings for the time control tool ||
|| [timeman.timecontrol.settings.set](./timecontrol/timeman-timecontrol-settings-set.md) | Sets the settings for the time control tool ||
|| [timeman.timecontrol.reports.settings.get](./timecontrol/timeman-timecontrol-reports-settings-get.md) | Retrieves user settings for building the time control tool report interface ||
|| [timeman.timecontrol.reports.users.get](./timecontrol/timeman-timecontrol-reports-users-get.md) | Retrieves the list of users in the specified department ||
|#

### Office Networks

#|
|| **Method** | **Description** ||
|| [timeman.networkrange.get](./networkrange/timeman-networkrange-get.md) | Retrieves the ranges of network addresses included in the office network ||
|| [timeman.networkrange.set](./networkrange/timeman-networkrange-set.md) | Sets the ranges of network addresses included in the office network ||
|| [timeman.networkrange.check](./networkrange/timeman-networkrange-check.md) | Checks if an IP address is within the ranges of network addresses in the office network ||
|#