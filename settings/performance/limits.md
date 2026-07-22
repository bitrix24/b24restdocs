# REST API Limits

REST request intensity and resource consumption limits apply to the Bitrix24 cloud version. In the self-hosted version, the load is configured on the server side.

These restrictions distribute the total load across Bitrix24. They prevent situations where intensive automation in one Bitrix24 instance affects the performance of other service users.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

## Request Rate Limits

The Bitrix24 request rate limit operates on the Leaky Bucket Algorithm principle. An application can perform intensive requests briefly, but it cannot maintain such intensity constantly.

The mechanism works as follows:

1. Each application request increases a virtual request counter in Bitrix24.
2. When the counter value exceeds the threshold `X`, the following request is blocked with status `503` and error code `QUERY_LIMIT_EXCEEDED`.
3. The counter value automatically decreases by `Y` once per second.

This leads to two rules:

1. At an intensity of no more than `Y` requests per second, the application will not receive error `QUERY_LIMIT_EXCEEDED`.
2. An application can briefly perform more than `Y` requests per second, for example, when several calls arrive simultaneously in a Telephony integration. It is not possible to maintain such intensity constantly.

The threshold values `X` and `Y` mentioned above depend on the tariff plan.

#|
|| **Tariff** | **Counter reduction rate**,
`Y` | **Limit before blocking**,
`X` ||
|| Enterprise | 5 per sec | 250 ||
|| Others | 2 per sec | 50 ||
|#

{% note info "" %}

Request intensity is tracked separately for each Bitrix24. A limit triggered for an application in one Bitrix24 does not affect its operation in another Bitrix24.

When executing REST requests, Bitrix24 considers the IP address of the request source. If several applications on a single server work with the same Bitrix24, they share a common request rate limit.

{% endnote %}

### Executing a Large Number of Requests

To estimate the allowable volume of requests, use the minimum sustainable intensity as a baseline calculation. At an intensity of 2 requests per second, 172,800 requests can be performed per day. The limit mechanism allows for brief increases in intensity beyond this value.

The rate limit accounts for HTTP requests, not the number of records in the response. Therefore, you can use list methods `*.list` to retrieve data. For many such methods, the default page size is 50 records. For example, at an intensity of 2 requests per second with a page size of 50 records, 8,640,000 records can be retrieved per day.

If you need to retrieve large volumes of data via Bitrix24 REST API list methods, apply the [recommendations for fetching large volumes](huge-data.md). These describe paginated fetching without counting the total number of items.

To reduce the number of HTTP requests, use the [batch](../how-to-call-rest-api/batch.md) method. It allows you to pass up to 50 method calls in a single HTTP request.

If `batch` sequentially executes 50 calls to list methods and each call returns 50 records, one HTTP request will return up to 2,500 records. At an intensity of 2 HTTP requests per second, the calculated upper scenario per day would be 432,000,000 records. This is a mathematical example, not a guaranteed export volume: in practice, the result depends on the resource intensity of the methods, the volume of data, and the activity within Bitrix24.

`batch` reduces the number of HTTP requests but does not eliminate the resource intensity limits of the executed methods. The Bitrix24 REST API is not designed for mass or intensive exchange of large datasets. Therefore, for the built-in BI connector, which is intended for retrieving large amounts of data, Bitrix24 uses a different mechanism.

Choose an approach based on your task:

#|
|| **Task** | **Approach** ||
|| Regularly fetch data pages via REST API | List methods `*.list` with pagination ||
|| Reduce the number of HTTP requests during multiple independent REST calls | [batch](../how-to-call-rest-api/batch.md), but taking into account the resource intensity of nested methods ||
|#

## Resource Intensity Limits

In the Bitrix24 cloud version, the `time` array in the response contains the `operating` key. It shows the accumulated execution time of a specific method separately for an application or a webhook.

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
        "operating_reset_at": 1778747792,
        "operating": 3.3076241016387939
    }
}
```

Method execution time is accounted for in 10 one-minute baskets. Each basket stores the sum of the method execution time for the corresponding minute.

{% note info "" %}

If the accumulated method execution time exceeds the established limit, the next call to this method is blocked for the application or webhook. In response, Bitrix24 returns the `429` status and error code `OPERATION_TIME_LIMIT`. Other applications, webhooks, and methods continue to function.

{% endnote %}

The `operating_reset_at` key contains the time in `timestamp` format, when the oldest minute basket is excluded from the calculation. The countdown for the basket starts from the first recorded method call in the corresponding minute and lasts 10 minutes. For example, if the first call in the basket started at 10:05:20 on May 14, 2026, the value `operating_reset_at` will correspond to 10:15:20 on May 14, 2026.

When the time from `operating_reset_at` arrives, the sum of the time in the oldest basket is excluded from `operating`. The value may not decrease to zero: the baskets of subsequent minutes continue to be accounted for until their own reset time.

Bitrix24 checks the accumulated value before executing each call. The call after which the value exceeds the limit is terminated. The next call is blocked with error `OPERATION_TIME_LIMIT`. It will become available again when, after resetting one or more baskets, the value `operating` no longer exceeds the limit.

### How to Respond to Limit Errors

#|
|| **Error** | **Cause** | **What to do** ||
|| `QUERY_LIMIT_EXCEEDED` | Bitrix24 HTTP request intensity exceeded | Decrease request frequency and retry calls with increasing delay. Do not run multiple parallel series of requests from a single IP address ||
|| `OPERATION_TIME_LIMIT` | Accumulated method execution time has exceeded the limit for the application or webhook | Suspend calls to the blocked method for the same application or webhook. For the first retry, refer to `operating_reset_at` from the last successful response. If the error persists, wait for the next bucket reset ||
|#

An external call to `batch` is not accounted for in `operating`. Nested calls are executed as separate methods, so the execution time of each is accounted for and checked against its own limit.

For example, assume the established limit is 480 seconds. An application calls method `crm.item.list`, and each call takes 20 seconds:

- in 10 minutes, the application has performed 20 calls, so `operating` is 400 seconds
- then five more calls are completed, and `operating` reaches 500 seconds
- the next call will be blocked because the value already exceeds 480 seconds before its execution
- when the oldest basket, which has accumulated 40 seconds, is excluded from the calculation, `operating` will decrease to 460 seconds and calls will become available again

The limit value in the cloud version is set by the Bitrix24 configuration. In the self-hosted version, the default restriction is disabled. If enabled, the default limit will be 420 seconds. An Administrator can change this value in the `rest` module settings.

Metrics are calculated within Bitrix24. They are influenced by data volume, user activity, and other applications.

The same request in different Bitrix24 instances may return different `operating` values. For example, to obtain a result in one Bitrix24, 10 records may need to be processed, while in another, 10,000. The resource intensity of a request cannot be determined solely on the application side.

[Performance Recommendations](./index.md) will help reduce the risk of triggering resource consumption limits.
