# Event Log: Overview of Methods

The event log records user actions in Bitrix24: logins, password change requests, and various system operations. The available events depend on the Bitrix24 plan.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [What is the Event Log in Bitrix24](https://helpdesk.bitrix24.com/open/19091828/)

The methods `main.eventlog.*` allow you to:
- export events based on a filter,
- retrieve a specific record,
- set up regular updates for new data,
- get descriptions of available log record fields.

## When to Use Each Method

Use [main.eventlog.get](./main-eventlog-get.md) when you need to get details of a specific event by ID.

The methods [main.eventlog.list](./main-eventlog-list.md) and [main.eventlog.tail](./main-eventlog-tail.md) are similar but serve different purposes.

Use [main.eventlog.list](./main-eventlog-list.md) when you need to:
- generate a report for a period,
- find events based on complex criteria,
- implement search and navigation through history.

Use [main.eventlog.tail](./main-eventlog-tail.md) when you need to:
- set up real-time monitoring,
- synchronize an external system with the log,
- track new events after a certain threshold.

Use [main.eventlog.field.list](./main-eventlog-field-list.md) and [main.eventlog.field.get](./main-eventlog-field-get.md) when you need to:
- learn about available fields for `select`, `filter`, and `order`,
- get types and metadata for a specific field,
- automatically build filtering interfaces and tables.

## Overview of Methods {#all-methods}

> Scope: [`main`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

#|
|| **Method** | **Description** ||
|| [main.eventlog.list](./main-eventlog-list.md) | Returns a list of log records based on a filter ||
|| [main.eventlog.get](./main-eventlog-get.md) | Returns a log record by identifier ||
|| [main.eventlog.tail](./main-eventlog-tail.md) | Returns new log records after a specified point ||
|| [main.eventlog.field.list](./main-eventlog-field-list.md) | Returns a list of log record fields ||
|| [main.eventlog.field.get](./main-eventlog-field-get.md) | Returns the description of a log record field by name ||
|#

## Continue Learning

- [{#T}](./main-eventlog-list.md)
- [{#T}](./main-eventlog-get.md)
- [{#T}](./main-eventlog-tail.md)
- [{#T}](./main-eventlog-field-list.md)
- [{#T}](./main-eventlog-field-get.md)
- [{#T}](../index.md)