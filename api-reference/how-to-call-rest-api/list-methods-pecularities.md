# Features of List Methods

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- add a link to performance

{% endnote %}

{% endif %}

In the REST API, there are several methods that return lists of entities - lists of deals, users, task comments, etc.

Since the number of returned entities depends on specific conditions and parameters, Bitrix24 returns them in "batches" of several items (currently no more than 50).

Example:

```http
https://your-domain.com/rest/crm.item.list?entityTypeId=183&auth=d161f25928c3184678924ec127edd29a

//get a json formatted list of all CRM custom entities with ID 183.
```

When calling list methods, REST returns additional values in the response:

```json
{
    "result": method execution result,
    "error": method execution error,
    "total": total number of records in the list method response,
    "next": value to send to get the next page of data from the list method
}
```

To retrieve the next batch of entities, you need to perform the same request, specifying an additional parameter `start` with the value received in the `next` parameter of the response. When using [SDK BX24.js](../bx24-js-sdk/index.md), obtaining the next batch is done using the `result.next()` method.

Executing list methods can be quite "heavy" in terms of Bitrix24 performance, as the volume of processed data can be very large. To ensure your applications stay within the [REST API limits](../performance/limits.md), we recommend [reading the article](../performance/huge-data.md).