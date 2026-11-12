# How to Retrieve Large Volumes of Data

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

If you need to retrieve many items with a list method that uses standard REST navigation, do not page through the results by incrementing `start = start + 50`. With `start >= 0`, the method returns a page of data and also calculates the total number of items for the `total` field. On large datasets or with complex filters, this calculation can be slow.

Instead, pass `start = -1` and select the next page with a filter by the last received identifier. This way, the method does not calculate the total number of items, and the load on Bitrix24 decreases.

For a single page, a filter by identifier is not required: pass `start = -1`, and the method returns up to 50 items without calculating the total.

Regular navigation of list methods via `next` and `start` fields is described in the article [List Method Specifics](../how-to-call-rest-api/list-methods-pecularities.md).

## How to Tell Whether a Method Supports start = -1 {#start-minus-one}

Standard REST navigation is used by list methods whose page describes the `start` parameter and whose response contains the `total` and `next` fields. For example, [crm.item.list](../../api-reference/crm/universal/crm-item-list.md), [crm.deal.list](../../api-reference/crm/deals/crm-deal-list.md), [tasks.task.list](../../api-reference/tasks/tasks-task-list.md), [user.get](../../api-reference/user/user-get.md).

Check the method with a single call with `start = -1` on a selection of more than 50 items. If the method supports this mode, the response contains no `next` field, and `total` is `0` or absent. REST 3.0 methods use a different navigation mechanism, and this approach does not apply to them.

## How to Select All Items

To sequentially retrieve all items by filter, use methods that support sorting by a stable numeric identifier.

1. Sort items by identifier in ascending order
2. Add a `>ID` or `>id` condition to the filter with the value of the last received identifier
3. Pass `start = -1` to disable the total count calculation
4. After each request, retain the identifier of the last item from the response
5. Repeat the request until the method returns fewer than 50 items

This loop does not need the `next` field: the next page is defined by the identifier filter.

## Considerations for Importing

Leave a delay between requests to avoid exceeding [REST API limits](limits.md). If a method returns a limit error, decrease the request frequency and call again later.

{% note info "" %}

Apply this approach only to methods that, with `start = -1`, apply the filter and sorting by identifier in the same way as with standard REST navigation. If items are deleted or access permissions change during the import, the selection set may change between requests.

{% endnote %}

## Example

The PHP example exports all leads with the [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) method: `entityTypeId = 1` is the Lead object type. It requires the cURL extension and an [inbound webhook](../../local-integrations/local-webhooks.md) with the `crm` scope created by a user who can read leads. If a request returns an error, the loop stops, and `$lastId` keeps the last processed identifier so you can resume from it.

Other methods may use different field names and a different path to the list in the response. For example, [crm.lead.list](../../api-reference/crm/leads/crm-lead-list.md) returns items directly in `result`, and the identifier field is `ID`.

```php
$webhook = 'https://example.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/'; // inbound webhook URL
$lastId = 0; // or the ID where the previous import stopped

do {
    $ch = curl_init($webhook . 'crm.item.list');
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query([
        'entityTypeId' => 1,
        'order' => ['id' => 'ASC'],
        'filter' => ['>id' => $lastId],
        'select' => ['id', 'title'],
        'start' => -1,
    ]));
    $result = json_decode((string)curl_exec($ch), true);
    curl_close($ch);

    if (!isset($result['result']['items'])) {
        // method error code, or a sign of a network error or a non-JSON response
        echo 'Error ', $result['error'] ?? 'no response', ', last ID: ', $lastId, PHP_EOL;
        break;
    }

    $items = $result['result']['items'];
    foreach ($items as $item) {
        // process the item
        $lastId = $item['id'];
    }

    usleep(500000); // pause to stay within the limits
} while (count($items) === 50);
```

How much the count slows down a request is shown by a [crm.lead.list](../../api-reference/crm/leads/crm-lead-list.md) call on 2,387,743 leads with identical permissions and filter. The request took 49.9 seconds with the total count calculation and 0.098 seconds without it. The response fields related to navigation and time:

```text
start = 0:  next = 50, total = 2387743, time.duration = 49.916
start = -1: total = 0, time.duration = 0.098
```

## Continue Learning

- [{#T}](./limits.md)
- [{#T}](./index.md)
- [{#T}](../how-to-call-rest-api/batch.md)
- [{#T}](./queue.md)
