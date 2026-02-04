# Event Log: Overview of Methods

The event log records user actions in Bitrix24: logins, password change requests, and a number of system operations. The available events depend on the Bitrix24 plan.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [What is the Event Log in Bitrix24](https://helpdesk.bitrix24.com/open/19091828/)

The methods `main.eventlog.*` allow you to:
- export events based on a filter,
- retrieve a specific record,
- set up regular updates for new data.

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
- track new events after a certain point.

## Overview of Methods {#all-methods}

> Scope: [`main`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

#|
|| **Method** | **Description** ||
|| [main.eventlog.list](./main-eventlog-list.md) | Returns a list of log entries based on a filter ||
|| [main.eventlog.get](./main-eventlog-get.md) | Returns a log entry by identifier ||
|| [main.eventlog.tail](./main-eventlog-tail.md) | Returns new log entries after a reference point ||
|#

## Continue Learning

- [{#T}](./main-eventlog-list.md)
- [{#T}](./main-eventlog-get.md)
- [{#T}](./main-eventlog-tail.md)
- [{#T}](../index.md)