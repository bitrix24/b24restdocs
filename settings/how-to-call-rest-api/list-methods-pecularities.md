# Features of List Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

List methods return sets of items of the same type, such as deals, users, or task comments. The result can span multiple pages. This page explains how to retrieve pages sequentially using the `start` parameter and the `next` field, and which approach to choose for large data volumes.

## How Pagination Works

Bitrix24 returns no more than 50 items per page.

To retrieve all items:

1. Call the list method without the `start` parameter to retrieve the first page
2. If the response contains the `next` field, pass its value in the `start` parameter of the next request
3. Repeat the request until the `next` field is absent. Its absence means that the last page has been retrieved

## Request and Response Example

In this example, `crm.item.list` requests the first page of SPA items with the ID `183`:

```http
https://your-domain.bitrix24.com/rest/crm.item.list?entityTypeId=183&auth=YOUR_ACCESS_TOKEN
```

The `next` field in the response contains the value for requesting the next page:

```json
{
    "result": {
        "items": [
            {
                "id": 101
            },
            {
                "id": 102
            }
        ]
    },
    "total": 73,
    "next": 50
}
```

{% note warning "" %}

Replace `YOUR_ACCESS_TOKEN` with an authorization token. Do not publish the token or store it in the application source code.

{% endnote %}

## Response Fields

List method calls return the following REST fields:

#|
|| **Field** | **Type** | **When Returned** | **Description** ||
|| `result` | `array` or `object` | In a successful response | Method result. The structure depends on the specific method ||
|| `error` | `string` | In an error response | Method error code ||
|| `total` | `integer` | In a successful list method response if the method counts the total number of items | Total number of items matching the request conditions ||
|| `next` | `integer` | If another page follows the current page | Value for the `start` parameter of the next request. The field is not returned on the last page ||
|#

## How to Retrieve the Next Page

Repeat the first request and pass the value `50` from `next` in the `start` parameter:

```http
https://your-domain.bitrix24.com/rest/crm.item.list?entityTypeId=183&start=50&auth=YOUR_ACCESS_TOKEN
```

How you retrieve the next page depends on the tool:

- when calling the REST API directly, pass the `next` value in the `start` parameter
- when using the [BX24.js SDK](../../sdk/bx24-js-sdk/index.md), call `result.next()`. The SDK creates the next-page request

## Limits and Large Data Volumes

Standard pagination is suitable for small data sets and one-time operations. Processing many items creates additional load and can exceed the [REST API limits](../performance/limits.md).

For regular exports of large data volumes, follow the recommendations in [How to Retrieve Large Data Volumes from Bitrix24](../performance/huge-data.md).

## Continue Exploring

- [{#T}](./index.md)
- [{#T}](../performance/limits.md)
- [{#T}](../performance/huge-data.md)
- [{#T}](../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-method.md)
