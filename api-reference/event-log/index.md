# Event Log in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The event log records user actions in Bitrix24: logins, password change requests, and various system operations. The available events depend on the Bitrix24 plan.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [What is the Event Log in Bitrix24](https://helpdesk.bitrix24.com/open/19091828/)

{% note info "" %}

The methods in this section belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../rest-v3.md).

{% endnote %}

The methods `main.eventlog.*` allow you to:
- export events based on a filter
- retrieve a specific record
- set up regular updates for new data
- get descriptions of available log record fields

## How a Log Record Is Structured

The methods in this section return log records and descriptions of their fields. The fields below are the ones most often used to build a query. The full list of fields and their flags is returned by [main.eventlog.field.list](./main-eventlog-field-list.md).

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Log record identifier ||
|| **timestampX**
[`datetime`](../data-types.md#standart-types) | Date and time of the event ||
|| **auditTypeId**
[`string`](../data-types.md) | Event type, for example `USER_AUTHORIZE` ||
|| **moduleId**
[`string`](../data-types.md) | Identifier of the module that recorded the event, for example `main` or `rest` ||
|| **userId**
[`integer`](../data-types.md) | User identifier ||
|| **description**
[`string`](../data-types.md) | Event description. The string contains a JSON object, and the set of keys depends on `auditTypeId` ||
|#

The remaining record fields — `severity`, `itemId`, `remoteAddr`, `userAgent`, `requestUri`, `siteId`, `guestId` — are described on the [main.eventlog.field.list](./main-eventlog-field-list.md) page.

## Example of a Log Record

Abbreviated response of [main.eventlog.get](./main-eventlog-get.md) in REST 3.0 format. The response contains all fields listed in `select` and the `time` object with the request execution time. The `remoteAddr`, `userAgent`, `requestUri`, `siteId`, and `guestId` fields are not shown in the example.

```json
{
    "result": {
        "item": {
            "id": 1250,
            "timestampX": "2026-01-30T16:00:39+02:00",
            "severity": "SECURITY",
            "auditTypeId": "USER_AUTHORIZE",
            "moduleId": "main",
            "itemId": "1",
            "userId": 1,
            "description": "{\"userId\":1,\"storedAuthId\":1310,\"requestId\":\"ea604c292209f5c562c653ba025b801e-1\",\"method\":\"cookie\"}"
        }
    },
    "time": {
        "start": 1769779221,
        "finish": 1769779222.060889,
        "duration": 1.0608890056610107,
        "processing": 0,
        "date_start": "2026-01-30T16:20:21+02:00",
        "date_finish": "2026-01-30T16:20:22+02:00",
        "operating_reset_at": 1769779822,
        "operating": 0
    }
}
```

## When to Use Each Method

Use [main.eventlog.get](./main-eventlog-get.md) when you need to get details of a specific event by identifier.

The methods [main.eventlog.list](./main-eventlog-list.md) and [main.eventlog.tail](./main-eventlog-tail.md) are similar but serve different purposes.

Use [main.eventlog.list](./main-eventlog-list.md) when you need to:
- generate a report for a period
- find events based on complex criteria
- implement search and navigation through history

Use [main.eventlog.tail](./main-eventlog-tail.md) when you need to:
- set up real-time monitoring
- synchronize an external system with the log
- track new events after a certain threshold

Use [main.eventlog.field.list](./main-eventlog-field-list.md) and [main.eventlog.field.get](./main-eventlog-field-get.md) when you need to:
- learn about available fields for `select`, `filter`, and `order`
- get types and metadata for a specific field
- automatically build filtering interfaces and tables

## Limitations and Specifics

- The methods in this section require the `main` scope. Without permission to view the event log, Bitrix24 returns `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`
- The log is read-only: the methods do not create, modify, or delete records. The set of recorded events depends on the Bitrix24 plan, so the same filter returns a different number of records on different plans
- `main.eventlog.list` and `main.eventlog.tail` return `50` records per request by default, with a maximum of `1000`. Call `list` page by page with the `pagination` parameter, and continue `tail` by the `cursor`
- `main.eventlog.tail` continues the selection from the last retrieved record, so the cursor field cannot be passed in `filter` at the same time. For details on the polling loop, see the [method page](./main-eventlog-tail.md)
- `select` accepts only an array of strings with field names

## Common Errors of the Section

All methods in this section return errors in the unified REST 3.0 format; the format is described in the [unified response format](../rest-v3.md#response) section. The codes below are the ones to handle in any scenario. Full error tables with fields and fixes are on the method pages.

#|
|| **Error Code** | **Where It Occurs** | **Cause** ||
|| `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION` | all methods | No permission to view the event log or the `main` scope is missing ||
|| `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION` | all methods | The required `id` or `name` parameter is missing, a parameter has the wrong type, or a field is used without the required flag ||
|| `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` | `main.eventlog.get` | There is no record with this `id` in the log ||
|| `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION` | `main.eventlog.field.get` | There is no field with this `name` in a log record ||
|#

## Overview of Methods {#all-methods}

> Scope: [`main`](../scopes/permissions.md)
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
- [{#T}](../rest-v3.md)
