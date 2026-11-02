# Time Tracking: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Time tracking methods help an app manage an employee's workday and retrieve their work schedule, absence reports, and work time records. For example, an integration with an access control system can open the workday when an employee passes the entrance turnstile and close it on exit.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Time and Reports in Bitrix24](https://helpdesk.bitrix24.com/open/18041848/)

## How to Get Started

1. Define the integration scenario: managing the current workday, reading the schedule, tracking absences, checking the office network, or retrieving work time records
2. To start, pause, close, and check the current workday, use the [Workday](./base/index.md) methods
3. To retrieve schedule settings, use the [Work Schedule](./schedule/index.md) method
4. To configure time control and retrieve reports on identified absences, use the [Time Control](./timecontrol/index.md) methods
5. To check office network IP addresses, use the [Office Networks](./networkrange/index.md) methods
6. To retrieve work time records in REST 3.0, use the [Work Time Records](./record/index.md) methods

If the time tracking tool is disabled in Bitrix24, the workday, schedule, time control, and office network methods, as well as the [timeman.record.list](./record/timeman-record-list.md) method, return the `TIMEMAN_TOOL_DISABLED` error.

## Workday

Time tracking records the hours worked by an employee. To do this, the employee marks the start and end of the workday in the system. You can manage the workday using the methods of the [Workday](./base/index.md) section.

A typical workday goes like this:

1. Check whether the day is open using [timeman.status](./base/timeman-status.md)
2. Start the day using [timeman.open](./base/timeman-open.md)
3. Pause it using [timeman.pause](./base/timeman-pause.md)
4. Resume work using the same [timeman.open](./base/timeman-open.md) method
5. Close the day using [timeman.close](./base/timeman-close.md)

{% note tip "User Documentation" %}

- [Worktime](https://helpdesk.bitrix24.com/open/24856218/)

{% endnote %}

## Work Schedule

The work schedule defines the mode and duration of employees' work. You can obtain the work schedule settings using the method [timeman.schedule.get](./schedule/timeman-schedule-get.md).

{% note tip "User Documentation" %}

- [Work Schedules](https://helpdesk.bitrix24.com/open/18039560)

{% endnote %}

## Time Control

Time control records when an employee was offline or idle during working hours. The manager views the monthly absence report, and the employee can specify the reason for each absence.

You can work with reports and configure time control using the group of methods [timeman.timecontrol.*](./timecontrol/index.md).

{% note tip "User Documentation" %}

- [Working Time Control](https://helpdesk.bitrix24.com/open/24640398)

{% endnote %}

## Office Networks

An office network is a set of IP address ranges of the organization's local network. The ranges are set and checked by the methods of the [timeman.networkrange.*](./networkrange/index.md) group.

Office network methods are related to time control: reports on identified absences can show whether an employee was in the office network when starting or ending the workday.

## Work Time Records

In [REST 3.0](../rest-v3.md), [timeman.record.*](./record/index.md) methods are available. They return employee work time records and the description of record fields. The methods are called at `/rest/api/`.

Work time records show when an employee started and ended the workday, how long breaks took, and how many hours were worked in the selected period. This data helps analyze workload, track unclosed or unconfirmed days, and compare actually worked time.

## Linking to Other Objects

Time tracking methods work with Bitrix24 employees and departments.

**User.** The employee is passed in the `USER_ID` parameter, and in the [timeman.record.list](./record/timeman-record-list.md) method, in the `userId` filter field. The employee identifier is returned by the [user.get](../user/user-get.md) method.

**Department.** The [timeman.timecontrol.reports.users.get](./timecontrol/timeman-timecontrol-reports-users-get.md) method retrieves department employees by the `DEPARTMENT_ID` parameter. The department identifier is returned by the [department.get](../departments/department-get.md) method.

## Access Permissions {#access}

Permissions depend on whose data the method retrieves or changes:

- any user — their own workday, their own working time settings, and the reasons for their own absences
- a user with the "View all work schedules" permission — the work schedule. Employees have this permission by default
- a user with the "Anybody's Clock-in And Clock-out" or "Edit All Records" permission — the workday and settings of another employee. A department head who has neither permission cannot manage a subordinate's workday
- a user with the "Subordination Controlled Read" permission — their own work time records, and for a department head, also the records of their subordinates
- a user with the "Read All Records" permission — work time records of any employee
- a department head — time control reports on their subordinates
- an administrator — reports on any employee, time control settings, office network ranges, and IP address checks. In cloud Bitrix24, an integrator can also work with time control settings, office network ranges, and IP address checks
- a user who has access to the full report in the time control settings — reports on any employee
- a user who has access to the simple report — their own report

A user who has access to a report in the time control settings sees it only while time control is enabled. The department head and the administrator see reports even when time control is disabled.

How to authorize requests is described in [Authorization in REST](../../settings/how-to-call-rest-api/authorization.md), and request rate limits are described in [REST API Limits](../../settings/performance/limits.md).

## Overview of Methods {#all-methods}

> Scope: [`timeman`](../scopes/permissions.md)
>
> Who can execute the methods: depends on the method, see [Access Permissions](#access) for details

### Workday

#|
|| **Method** | **Description** ||
|| [timeman.open](./base/timeman-open.md) | Starts a new workday or resumes it after a pause or after it was closed ||
|| [timeman.pause](./base/timeman-pause.md) | Pauses the workday ||
|| [timeman.close](./base/timeman-close.md) | Closes the workday ||
|| [timeman.status](./base/timeman-status.md) | Retrieves information about the user's current workday ||
|| [timeman.settings](./base/timeman-settings.md) | Retrieves the user's working time settings ||
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

### Work Time Records in REST 3.0

#|
|| **Method** | **Description** ||
|| [timeman.record.list](./record/timeman-record-list.md) | Retrieves a list of employee work time records ||
|| [timeman.record.field.list](./record/timeman-record-field-list.md) | Retrieves a list of work time record fields ||
|| [timeman.record.field.get](./record/timeman-record-field-get.md) | Retrieves the description of a work time record field ||
|#
