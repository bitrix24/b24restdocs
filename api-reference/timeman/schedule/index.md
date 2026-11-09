# Work Schedule: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The work schedule defines the rules of employees' workday, such as the latest start of the day and the minimum work duration. You can retrieve the schedule settings using the method [timeman.schedule.get](./timeman-schedule-get.md).

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Work Schedules](https://helpdesk.bitrix24.com/open/18039560/)

## Getting Started

1. Find the work schedule identifier in the Bitrix24 interface
2. Retrieve the schedule configurations using the [timeman.schedule.get](./timeman-schedule-get.md) method
3. Use the retrieved configurations when calculating work hours, shifts, or checking employee availability

## Work Schedule Identifier

You can find the work schedule identifier `id` in the Bitrix24 interface on the *Employees > Time and Reports > Work Schedules* page.

Pass `id` to the [timeman.schedule.get](./timeman-schedule-get.md) method to retrieve the schedule configurations, shifts, calendar, and violation rules.

## Relationship with Other Objects

The schedule is related to the employee's workday and work time records.

**Workday.** The [workday](../base/index.md) methods open, pause, and close an employee's workday. The schedule restrictions are returned by the [timeman.settings](../base/timeman-settings.md) method:
- `UF_TM_MAX_START` — the latest start of the day
- `UF_TM_MIN_FINISH` — the earliest end of the day
- `UF_TM_MIN_DURATION` — the minimum workday duration

The `UF_TM_FREE` field shows whether the employee works on a flexible schedule.

**Work time records.** Records of an employee's work intervals are returned by the [timeman.record.list](../record/timeman-record-list.md) method. The response does not include the schedule the employee worked on.

## Overview of Methods {#all-methods}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View all work schedules" permission. Employees have this permission by default

#|
|| **Method** | **Description** ||
|| [timeman.schedule.get](./timeman-schedule-get.md) | Retrieves the work schedule by ID ||
|#
