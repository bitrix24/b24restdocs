# Performance Recommendations

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The load that an integration puts on Bitrix24 consists of two values: how many HTTP requests it sends and how many resources each called method consumes. The recommendations below help you keep both values within the limits and avoid losing events under peak load. The threshold values themselves and the error codes are described on the [REST API Limits](./limits.md) page.

In cloud Bitrix24, the limit thresholds depend on the plan. In the [self-hosted version](../cloud-and-on-premise/on-premise/index.md), the restrictions depend on the settings of the server Bitrix24 runs on, so they can be either stricter or looser than the cloud ones.

## How to Reduce the Number of Requests {#requests}

The rate limit counts HTTP requests, not the number of records in the response. Reduce the number of calls to Bitrix24.

**Cache data on your side.** Field lists, dictionaries, and settings change rarely. Retain them on your side so that you do not request the same data again.

**Combine calls into a batch.** The [batch](../how-to-call-rest-api/batch.md) method passes up to 50 method calls in a single HTTP request, so it consumes the rate limit as one request.

**Request only the fields you need.** Pass the list of fields in the `select` parameter for those methods that support it.

**Turn off counting the total.** When retrieving large volumes, pass `start = -1` and move to the next page by filtering on the last received identifier. The procedure is described in the article [How to Retrieve Large Volumes of Data](./huge-data.md).

## How to Reduce the Resource Intensity of Calls {#resources}

Bitrix24 counts not only the rate of requests but also their execution time. In the cloud version, a single request runs for no longer than 60 seconds, and the accumulated execution time of a method is limited separately for each application and webhook.

**Split mass operations.** Several small requests are safer than one heavy call that can be interrupted by a timeout.

**Simplify filters and selections.** The execution time of a method depends directly on the volume of data and the complexity of the filter.

**Watch the time in the response.** In the [`time`](../../api-reference/data-types.md#time) object, the `duration` field shows the time of the current request, and `operating` shows the accumulated time of calls to this method over the last 10 minutes.

**Keep nested calls in mind.** The `batch` method reduces the number of HTTP requests but does not reduce the resource intensity of the methods inside the batch.

If calls are already blocked by a limit, repeat them with an increasing delay. For the breakdown of the `QUERY_LIMIT_EXCEEDED` and `OPERATION_TIME_LIMIT` errors, see the article [REST API Limits](./limits.md#how-to-respond-to-limit-errors).

## How to Handle Events and Outgoing Webhooks {#events}

[Events](../../api-reference/events/index.md) are delivered by a separate service — the event queue. It calls the handler that your integration has registered. The rate of these calls is driven by user actions, and you cannot regulate it.

For example, an application has registered a handler for the [OnCRMDealUpdate](../../api-reference/crm/deals/events/on-crm-deal-update.md) event, and users have massively updated 10,000 deals — with a [workflow](../../api-reference/bizproc/index.md) or automation based on the REST API. The event queue will immediately generate 10,000 tasks to call your handler.

This leads to two requirements for the handler.

**Respond quickly.** If the event queue receives no response or the handler responds slowly, subsequent calls are executed with lower priority and longer delays.

**Withstand peak load.** Bitrix24 does not resend an event if the handler did not respond or returned an error status from the web server. Some of the events will be lost in this case.

To meet both requirements, accept events into an [incoming queue](./queue.md). The handler immediately responds to Bitrix24 that the request has been accepted and retains the request data in your queue. Separate workers — one or several — process that queue. This design keeps the response time short, lets you manage the load on your side, and makes the processing scalable.

## Continue Learning

- [{#T}](./limits.md)
- [{#T}](./huge-data.md)
- [{#T}](./queue.md)
- [{#T}](../how-to-call-rest-api/batch.md)
- [{#T}](../../api-reference/events/index.md)