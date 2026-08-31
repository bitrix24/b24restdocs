# Get Sessions imopenlines.v2.Session.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to Open Channels reports

The method `imopenlines.v2.Session.list` retrieves Open Channel sessions with filters and pagination.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **configId**
[`integer`](../../data-types.md) | Open channel ID.

You can obtain the ID using [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md) ||
|| **configIdList**
[`integer[]`](../../data-types.md) | List of open channel IDs.

You can obtain the IDs using [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md).

If both `configIdList` and `configId` are passed, `configIdList` is used.

Maximum: 1000 items ||
|| **operatorId**
[`integer`](../../data-types.md) | Operator ID.

You can obtain the ID using [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md) ||
|| **operatorIdList**
[`integer[]`](../../data-types.md) | List of operator IDs.

You can obtain the IDs using [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md).

If both `operatorIdList` and `operatorId` are passed, `operatorIdList` is used.

Maximum: 1000 items ||
|| **source**
[`string`](../../data-types.md) | Channel code.

You can obtain the code using [imconnector.list](../imconnector/imconnector-list.md) ||
|| **sourceList**
[`string[]`](../../data-types.md) | List of channel codes.

You can obtain the codes using [imconnector.list](../imconnector/imconnector-list.md).

If both `sourceList` and `source` are passed, `sourceList` is used.

Maximum: 1000 items ||
|| **status**
[`string`](../../data-types.md) | Session status.

Possible values:

- `new` — session is in the queue or missed
- `answered` — operator is handling the dialog
- `closed` — session is closed
- `spam` — session is marked as spam
- `paused` — session is paused ||
|| **closeReason**
[`string`](../../data-types.md) | Close reason.

Cannot be passed together with `status`.

Possible values:

- `operator` — session was closed by an operator
- `auto` — session was closed automatically by timeout
- `spam` — session was closed as spam
- `client` — session was closed because of customer inactivity
- `replyLimit` — session was closed after the channel response window expired ||
|| **dateCreateFrom**
[`string`](../../data-types.md) | Start of the creation period in ISO 8601 format ||
|| **dateCreateTo**
[`string`](../../data-types.md) | End of the creation period in ISO 8601 format.

Maximum period: 366 days ||
|| **dateCloseFrom**
[`string`](../../data-types.md) | Start of the close period in ISO 8601 format ||
|| **dateCloseTo**
[`string`](../../data-types.md) | End of the close period in ISO 8601 format.

Maximum period: 366 days ||
|| **vote**
[`string`](../../data-types.md) | Customer rating.

Possible values:

- `like` — customer left a like
- `dislike` — customer left a dislike
- `none` — no rating
- `any` — any customer rating exists ||
|| **hasVoteHead**
[`boolean`](../../data-types.md) | Filter by supervisor rating availability.

Possible values:

- `true`, `Y`, `1` — supervisor rating exists
- `false`, `N`, `0` — supervisor rating does not exist ||
|| **kpiFirstAnswer**
[`boolean`](../../data-types.md) | Filter by first response KPI performance.

Requires a full `dateCreateFrom` and `dateCreateTo` or `dateCloseFrom` and `dateCloseTo` period.

Possible values:

- `true`, `Y`, `1` — first response KPI is met
- `false`, `N`, `0` — first response KPI is not met ||
|| **hasCrm**
[`boolean`](../../data-types.md) | Filter by an available CRM link.

Possible values:

- `true`, `Y`, `1` — an available CRM link exists
- `false`, `N`, `0` — no available CRM link exists ||
|| **waitAnswerFrom**
[`integer`](../../data-types.md) | Minimum time to first response, seconds ||
|| **waitAnswerTo**
[`integer`](../../data-types.md) | Maximum time to first response, seconds ||
|| **waitCloseFrom**
[`integer`](../../data-types.md) | Minimum time to close, seconds ||
|| **waitCloseTo**
[`integer`](../../data-types.md) | Maximum time to close, seconds ||
|| **order**
[`string`](../../data-types.md) | Sort field.

Possible values:

- `dateCreate` — session creation date
- `dateClose` — session close date
- `waitAnswer` — time to first response
- `waitClose` — time to close

Default: `dateCreate` ||
|| **orderDirection**
[`string`](../../data-types.md) | Sort direction.

Possible values:

- `asc` — ascending
- `desc` — descending

Default: `desc` ||
|| **offset**
[`integer`](../../data-types.md) | Pagination offset.

Default: `0` ||
|| **limit**
[`integer`](../../data-types.md) | Page size.

Possible values: from `1` to `200`.

Default: `50` ||
|#

{% note info "" %}

Filters are combined with logical AND.

Sorting by `dateClose`, `waitAnswer`, and `waitClose` is applied only when a full creation or close period is specified; otherwise, the method sorts by `dateCreate`.

The `voteHead` and `commentHead` fields in `session` items are returned as `null` if the plan or user permissions do not allow viewing supervisor ratings. The `hasVoteHead` filter applies only to lines where the user has permission to view supervisor ratings.

The `crmEntityType` and `crmEntityId` fields in `session` items are returned as `null` if the user does not have read permission for the linked CRM object. The `hasCrm` filter considers only CRM links available to the user.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "configId": 3,
        "status": "closed",
        "dateCreateFrom": "2026-06-01T00:00:00+02:00",
        "dateCreateTo": "2026-06-30T23:59:59+02:00",
        "limit": 50
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.v2.Session.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "configId": 3,
        "status": "closed",
        "dateCreateFrom": "2026-06-01T00:00:00+02:00",
        "dateCreateTo": "2026-06-30T23:59:59+02:00",
        "limit": 50,
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imopenlines.v2.Session.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SessionListResult = {
      sessions: Array<{ id: number, configId: number, status: string }>
      hasNextPage: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<SessionListResult>({
        method: 'imopenlines.v2.Session.list',
        params: {
          configId: 3,
          status: 'closed',
          dateCreateFrom: '2026-06-01T00:00:00+02:00',
          dateCreateTo: '2026-06-30T23:59:59+02:00',
          limit: 50,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info(response.getData()!.result.sessions)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getOpenlinesSessions() {
        try {
          const $b24 = await B24Js.initializeB24Frame()
          const response = await $b24.actions.v2.call.make({
            method: 'imopenlines.v2.Session.list',
            params: {
              configId: 3,
              status: 'closed',
              dateCreateFrom: '2026-06-01T00:00:00+02:00',
              dateCreateTo: '2026-06-30T23:59:59+02:00',
              limit: 50,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          console.info(response.getData().result.sessions)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getOpenlinesSessions)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.call(
            "imopenlines.v2.Session.list",
            {
                "configId": 3,
                "status": "closed",
                "dateCreateFrom": "2026-06-01T00:00:00+02:00",
                "dateCreateTo": "2026-06-30T23:59:59+02:00",
                "limit": 50,
            },
        ).response
        print(bitrix_response.result)
    except BitrixAPIError as error:
        print(f"error: {error.error}", f"error_description: {error.error_description}", sep="\n")
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.v2.Session.list',
                [
                    'configId' => 3,
                    'status' => 'closed',
                    'dateCreateFrom' => '2026-06-01T00:00:00+02:00',
                    'dateCreateTo' => '2026-06-30T23:59:59+02:00',
                    'limit' => 50,
                ]
            );

        print_r($response->getResponseData()->getResult());
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.v2.Session.list',
        {
            configId: 3,
            status: 'closed',
            dateCreateFrom: '2026-06-01T00:00:00+02:00',
            dateCreateTo: '2026-06-30T23:59:59+02:00',
            limit: 50,
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imopenlines.v2.Session.list',
        [
            'configId' => 3,
            'status' => 'closed',
            'dateCreateFrom' => '2026-06-01T00:00:00+02:00',
            'dateCreateTo' => '2026-06-30T23:59:59+02:00',
            'limit' => 50,
        ]
    );

    print_r($result);
    ```

- Go

    ```go
    res, err := client.Core().Call(ctx, "imopenlines.v2.Session.list", b24.Params{
    	"configId":        3,
    	"status":          "closed",
    	"dateCreateFrom":  "2026-06-01T00:00:00+02:00",
    	"dateCreateTo":    "2026-06-30T23:59:59+02:00",
    	"limit":           50,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("imopenlines.v2.Session.list: %w", err)
    }

    fmt.Println(string(res.Result))
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "sessions": [
            {
                "id": 1024,
                "configId": 3,
                "source": "livechat",
                "operatorId": 42,
                "userId": 501,
                "userCode": "site_visitor_88a1",
                "chatId": 2048,
                "dateCreate": "2026-06-15T14:30:00+02:00",
                "dateClose": "2026-06-15T14:52:10+02:00",
                "dateFirstAnswer": "2026-06-15T14:31:05+02:00",
                "dateOperatorAnswer": "2026-06-15T14:31:05+02:00",
                "status": "closed",
                "closeReason": "operator",
                "vote": "like",
                "voteHead": 5,
                "commentHead": "Good work",
                "crmEntityType": "deal",
                "crmEntityId": 771,
                "queueTransfers": 1,
                "waitAnswer": 65,
                "waitClose": 1330,
                "kpiFirstAnswer": true,
                "messageCount": 14
            }
        ],
        "hasNextPage": false
    },
    "time": {
        "start": 1782810000,
        "finish": 1782810000.4,
        "duration": 0.4,
        "processing": 0,
        "date_start": "2026-06-30T10:00:00+02:00",
        "date_finish": "2026-06-30T10:00:00+02:00",
        "operating_reset_at": 1782810600,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root response object ||
|| **result.sessions**
[`session[]`](./data-types.md#session) | List of sessions.

See all fields of the [`session`](./data-types.md#session) type in [Open Channels Statistics Data Types](./data-types.md#session) ||
|| **result.hasNextPage**
[`boolean`](../../data-types.md) | Indicates whether there is a next page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "PERIOD_TOO_LARGE",
    "error_description": "The requested period exceeds the maximum of 1 year"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `TARIFF_RESTRICTION` | Statistics reports are not available on the current tariff plan | The plan does not allow using Open Channels reports ||
|| `400` | `INVALID_FILTER` | Invalid filter value | An invalid filter, date, date order, combination of `status` and `closeReason`, list with more than 1000 items, or `kpiFirstAnswer` without a full period was passed ||
|| `400` | `PERIOD_TOO_LARGE` | The requested period exceeds the maximum of 1 year | The creation or close period is longer than 366 days ||
|| `400` | `OFFSET_TOO_LARGE` | Offset is too large | `offset` is greater than `10000` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [imopenlines.v2.Session.Stat.get](./imopenlines-v2-session-stat-get.md)
- [imopenlines.v2.Session.Rating.list](./imopenlines-v2-session-rating-list.md)
- [imopenlines.v2.Session.Transfer.list](./imopenlines-v2-session-transfer-list.md)
- [Open Channels Statistics Data Types](./data-types.md)
