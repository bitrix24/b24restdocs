# Features of List Methods

In the REST API, there are several methods that return lists of entities—lists of deals, users, task comments, etc.

Since the number of returned entities depends on specific conditions and parameters, *Bitrix24* returns them in "batches" of several items (currently no more than 50 at a time).

Example:

```http
https://your-domain.bitrix24.com/rest/crm.item.list?entityTypeId=183&auth=d161f25928c3184678924ec127edd29a

//get a list of all custom CRM entities with ID 183 in JSON format.
```

When calling list methods, REST returns additional values in the response:

```json
{
    "result": result of the method execution,
    "error": error of the method execution,
    "total": total number of records in the response of the list method,
    "next": value to be sent to get the next page of data from the list method
}
```

To retrieve the next batch of entities, you need to execute the same request, specifying an additional parameter `start` with the value received in the `next` parameter of the response. When using [SDK BX24.js](../bx24-js-sdk/index.md), obtaining the next batch is done using the method `result.next()`.

Executing list methods can be quite "heavy" in terms of performance for *Bitrix24*, as the volume of processed data can be very large. To ensure your applications stay within the [limits of the REST API](../performance/limits.md), we recommend [reading the article](../performance/huge-data.md).