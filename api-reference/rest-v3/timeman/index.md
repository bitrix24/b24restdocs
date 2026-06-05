# Time Tracking Records: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Time tracking records help monitor when an employee starts and ends their workday, how long breaks take, and how many hours were worked during a selected period. This data is used to analyze workload, track unclosed or unconfirmed days, and reconcile actual working hours.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Worktime](hhttps://helpdesk.bitrix24.com/open/24856218/)

## Getting Started

The methods `timeman.record.*` return time tracking records for a specific employee and the structure of this object's fields. To modify data, use the methods in the [Time Tracking](../../timeman/index.md) section.

1. Identify the employee ID `userId` for whom you need to retrieve time tracking records.
2. Formulate a request to [timeman.record.list](./timeman-record-list.md): pass the employee ID in `filter.userId`, and if necessary, add a condition for `startTime` to get records for the desired period.
3. In the same request, specify `select` if you need to return only certain fields.
4. In the same request, use `order` and `pagination` if you need to sort records by the start time of the workday and limit the sample size.
5. Clarify available fields through [timeman.record.field.list](./timeman-record-field-list.md) if you need to check which fields are available in the response, filter, and sorting.
6. Obtain the description of a specific field through [timeman.record.field.get](./timeman-record-field-get.md) if you need to understand the data type and metadata of one field before processing the response.

## Section Limitations

- The method [timeman.record.list](./timeman-record-list.md) requires a mandatory filter by `userId`.
- Only one employee ID can be passed in a single request to [timeman.record.list](./timeman-record-list.md).
- Access to time tracking records depends on read permissions and subordination settings.

## Relation to Other Objects

**User.** In [timeman.record.list](./timeman-record-list.md), the employee ID is passed in `filter.userId`. It can be obtained using the [user.get](../../user/user-get.md) method.

**Working Time Period.** To filter records for a specific day, week, or other period, the `startTime` field is used. This allows you to build a selection based on a date range and retrieve only those records that pertain to the desired reporting period.

## Overview of Methods {#all-methods}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#| 
|| **Method** | **Description** ||
|| [timeman.record.list](./timeman-record-list.md) | Returns a list of time tracking records for an employee ||
|| [timeman.record.field.list](./timeman-record-field-list.md) | Returns a list of fields for a time tracking record ||
|| [timeman.record.field.get](./timeman-record-field-get.md) | Returns the description of a time tracking record field ||
|#

## Continue Your Exploration

- [{#T}](../index.md)