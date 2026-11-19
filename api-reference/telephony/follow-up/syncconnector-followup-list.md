# Get a List of Bitrix24 Sync Call Follow-ups syncconnector.followup.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`syncconnector`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

The method belongs to REST 3.0. Details regarding call specifics and the response format of the new API version are described in the [REST 3.0 Overview](../../rest-v3.md).

{% endnote %}

The `syncconnector.followup.list` method returns a list of Bitrix24 Sync call Follow-ups for the specified period.

A regular user receives Follow-ups of the calls they spoke in or whose chat they belong to. An administrator receives Follow-ups of all employees, or of a single employee when the `filter.participantId` parameter is passed.

The method returns only Follow-ups of Sync calls. To retrieve them together with Follow-ups of Bitrix24 calls, use [call.followup.list](./call-followup-list.md): it has the same parameters and response fields.

`callId` in the response is not a call identifier but the identifier of a detailed analysis page: the page opened by the "Detailed analysis" link in the chat. A Sync call can have several such pages, one for each call part. The `uuid` field links the parts of one call: add it to `select` to group them. In [call.followup.list](./call-followup-list.md), the same Follow-up is returned with `callId` equal to `10000000000` plus the page identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../data-types.md) | Selection criteria [(detailed description)](#filter) ||
|| **select**
[`array`](../../data-types.md) | List of fields and nested paths to be returned in the list items.

If the parameter is not passed or an empty array is passed, the method returns the fields `callId`, `callType`, `initiatorId`, `startDate`, `endDate`, `durationSeconds`.

A field listed in `select` is present in the response even if its value is `null`. The method always returns the `callId` field. For a full list of fields and nested paths, see the article [Call Follow-up Fields](./fields.md#select-paths).

AI fields are `transcription`, `overview`, `summary`, `insights`, `evaluation`. If `select` contains at least one of them or one of their nested paths, a single response contains no more than `20` items ||
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
[`integer`](../../data-types.md) | Employee identifier. For an administrator, the method returns this employee's Follow-ups, and without the parameter, the Follow-ups of all employees. For other users, the parameter does not change the result.

Pass the value as a number; otherwise the method returns a validation error ||
|| **chatId**
[`integer`](../../data-types.md) | Chat identifier. The method returns Follow-ups only for calls from this chat.

Pass the value as a number: the string `"123"` returns a validation error. If you pass `0` or a negative number, the method returns an empty list ||
|#

#### Parameter filter.startDate {#startdate}

#|
|| **Name**
`type` | **Description** ||
|| **from***
[`string`](../../data-types.md) | Period start in ISO 8601 format with time and time zone, for example `2026-01-01T00:00:00Z` or `2026-01-01T00:00:00+03:00` ||
|| **to***
[`string`](../../data-types.md) | Period end in the same format. The value must be greater than or equal to `from` ||
|#

### Parameter order {#order}

#|
|| **Name**
`type` | **Description** ||
|| **startDate**
[`string`](../../data-types.md) | Sort direction by start date.

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
[`integer`](../../data-types.md) | Maximum number of Follow-ups in a single response.

Default: `50`. Maximum: `200`. If `select` contains an AI field, the response contains no more than `20` items, even without `limit`. If a value greater than the maximum is passed, the method applies the maximum value ||
|| **afterCursor**
[`object`](../../data-types.md) | Next page cursor. Pass the `afterCursor` object from the previous response unchanged [(detailed description)](#aftercursor) ||
|#

The method does not accept other keys in `pagination`: `page` and `offset` return the `invalid_pagination` error.

#### Parameter pagination.afterCursor {#aftercursor}

To retrieve all pages:

1. Send the first request without `pagination.afterCursor`
2. If `hasMore` in the response equals `true`, copy the `afterCursor` object from the response into the `pagination.afterCursor` of the next request
3. Repeat requests until `hasMore` equals `false`, even if a response is empty or contains fewer items than `limit`

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

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/syncconnector.followup.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"startDate":{"from":"2026-11-01T00:00:00Z","to":"2026-11-30T23:59:59Z"}},"select":["callId","startDate","participants","overview.topic","overview.actionItems"],"order":{"startDate":"desc"},"pagination":{"limit":20},"mentionFormat":"html"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/syncconnector.followup.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"startDate":{"from":"2026-11-01T00:00:00Z","to":"2026-11-30T23:59:59Z"}},"select":["callId","startDate","participants","overview.topic","overview.actionItems"],"order":{"startDate":"desc"},"pagination":{"limit":20},"mentionFormat":"html","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/syncconnector.followup.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SyncFollowUpListResult = {
      items: Array<{
        callId: number
        startDate?: string
        participants?: unknown[]
        overview?: { topic?: string, actionItems?: unknown[] } | null
      }>
      hasMore: boolean
      afterCursor: { startDate: string, id: number } | null
    }

    try {
      const response = await $b24.actions.v3.call.make<SyncFollowUpListResult>({
        method: 'syncconnector.followup.list',
        params: {
          filter: {
            startDate: {
              from: '2026-11-01T00:00:00Z',
              to: '2026-11-30T23:59:59Z',
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
        console.info(result.items, result.hasMore, result.afterCursor)
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
      async function getSyncFollowUpList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'syncconnector.followup.list',
            params: {
              filter: {
                startDate: {
                  from: '2026-11-01T00:00:00Z',
                  to: '2026-11-30T23:59:59Z',
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

      document.addEventListener('DOMContentLoaded', getSyncFollowUpList)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made methods for [syncconnector.followup.*](./index.md#sync-methods), so call the method via `call_method` and add it to `api_v3_methods`, as shown in the example.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "syncconnector.followup.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": {
                    "startDate": {
                        "from": "2026-11-01T00:00:00Z",
                        "to": "2026-11-30T23:59:59Z",
                    },
                },
                "select": [
                    "callId",
                    "startDate",
                    "participants",
                    "overview.topic",
                    "overview.actionItems",
                ],
                "order": {
                    "startDate": "desc",
                },
                "pagination": {
                    "limit": 20,
                },
                "mentionFormat": "html",
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'syncconnector.followup.list',
                [
                    'filter' => [
                        'startDate' => [
                            'from' => '2026-11-01T00:00:00Z',
                            'to' => '2026-11-30T23:59:59Z',
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

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'syncconnector.followup.list',
        {
            filter: {
                startDate: {
                    from: '2026-11-01T00:00:00Z',
                    to: '2026-11-30T23:59:59Z'
                }
            },
            select: ['callId', 'startDate', 'participants', 'overview.topic', 'overview.actionItems'],
            order: { startDate: 'desc' },
            pagination: { limit: 20 },
            mentionFormat: 'html'
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'syncconnector.followup.list',
        [
            'filter' => [
                'startDate' => [
                    'from' => '2026-11-01T00:00:00Z',
                    'to' => '2026-11-30T23:59:59Z',
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
    res, err := client.Core().Call(ctx, "syncconnector.followup.list", b24.Params{
    	"filter": b24.Params{
    		"startDate": b24.Params{
    			"from": "2026-11-01T00:00:00Z",
    			"to":   "2026-11-30T23:59:59Z",
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
    	return fmt.Errorf("syncconnector.followup.list: %w", err)
    }

    var page struct {
    	Items       []json.RawMessage `json:"items"`
    	HasMore     bool              `json:"hasMore"`
    	AfterCursor *struct {
    		StartDate string `json:"startDate"`
    		ID        int    `json:"id"`
    	} `json:"afterCursor"`
    }
    if err := json.Unmarshal(res.Result, &page); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(len(page.Items), page.HasMore)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "callId": 128,
                "startDate": "2026-11-12T09:00:00+00:00",
                "participants": [
                    { "userId": 7, "talkedSeconds": 600, "name": "Klaus Weber", "avatar": "https://...", "workPosition": "Project Manager" },
                    { "userId": 42, "talkedSeconds": 1200, "name": "Maria Schmidt", "avatar": null, "workPosition": null }
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
        "afterCursor": { "startDate": "2026-11-12T09:00:00.000000+00:00", "id": 128 }
    },
    "time": {
        "start": 1794476996,
        "finish": 1794476996.873855,
        "duration": 0.8738551139831543,
        "processing": 0,
        "date_start": "2026-11-12T12:49:56+03:00",
        "date_finish": "2026-11-12T12:49:56+03:00",
        "operating_reset_at": 1794477540,
        "operating": 0
    }
}
```

If there are no Follow-ups for the period, the method returns `{"items":[],"hasMore":false,"afterCursor":null}`.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with response data ||
|| **items**
[`array`](../../data-types.md) | Array of Follow-up objects. The composition of fields depends on `select`. For field descriptions, see the article [Call Follow-up Fields](./fields.md#root-object); for differences in Sync fields, see the section [Field Differences in Bitrix24 Sync](./fields.md#sync).

If no matching Follow-ups are found, an empty array `[]` is returned ||
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
        "message": "Invalid date range: both from and to are required"
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Insufficient access permissions: required scope is missing | Check that the application or webhook has the `syncconnector` scope ||
|#

#### Filter Errors

Error Code: `invalid_date_range`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.startDate` | Invalid date range: both from and to are required | Pass `from` and `to` in ISO 8601 format with time and time zone. A date without time, for example `2026-01-01`, returns the same error ||
|| `filter.startDate` | Invalid date range: from must be <= to | The value of `from` must be less than or equal to `to` ||
|#

#### Errors in the `select` Parameter

Error Code: `invalid_select_field`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Field not allowed in select: `#FIELD#` | Pass a field from the [List of Available Values](./fields.md#select-paths) ||
|#

#### Sorting Errors

Error Code: `invalid_order`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Invalid order parameter | Pass `{ "startDate": "asc" }` or `{ "startDate": "desc" }` ||
|#

#### Pagination Errors

Error Code: `invalid_pagination`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Invalid pagination parameter | Pass only `limit` and the `afterCursor` from the previous response. `limit` must be a positive integer ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.participantId` | This request requires data type `int` in field `filter.participantId` | Pass `filter.participantId` as an integer ||
|| `filter.chatId` | This request requires data type `int` in field `filter.chatId` | Pass `filter.chatId` as an integer ||
|| `mentionFormat` | Field value is not one of the possible values | Pass `bb`, `html`, or `none` ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./syncconnector-followup-get.md)
- [{#T}](./syncconnector-followup-field-list.md)
- [{#T}](./syncconnector-followup-field-get.md)
- [{#T}](./fields.md)
- [{#T}](./index.md)
