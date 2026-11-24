# REST API Limits

Bitrix24 limits request execution time, request rate, and the resource intensity of methods, as well as the size of batches and pages in responses. The main values apply to the Bitrix24 cloud version. In the on-premise version, the limits are configured on the server side, and some of them are disabled by default.

The limits distribute the load across Bitrix24 instances so that intensive automation in one Bitrix24 does not interfere with other service users.

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Current limits:

#|
|| **Limit** | **Value** ||
|| Execution time of a single request | 60 seconds in the cloud version; in the on-premise version, depends on server settings ||
|| SQL query execution time inside a method | configured in the `rest` module; disabled by default in the on-premise version ||
|| Request rate | blocked when the request counter exceeds 50, with the counter decreasing by 2 per second; on the Enterprise plan, 250 and 5 per second ||
|| Resource intensity of a method | 420 seconds of accumulated method time over 10 minutes for one application or webhook in the cloud version; disabled by default in the on-premise version ||
|| Method calls in a single `batch` request | 50 ||
|| Records in one page of a list method | 50 by default for most methods ||
|#

How the time, rate, and resource intensity limits work and how to respond to their errors is described in the sections below.

## Request Execution Time Limit

In the Bitrix24 cloud version, one REST request must finish within 60 seconds. If processing takes longer, the request is interrupted by timeout.

Request duration can be affected by the data volume, filter complexity, nested calls in [batch](settings/how-to-call-rest-api/batch.md), and automation that is launched when the method is executed.

To reduce the timeout risk:

- split large operations into several requests
- use pagination and do not request unnecessary fields
- move long-running scenarios to an external queue or background processing on the application side
- check the `duration` key in the response [`time`](api-reference/data-types.md#time) object

In the Bitrix24 on-premise version, request execution time depends on server settings.

## SQL Query Execution Time Limit

Bitrix24 limits the execution time of a single SQL query inside a method. The limit applies to classic methods and [REST 3.0](api-reference/rest-v3.md) methods.

The limit is configured in the `rest` module settings. In the on-premise version, the restriction is disabled by default; once enabled, the default limit is 30 seconds. The setting is available only if the database supports it: MySQL 5.7.8 or later, or MariaDB 10.1.1 or later.

If an SQL query does not fit within the limit, the method returns the `BITRIX_REST_V3_EXCEPTION_LIMITER_QUERYTIMELIMITEXCEPTION` error with the text "Request canceled: query time limit exceeded" and status `503`. The `Retry-After` header contains the limit value in seconds: retry the call no earlier than after this time.

The exception is classic methods built on module controllers. The controller intercepts the SQL query error itself and returns a generic internal error with status `400`, so the limit excess cannot be identified from the response.

In classic [batch](settings/how-to-call-rest-api/batch.md), the error is returned in the result of the subrequest that exceeded the limit. In [batch 3.0](settings/how-to-call-rest-api/batch-v3.md), an error in any element returns an error for the whole batch.

## Request Rate Limit

The Bitrix24 request rate limit works on the leaky bucket algorithm. An application can perform intensive requests briefly, but it cannot maintain such intensity constantly.

The mechanism works as follows:

1. Each request increases a virtual counter. Bitrix24 keeps a separate counter for each pair of the request source IP address and the Bitrix24 instance.
2. When the counter value exceeds the threshold `X`, the following request is blocked with status `503` and error code `QUERY_LIMIT_EXCEEDED`.
3. The counter value automatically decreases by `Y` once per second.

This leads to two rules:

1. At an intensity of no more than `Y` requests per second, the application will not receive error `QUERY_LIMIT_EXCEEDED`.
2. An application can briefly perform more than `Y` requests per second, for example, when several calls arrive simultaneously in a Telephony integration. It is not possible to maintain such intensity constantly.

The values `X` and `Y` depend on the tariff plan.

#|
|| **Tariff** | **Counter reduction rate**,
`Y` | **Blocking threshold**,
`X` ||
|| Enterprise | 5 per second | 250 ||
|| Others | 2 per second | 50 ||
|#

{% note info "" %}

Request intensity is tracked separately for each Bitrix24. A limit triggered for an application in one Bitrix24 does not affect its operation in another Bitrix24.

If several applications send requests to the same Bitrix24 from the same IP address, they share a common counter and a common request rate limit.

{% endnote %}

### Executing a Large Number of Requests

To estimate the allowed volume of requests, use the counter reduction rate `Y` of your plan: 2 requests per second, or 5 on the Enterprise plan. At 2 requests per second, 172,800 requests can be performed per day. The limit mechanism allows for brief increases in intensity beyond this value.

The rate limit accounts for HTTP requests, not the number of records in the response. Therefore, you can use list methods `*.list` to retrieve data. For most such methods, the default page size is 50 records. For example, at an intensity of 2 requests per second with a page size of 50 records, 8,640,000 records can be retrieved per day.

If you need to retrieve large volumes of data via Bitrix24 REST API list methods, apply the [recommendations for fetching large volumes](settings/performance/huge-data.md). These describe paginated fetching without counting the total number of items.

To reduce the number of HTTP requests, use the [batch](settings/how-to-call-rest-api/batch.md) method. It allows you to pass up to 50 method calls in a single HTTP request.

If `batch` sequentially executes 50 calls to list methods and each call returns 50 records, one HTTP request will return up to 2,500 records. At an intensity of 2 HTTP requests per second, the calculated upper scenario per day would be 432,000,000 records. This is a mathematical example, not a guaranteed export volume: in practice, the result depends on the resource intensity of the methods, the volume of data, and the activity within Bitrix24.

`batch` reduces the number of HTTP requests but does not eliminate the resource intensity limits of the executed methods. The Bitrix24 REST API is not designed for mass or intensive exchange of large datasets. For reports and analytics on large volumes of data, Bitrix24 offers [BI Builder](https://helpdesk.bitrix24.com/open/25744889/): it retrieves Bitrix24 data through a separate mechanism rather than the REST API.

Choose an approach based on your task:

#|
|| **Task** | **Approach** ||
|| Regularly fetch data pages via REST API | List methods `*.list` with pagination ||
|| Reduce the number of HTTP requests during multiple independent REST calls | [batch](settings/how-to-call-rest-api/batch.md), but taking into account the resource intensity of nested methods ||
|| Build reports and analytics on large volumes of data | [BI Builder](https://helpdesk.bitrix24.com/open/25744889/) ||
|#

## Resource Intensity Limit

In the Bitrix24 cloud version, the `time` object in the response contains the `operating` key. It shows the accumulated execution time of a specific method. Resource intensity is tracked for webhooks and OAuth applications: separately for each application and each webhook.

Example response:

```json
{
    ...
    "time": {
        "start": 1778747192.910734,
        "finish": 1778747193.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2026-05-14T10:26:32+02:00",
        "date_finish": "2026-05-14T10:26:33+02:00",
        "operating_reset_at": 1778747760,
        "operating": 3.3076241016387939
    }
}
```

Method execution time is accounted for in 10 one-minute baskets. A basket is a calendar minute: it stores the sum of the method execution time for that minute. The calculation includes the current minute and the nine previous ones. Calls whose time, taking the weight into account, does not exceed 0.1 seconds are not included in `operating`. The weight is 1 for all methods except `batch`, which has half weight; see the `batch` paragraph below.

{% note info "" %}

If the accumulated method execution time exceeds the established limit, the next call to this method is blocked for the application or webhook for 5 minutes. Classic methods receive status `429` and error code `OPERATION_TIME_LIMIT`, REST 3.0 methods receive status `429`, code `BITRIX_REST_V3_EXCEPTION_RATELIMITEXCEPTION`, and a `Retry-After` header with the remaining block time. Other applications, webhooks, and methods continue to function.

{% endnote %}

The `operating_reset_at` key contains the time in `timestamp` format when the oldest minute basket is excluded from the calculation. A basket expires exactly at the start of the minute that comes 10 minutes after it. For example, if the first call in the 10:05 basket started at 10:05:20 on May 14, 2026, the basket stops being counted at 10:15:00, and `operating_reset_at` will correspond to 10:15:00.

When the time from `operating_reset_at` arrives, the sum of the time in the oldest basket is excluded from `operating`. The value may not decrease to zero: the baskets of subsequent minutes continue to be accounted for until their own reset time.

Bitrix24 checks the accumulated value before executing each call. The call after which the value exceeds the limit completes, and the next call is blocked. On blocking, the baskets are cleared, so after it the accumulated method time is counted from zero.

A `batch` call is accounted for in the `operating` of the `batch` method with half weight: in the cloud version, a batch that ran for 2 seconds increases `operating` by about 1 second. Nested calls are not counted or blocked individually: their time is included in the time of the whole batch. That is why the `result_time` of nested calls contains no `operating` or `operating_reset_at` keys.

In the cloud version, the resource intensity limit is 420 seconds. In the on-premise version, the restriction is disabled by default. The `rest` module administrative interface does not expose this setting: it configures only the SQL query time limit. The limit is set with module options: `load_limiter_active` enables the restriction, `load_limiter_second_limit` sets the limit in seconds, `load_limiter_connection_name` specifies the Redis connection. The options are set from PHP with the `\Bitrix\Main\Config\Option::set('rest', ...)` method. Without a Redis connection, time accumulation does not work.

Let us look at how the block triggers. An application calls the `crm.item.list` method, and each call takes 20 seconds:

- over the last 10 minutes, the application has performed 20 calls that are all still included in the calculation, so `operating` is 400 seconds
- then one more call completed, and `operating` reached 420 seconds — exactly the limit, so there is no block
- the next call completed and increased the value to 440 seconds, so the call after it will be blocked
- after 5 minutes, the block is lifted, and the accumulated time of `crm.item.list` for this application starts counting from zero

Metrics are calculated within Bitrix24. They are influenced by data volume, user activity, and other applications.

The same request in different Bitrix24 instances may return different `operating` values. For example, to obtain a result in one Bitrix24, 10 records may need to be processed, while in another, 10,000. The resource intensity of a request cannot be determined solely on the application side.

[Performance Recommendations](./settings/performance/index.md) will help reduce the risk of triggering resource consumption limits.

## How to Respond to Limit Errors

#|
|| **Error** | **HTTP status** | **Reason** | **What to do** ||
|| `QUERY_LIMIT_EXCEEDED` | `503` | Bitrix24 HTTP request intensity exceeded | Decrease request frequency and retry calls with increasing delay. Do not run multiple parallel series of requests from a single IP address ||
|| `BITRIX_REST_V3_EXCEPTION_LIMITER_QUERYTIMELIMITEXCEPTION` | `503` | An SQL query inside the method ran longer than the limit. Classic methods on module controllers return a generic internal error with status `400` instead of this code | Retry the call no earlier than after the time in the `Retry-After` header. Narrow the selection: add filters, reduce the page size, remove unnecessary fields ||
|| `OPERATION_TIME_LIMIT` | `429` | Accumulated method execution time has exceeded the limit for the application or webhook, classic methods | Suspend calls to the blocked method for the same application or webhook for 5 minutes, then retry the call. To prevent the block from recurring, reduce the load: split the selection, remove unnecessary fields, monitor `operating` in responses. There is no `Retry-After` header in the response ||
|| `BITRIX_REST_V3_EXCEPTION_RATELIMITEXCEPTION` | `429` | The same block in REST 3.0 methods | Wait for the time from the `Retry-After` header: it contains the remaining block time. Then reduce the load on the method ||
|#

The full list of REST API error codes is provided in the [Error Codes](error-codes.md) article.
