# REST API Limits

In the Bitrix24 cloud version, rate and resource intensity limits apply to REST requests. Rate is how often an application sends requests, and resource intensity is how long its methods take to execute. In the self-hosted version, the load is configured on the server side.

The limits distribute the load across Bitrix24 instances so that intensive automation in one Bitrix24 does not interfere with other service users.

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

## Request Execution Time Limit

In the Bitrix24 cloud version, one REST request must finish within 60 seconds. If processing takes longer, the request is interrupted by timeout.

Request duration can be affected by the data volume, filter complexity, nested calls in [batch](../how-to-call-rest-api/batch.md), and automation that is launched when the method is executed.

To reduce the timeout risk:

- split large operations into several requests
- use pagination and do not request unnecessary fields
- move long-running scenarios to an external queue or background processing on the application side
- check the `duration` field in the response [`time`](../../api-reference/data-types.md#time) object

In the Bitrix24 self-hosted version, request execution time depends on server settings.

### SQL Query Time Limit

Bitrix24 limits the execution time of a single SQL query inside a REST method. The limit applies to classic methods and REST 3.0 methods. In the cloud version, the limit value is set by the Bitrix24 configuration. In the self-hosted version, it is disabled by default. An administrator enables it in the `rest` module settings and sets the limit in seconds, 30 seconds by default. The setting is available only if the database supports it.

If an SQL query does not fit within the limit, the method returns the `BITRIX_REST_V3_EXCEPTION_LIMITER_QUERYTIMELIMITEXCEPTION` error with the text "Request canceled: query time limit exceeded" and HTTP status 503. The `Retry-After` header contains the limit in seconds. Classic methods based on Engine controllers return HTTP status 400 without `Retry-After`. In [batch](../how-to-call-rest-api/batch.md), the error is returned in the result of the subrequest that exceeded the limit.

## Request Rate Limits

The Bitrix24 request rate limit operates on the Leaky Bucket Algorithm principle. An application can perform intensive requests briefly, but it cannot maintain such intensity constantly.

The mechanism works as follows:

1. Each application request increases a virtual request counter in Bitrix24.
2. Once per second, the counter decreases by `Y`.
3. When the counter value exceeds the threshold `X`, the following request is blocked with status `503` and error code `QUERY_LIMIT_EXCEEDED`.

This leads to two rules:

1. If all requests from one IP address to Bitrix24 are sent at a rate of no more than `Y` per second, the `QUERY_LIMIT_EXCEEDED` error does not occur.
2. An application can briefly perform more than `Y` requests per second, for example, when several calls arrive simultaneously in a Telephony integration. It is not possible to maintain such intensity constantly.

The values `X` and `Y` depend on the tariff plan.

#|
|| **Tariff** | **Counter reduction rate**,
`Y` | **Limit before blocking**,
`X` ||
|| Enterprise | 5 per second | 250 ||
|| Others | 2 per second | 50 ||
|#

{% note info "" %}

Request intensity is tracked separately for each Bitrix24. A limit triggered for an application in one Bitrix24 does not affect its operation in another Bitrix24.

Bitrix24 takes into account the IP address a request comes from. If several applications on a single server work with the same Bitrix24, they share a common rate limit.

{% endnote %}

### Executing a Large Number of Requests

To estimate how many requests you can perform, calculate based on the minimum sustainable intensity of 2 requests per second. That is 172,800 requests per day. For brief periods, the intensity can be higher.

The rate limit accounts for HTTP requests, not records in the response. List methods `*.list` return one page of records per request, which is 50 records by default for many methods. At 2 requests per second, that is 8,640,000 records per day.

If you need to retrieve large volumes of data via Bitrix24 REST API list methods, apply the [recommendations for fetching large volumes](huge-data.md). These describe paginated fetching without counting the total number of items.

To reduce the number of HTTP requests, use the [batch](../how-to-call-rest-api/batch.md) method. It passes up to 50 method calls in a single HTTP request.

If `batch` sequentially executes 50 calls to list methods and each returns 50 records, one HTTP request will return up to 2,500 records. At 2 HTTP requests per second, that is up to 432,000,000 records per day. This is an estimate, not a guaranteed export volume: in practice, the result depends on the resource intensity of the methods, the volume of data, and the activity within Bitrix24.

`batch` reduces the number of HTTP requests but does not eliminate the resource intensity limits of the executed methods. The Bitrix24 REST API is not designed for mass or intensive exchange of large datasets. For reports and analytics on large volumes of data, Bitrix24 offers [BI Builder](https://helpdesk.bitrix24.com/open/25744889/): it retrieves Bitrix24 data through a separate mechanism rather than the REST API.

Choose an approach based on your task:

#|
|| **Task** | **Approach** ||
|| Regularly fetch data pages via REST API | List methods `*.list` with pagination ||
|| Reduce the number of HTTP requests during multiple independent REST calls | [batch](../how-to-call-rest-api/batch.md), but taking into account the resource intensity of nested methods ||
|| Build reports and analytics on large volumes of data | [BI Builder](https://helpdesk.bitrix24.com/open/25744889/) ||
|#

## Resource Intensity Limits

Resource intensity is the accumulated execution time of a method, separately for each application or webhook. In the Bitrix24 cloud version, it is shown by the `operating` key in the `time` object of the response.

Response fragment, the `time` object:

```json
{
    "time": {
        "start": 1778747192.910734,
        "finish": 1778747193.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2026-05-14T10:26:32+02:00",
        "date_finish": "2026-05-14T10:26:33+02:00",
        "operating_reset_at": 1778747792,
        "operating": 3.3076241016387939
    }
}
```

Method execution time is accounted for in 10 one-minute baskets. A basket is a calendar minute: it stores the sum of the method execution time for that minute. `operating` includes the current minute and the nine previous ones.

{% note info "" %}

If the accumulated method execution time exceeds the established limit, the method is blocked for the application or webhook for 5 minutes. During this time, Bitrix24 responds to calls with the `429` status and error code `OPERATION_TIME_LIMIT`. Other applications, webhooks, and methods continue to function.

{% endnote %}

A basket leaves the calculation when the tenth minute after it starts. The `operating_reset_at` key is the time in `timestamp` format when Bitrix24 deletes the oldest basket: 10 minutes after the first call recorded in it. For example, if the first call in the 10:05 basket started at 10:05:20, the basket stops being counted at 10:15:00, and `operating_reset_at` will correspond to 10:15:20. After `operating_reset_at`, this basket is guaranteed to be excluded from the calculation.

At the same time, `operating` does not necessarily drop to zero: the baskets of subsequent minutes remain in the calculation.

Bitrix24 checks the accumulated time before each call. The call during which the time exceeds the limit runs to completion. The next call is blocked with error `OPERATION_TIME_LIMIT`, and the method remains blocked for 5 minutes, even if old baskets leave the calculation during this time. After the block, the accumulated time of the method is counted from zero.

A `batch` call is accounted for in the `operating` of the `batch` method with half weight: in the cloud, a batch that ran for 2 seconds increases `operating` by about 1 second. Nested calls are executed as separate methods, so the execution time of each is accounted for and checked against its own limit.

For example, assume the established limit is 480 seconds. An application calls the [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) method, and each call takes 20 seconds:

- over the last 10 minutes, the application has performed 20 calls that are all still included in the calculation, so `operating` is 400 seconds
- then five more calls are completed, and `operating` reaches 500 seconds
- the next call will be blocked because the value already exceeds 480 seconds before its execution
- after 5 minutes, the block is lifted, and the accumulated time of `crm.item.list` for this application starts counting from zero

The limit value in the cloud version is set by the Bitrix24 configuration. In the self-hosted version, the restriction is disabled by default. If enabled, the default limit will be 420 seconds. The restriction works only with a Redis connection. The restriction is configured with the `rest` module options:

- `load_limiter_active` — enables the restriction
- `load_limiter_second_limit` — the limit in seconds
- `load_limiter_connection_name` — the Redis connection

Resource intensity metrics are calculated by Bitrix24. They are influenced by data volume, user activity, and other applications.

The same request in different Bitrix24 instances may return different `operating` values: in one, 10 records need to be processed to get the result, in another, 10,000. The resource intensity of a request cannot be determined solely on the application side.

[Performance Recommendations](./index.md) will help reduce the risk of triggering resource consumption limits.

## How to Respond to Limit Errors

#|
|| **Error** | **Cause** | **What to do** ||
|| `QUERY_LIMIT_EXCEEDED` | Bitrix24 HTTP request intensity exceeded | Decrease request frequency and retry calls with increasing delay. Do not run multiple parallel series of requests from a single IP address ||
|| `BITRIX_REST_V3_EXCEPTION_LIMITER_QUERYTIMELIMITEXCEPTION` | An SQL query inside the method ran longer than the limit | Retry the call no earlier than after the time in the `Retry-After` header. If the header is absent, retry with increasing delay. Narrow the selection: add filters, reduce the page size, remove unnecessary fields ||
|| `OPERATION_TIME_LIMIT` | Accumulated method execution time has exceeded the limit for the application or webhook | Suspend calls to the blocked method for the same application or webhook for 5 minutes, then retry the call. To prevent the block from recurring, reduce the load: split the selection, remove unnecessary fields, monitor `operating` in responses ||
|#
