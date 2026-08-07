# Get a List of Follow-up Calls call.followup.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`call`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

The method belongs to REST 3.0. Details regarding call specifics and the response format of the new API version are described in the [REST 3.0 Overview](../../rest-v3.md).

{% endnote %}

The `call.followup.list` method returns a list of follow-up calls for the specified period.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../data-types.md) | Selection criteria [(detailed description)](#filter) ||
|| **select**
[`array`](../../data-types.md) | List of fields and nested paths to be returned in the list items.

If the parameter is not passed or an empty array is passed, the method returns only basic metadata: `callId`, `callType`, `initiatorId`, `startDate`, `endDate`, `durationSeconds`.

In `select`, you can pass root Follow-up fields, AI blocks, or available nested paths via dot notation. For a full list of fields and available nested paths, see the article [Follow-up call fields](./fields.md#select-paths).

Fields `transcription`, `overview`, and `insights` are considered heavy. If they are present in `select`, server will limit `pagination.limit` with value `20` ||
|| **order**
[`object`](../../data-types.md) | Sorting parameters [(detailed description)](#order).

Default: `{ "startDate": "desc" }` ||
|| **pagination**
[`object`](../../data-types.md) | Cursor pagination parameters [(detailed description)](#pagination) ||
|| **mentionFormat**
[`string`](../../data-types.md) | Format of user mentions in text AI fields.

Possible values:

- `bb` — BBCode format
- `html` — HTML format
- `none` — plain text without mention markup

Default: `bb` ||
|#

### Parameter filter {#filter}

#|
|| **Name**
`type` | **Description** ||
|| **startDate***
[`object`](../../data-types.md) | Call start period [(detailed description)](#startdate) ||
|| **participantId**
[`integer`](../../data-types.md) | Call participant identifier.

An Administrator can obtain Follow-ups for any user. For a regular user, the filter is forcibly restricted to their identifier ||
|#

#### Parameter filter.startDate {#startdate}

#|
|| **Name**
`type` | **Description** ||
|| **from***
[`string`](../../data-types.md) | Period start in ISO 8601 format. For example: `2026-01-01T00:00:00Z` ||
|| **to***
[`string`](../../data-types.md) | Period end in ISO 8601 format. The value must be greater than or equal to `from` ||
|#

### Parameter order {#order}

#|
|| **Name**
`type` | **Description** ||
|| **startDate**
[`string`](../../data-types.md) | Sort direction by call start date.

Possible values:

- `asc` — ascending
- `desc` — descending

Default: `desc` ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Page size.

Default: `50`. Maximum: `200` for light selection and `20` for selection with heavy AI fields. If a value greater than the maximum is passed, the server will apply the maximum value ||
|| **afterCursor**
[`object`](../../data-types.md) | Next page cursor. Pass the `afterCursor` value in its entirety from the previous response in the same format it was received [(detailed description)](#aftercursor) ||
|#

#### Parameter pagination.afterCursor {#aftercursor}

To retrieve all pages:

1. Send the first request without `pagination.afterCursor`
2. If `hasMore` in the response equals `true`, copy the `afterCursor` object from the response into the `pagination.afterCursor` of the next request
3. Repeat requests until `hasMore` equals `false`

#|
|| **Name**
`type` | **Description** ||
|| **startDate***
[`string`](../../data-types.md) | Start date of the last item of the previous page ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the last item of the previous page ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

Calling the new API differs by adding the `/api/` parameter to the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/call.followup.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"startDate":{"from":"2026-01-01T00:00:00Z","to":"2026-01-31T23:59:59Z"}},"select":["callId","startDate","participants","overview.topic","overview.actionItems"],"order":{"startDate":"desc"},"pagination":{"limit":20},"mentionFormat":"html"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/call.followup.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"startDate":{"from":"2026-01-01T00:00:00Z","to":"2026-01-31T23:59:59Z"}},"select":["callId","startDate","participants","overview.topic","overview.actionItems"],"order":{"startDate":"desc"},"pagination":{"limit":20},"mentionFormat":"html","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/call.followup.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FollowUpListResult = {
      items: Array<{
        callId: number
        startDate: string
        participants?: unknown[]
        overview?: { topic?: string, actionItems?: unknown[] }
      }>
      hasMore: boolean
      afterCursor: { startDate: string, id: number } | null
    }

    try {
      const response = await $b24.actions.v3.call.make<FollowUpListResult>({
        method: 'call.followup.list',
        params: {
          filter: {
            startDate: {
              from: '2026-01-01T00:00:00Z',
              to: '2026-01-31T23:59:59Z',
            },
          },
          select: ['callId', 'startDate', 'participants', 'overview.topic', 'overview.actionItems'],
          order: { startDate: 'desc' },
          pagination: { limit: 20 },
          mentionFormat: 'html',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.items, result.afterCursor)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getFollowUpList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'call.followup.list',
            params: {
              filter: {
                startDate: {
                  from: '2026-01-01T00:00:00Z',
                  to: '2026-01-31T23:59:59Z',
                },
              },
              select: ['callId', 'startDate', 'participants', 'overview.topic', 'overview.actionItems'],
              order: { startDate: 'desc' },
              pagination: { limit: 20 },
              mentionFormat: 'html',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Follow-ups found:', result.items.length, result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getFollowUpList)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'call.followup.list',
                [
                    'filter' => [
                        'startDate' => [
                            'from' => '2026-01-01T00:00:00Z',
                            'to' => '2026-01-31T23:59:59Z',
                        ],
                    ],
                    'select' => ['callId', 'startDate', 'participants', 'overview.topic', 'overview.actionItems'],
                    'order' => ['startDate' => 'desc'],
                    'pagination' => ['limit' => 20],
                    'mentionFormat' => 'html',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'call.followup.list',
        {
            filter: {
                startDate: {
                    from: '2026-01-01T00:00:00Z',
                    to: '2026-01-31T23:59:59Z'
                }
            },
            select: ['callId', 'startDate', 'participants', 'overview.topic', 'overview.actionItems'],
            order: { startDate: 'desc' },
            pagination: { limit: 20 },
            mentionFormat: 'html'
        },
        function(result) {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'call.followup.list',
        [
            'filter' => [
                'startDate' => [
                    'from' => '2026-01-01T00:00:00Z',
                    'to' => '2026-01-31T23:59:59Z',
                ],
            ],
            'select' => ['callId', 'startDate', 'participants', 'overview.topic', 'overview.actionItems'],
            'order' => ['startDate' => 'desc'],
            'pagination' => ['limit' => 20],
            'mentionFormat' => 'html',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "call.followup.list", b24.Params{
    	"filter": b24.Params{
    		"startDate": b24.Params{
    			"from": "2026-01-01T00:00:00Z",
    			"to":   "2026-01-31T23:59:59Z",
    		},
    	},
    	"select": []string{"callId", "startDate", "participants", "overview.topic", "overview.actionItems"},
    	"order": b24.Params{
    		"startDate": "desc",
    	},
    	"pagination": b24.Params{
    		"limit": 20,
    	},
    	"mentionFormat": "html",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("call.followup.list: %w", err)
    }

    var item struct {
    	HasMore bool `json:"hasMore"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.HasMore)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "callId": 12345,
                "startDate": "2026-01-15T10:00:00+00:00",
                "participants": [
                    { "userId": 7, "name": "Klaus Weber", "avatar": "https://...", "talkedSeconds": 600 },
                    { "userId": 42, "name": "Maria Schmidt", "talkedSeconds": 1200 }
                ],
                "overview": {
                    "topic": "Sprint planning",
                    "actionItems": [
                        { "actionItem": "Deploy MVP by Friday", "quote": "..." }
                    ]
                }
            }
        ],
        "hasMore": true,
        "afterCursor": { "startDate": "2026-01-12T14:30:00.000000+00:00", "id": 12330 }
    },
    "time": {
        "start": 1784017027,
        "finish": 1784017027.356922,
        "duration": 0.356921911239624,
        "processing": 0,
        "date_start": "2026-07-14T11:17:07+03:00",
        "date_finish": "2026-07-14T11:17:07+03:00",
        "operating_reset_at": 1784017627,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with response data ||
|| **items**
[`array`](../../data-types.md) | Array of Follow-up objects. The composition of fields depends on `select`.

If no matching Follow-ups are found, an empty array `[]` will be returned ||
|| **hasMore**
[`boolean`](../../data-types.md) | Presence indicator for the next page ||
|| **afterCursor**
[`object`](../../data-types.md) | Cursor to retrieve the next page.

If there is no next page, `null` is returned ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "invalid_date_range",
        "message": "Incorrect date range: both from and to are required"
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Insufficient permissions: required scope is missing | Check that the application or webhook has the scope `call` ||
|#

#### Filter Errors

Error Code: `invalid_date_range`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.startDate` | Incorrect date range | Pass `from` and `to` in ISO 8601 format. The value of `from` must be less than or equal to `to` ||
|#

#### Errors in the `select` Parameter

Error Code: `invalid_select_field`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select` | Pass a field from the list of available values `select` ||
|#

#### Sorting Errors

Error Code: `invalid_order`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Incorrect parameter `order` | Pass `{ "startDate": "asc" }` or `{ "startDate": "desc" }` ||
|#

#### Pagination Errors

Error Code: `invalid_pagination`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect parameter `pagination` | Pass a positive integer `limit` and the cursor from the previous response ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.participantId` | An invalid participant identifier type was passed | Pass `filter.participantId` as an integer ||
|| `mentionFormat` | A value not from the list of allowed formats was passed | Provide `bb`, `html`, or `none` ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./call-followup-get.md)
- [{#T}](./fields.md)
- [{#T}](./call-followup-field-list.md)
- [{#T}](./call-followup-field-get.md)
- [{#T}](./index.md)

<!-- Generated by skill-1-gen-docs from source: api-reference/telephony/doc.md, C:\Users\g.m.tagirova\original-bitrix\modules\call\lib\Controller\FollowUp.php -->
<!-- Generated: 2026-07-14 -->
<!-- Requires verification: no -->
