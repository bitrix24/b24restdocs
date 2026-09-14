# Work Schedule: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The work schedule defines the mode and duration of employees' work. You can retrieve the work schedule settings using the method [timeman.schedule.get](./timeman-schedule-get.md).

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Work Schedules](https://helpdesk.bitrix24.com/open/18039560/)

## Getting Started

1. Determine the work schedule identifier
2. Retrieve the schedule configurations using the [timeman.schedule.get](./timeman-schedule-get.md) method
3. Use the retrieved configurations when calculating work hours, shifts, or checking employee availability

## Work Schedule Identifier

`id` — the work schedule identifier. You can find it in the Bitrix24 interface on the *Employees > Time and Reports > Work Schedules* page.

Pass `id` to the [timeman.schedule.get](./timeman-schedule-get.md) method to retrieve the schedule configurations, shifts, calendar, and violation rules.

## Overview of Methods {#all-methods}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [timeman.schedule.get](./timeman-schedule-get.md) | Retrieves the work schedule by ID ||
|#
