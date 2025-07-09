# Time Tracking: Overview of Methods

Time tracking in Bitrix24 checks the compliance of an employee's working hours with the established schedule. The work schedule records:

-  start and end of the workday
-  number of working hours per day
-  permissible editing of the workday by the employee
-  allowable underperformance for the reporting period

The system logs schedule violations, and the manager can view reports on these violations.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [Working time control](https://helpdesk.bitrix24.com/open/24640398/)

## Connection of Time Tracking with Other Objects

**User.** The report on identified absences is linked to the user by the identifier `USER_ID`. In the time tracking tool settings, lists of users are defined:

-  who to request the report from
-  who has access to the simplified or detailed report

You can obtain the user identifier using the [user.get](../../user/user-get.md) method.

## Settings of the Time Tracking Tool

You can enable and configure the time tracking tool using the [timeman.timecontrol.settings.set](./timeman-timecontrol-settings-set.md) method. The settings define:

-  which user activities to record
-  users from whom to request the report
-  users who have access to simplified and detailed reports

Settings are usually the same for employees of the same department. To obtain the identifiers of users in the department, use the [timeman.timecontrol.reports.users.get](./timeman-timecontrol-reports-users-get.md) method.

You can get the current settings of the tool using the [timeman.timecontrol.settings.get](./timeman-timecontrol-settings-get.md) method.

## Reports on Identified Absences

To obtain a report on an employee's absences for a calendar month, use the [timeman.timecontrol.reports.get](./timeman-timecontrol-reports-get.md) method. If the method returns an empty array `days`, configure the time tracking tool using the [timeman.timecontrol.settings.set](./timeman-timecontrol-settings-set.md) method.

The [timeman.timecontrol.report.add](./timeman-timecontrol-report-add.md) method sends the absence report and adds it to the calendar.

## Overview of Methods {#all-methods}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#|
|| **Method** | **Description** ||
|| [timeman.timecontrol.report.add](./timeman-timecontrol-report-add.md) | Sends a report on identified absences ||
|| [timeman.timecontrol.reports.get](./timeman-timecontrol-reports-get.md) | Retrieves a report on identified absences ||
|| [timeman.timecontrol.settings.get](./timeman-timecontrol-settings-get.md) | Retrieves the settings of the time tracking tool ||
|| [timeman.timecontrol.settings.set](./timeman-timecontrol-settings-set.md) | Sets the settings of the time tracking tool ||
|| [timeman.timecontrol.reports.settings.get](./timeman-timecontrol-reports-settings-get.md) | Retrieves report settings for building the report interface of the time tracking tool ||
|| [timeman.timecontrol.reports.users.get](./timeman-timecontrol-reports-users-get.md) | Retrieves a list of users in the specified department ||
|#