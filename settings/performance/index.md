# General Recommendations

There are certain [limits](./limits.md) when working with the REST API of the cloud version of Bitrix24. In the case of [on-premise installation](../cloud-and-on-premise/on-premise/index.md), the limitations depend on the specific server infrastructure settings on which Bitrix24 operates, meaning that the limits can vary both upwards and downwards.

However, there are several general recommendations that, if followed, will reduce the risk of your applications hitting the REST API limits.

## Requests to Bitrix24

1. Reduce the number of requests to Bitrix24:
   1. Cache some data on your side to avoid unnecessary repeated requests that return the same data you have already received;
   2. Use batch execution of requests with the [batch method](../how-to-call-rest-api/batch.md) (the limit on request intensity takes into account the number of hits to Bitrix24, and using batch allows you to make several actual REST API requests in one hit).
2. Use the [start=-1 parameter](./huge-data.md) when calling list methods - this reduces the load on Bitrix24's server resources.
3. Try to request only the data that is actually needed (specify the list of required fields in the `select` parameter for those methods that support it).

## Event Handling and Outgoing Webhooks

As you know, the [event mechanism](../../api-reference/events/index.md) in Bitrix24 operates using a special service - the event queue - which invokes your handlers.

It is important to consider that you cannot control the intensity of events.

Imagine that your application has registered a handler for [OnCRMDealUpdate](../../api-reference/crm/deals/events/on-crm-deal-update.md), and users on a specific Bitrix24 instance have massively (using a [workflow](../../api-reference/bizproc/index.md) or automation based on the REST API) updated 10K deals. The Bitrix24 event queue will immediately generate tasks to send 10K notifications to your handler.

Therefore, your handler must be prepared for potential peak loads. It should respond as quickly as possible - if the event queue does not receive a response from your handler when sending the next event, or if your handler responds slowly, subsequent calls will be executed with lower priority, resulting in longer delays.

Most importantly, if your handler cannot handle such a load and your server "crashes," you will simply not receive and will be unable to process some of the events sent to your handler. Bitrix24 does not resend events if the event handler does not respond at all or returns an error status from the web server.

Thus, we **strongly recommend** using the "[incoming queue](./queue.md)" for event processing. The concept of an “incoming queue” for handling HTTP requests implies that requests are not processed immediately upon arrival but are placed in a queue, from which they are subsequently retrieved and processed by your server or even a group of servers. This allows for better load management, increased stability, and scalability of the system.