# Get Ratings imopenlines.v2.Session.Rating.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to Open Channels reports

The method `imopenlines.v2.Session.Rating.list` retrieves sessions rated by customers.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **dateVoteFrom***
[`string`](../../data-types.md) | Start of the customer rating period in ISO 8601 format ||
|| **dateVoteTo***
[`string`](../../data-types.md) | End of the customer rating period in ISO 8601 format.

Maximum period: 366 days ||
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
|| **vote**
[`string`](../../data-types.md) | Customer rating.

Possible values:

- `like` — customer left a like
- `dislike` — customer left a dislike ||
|| **hasVoteHead**
[`boolean`](../../data-types.md) | Filter by supervisor rating availability.

Possible values:

- `true`, `Y`, `1` — supervisor rating exists
- `false`, `N`, `0` — supervisor rating does not exist ||
|| **offset**
[`integer`](../../data-types.md) | Pagination offset.

Default: `0` ||
|| **limit**
[`integer`](../../data-types.md) | Page size.

Possible values: from `1` to `200`.

Default: `50` ||
|#

{% note info "" %}

The method returns only sessions with a customer rating.

Sessions with `vote: none` are not included in the response.

The `voteHead` and `commentHead` fields in `rating` items are returned as `null` if the plan or user permissions do not allow viewing supervisor ratings.

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
        "vote": "like",
        "dateVoteFrom": "2026-06-01T00:00:00+02:00",
        "dateVoteTo": "2026-06-30T23:59:59+02:00"
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.v2.Session.Rating.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "configId": 3,
        "vote": "like",
        "dateVoteFrom": "2026-06-01T00:00:00+02:00",
        "dateVoteTo": "2026-06-30T23:59:59+02:00",
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imopenlines.v2.Session.Rating.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make({
        method: 'imopenlines.v2.Session.Rating.list',
        params: {
          configId: 3,
          vote: 'like',
          dateVoteFrom: '2026-06-01T00:00:00+02:00',
          dateVoteTo: '2026-06-30T23:59:59+02:00',
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info(response.getData()!.result.ratings)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getOpenlinesRatings() {
        try {
          const $b24 = await B24Js.initializeB24Frame()
          const response = await $b24.actions.v2.call.make({
            method: 'imopenlines.v2.Session.Rating.list',
            params: {
              configId: 3,
              vote: 'like',
              dateVoteFrom: '2026-06-01T00:00:00+02:00',
              dateVoteTo: '2026-06-30T23:59:59+02:00',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          console.info(response.getData().result.ratings)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getOpenlinesRatings)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.call(
            "imopenlines.v2.Session.Rating.list",
            {
                "configId": 3,
                "vote": "like",
                "dateVoteFrom": "2026-06-01T00:00:00+02:00",
                "dateVoteTo": "2026-06-30T23:59:59+02:00",
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
                'imopenlines.v2.Session.Rating.list',
                [
                    'configId' => 3,
                    'vote' => 'like',
                    'dateVoteFrom' => '2026-06-01T00:00:00+02:00',
                    'dateVoteTo' => '2026-06-30T23:59:59+02:00',
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
        'imopenlines.v2.Session.Rating.list',
        {
            configId: 3,
            vote: 'like',
            dateVoteFrom: '2026-06-01T00:00:00+02:00',
            dateVoteTo: '2026-06-30T23:59:59+02:00',
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
        'imopenlines.v2.Session.Rating.list',
        [
            'configId' => 3,
            'vote' => 'like',
            'dateVoteFrom' => '2026-06-01T00:00:00+02:00',
            'dateVoteTo' => '2026-06-30T23:59:59+02:00',
        ]
    );

    print_r($result);
    ```

- Go

    ```go
    res, err := client.Core().Call(ctx, "imopenlines.v2.Session.Rating.list", b24.Params{
    	"configId":     3,
    	"vote":         "like",
    	"dateVoteFrom": "2026-06-01T00:00:00+02:00",
    	"dateVoteTo":   "2026-06-30T23:59:59+02:00",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("imopenlines.v2.Session.Rating.list: %w", err)
    }

    fmt.Println(string(res.Result))
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "ratings": [
            {
                "sessionId": 1024,
                "configId": 3,
                "operatorId": 42,
                "source": "livechat",
                "vote": "like",
                "voteHead": 5,
                "commentHead": "Good work",
                "dateVote": "2026-06-15T14:53:00+02:00",
                "dateSessionClose": "2026-06-15T14:52:10+02:00"
            }
        ],
        "hasNextPage": false
    },
    "time": {
        "start": 1782810000,
        "finish": 1782810000.2,
        "duration": 0.2,
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
|| **result.ratings**
[`rating[]`](./data-types.md#rating) | List of sessions with customer ratings.

See all fields of the [`rating`](./data-types.md#rating) type in [Open Channels Statistics Data Types](./data-types.md#rating) ||
|| **result.hasNextPage**
[`boolean`](../../data-types.md) | Indicates whether there is a next page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "INVALID_FILTER",
    "error_description": "Invalid vote filter value"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `TARIFF_RESTRICTION` | Statistics reports are not available on the current tariff plan | The plan does not allow using Open Channels reports ||
|| `400` | `PERIOD_REQUIRED` | dateVoteFrom and dateVoteTo are required | The `dateVoteFrom` or `dateVoteTo` parameter is not provided ||
|| `400` | `INVALID_FILTER` | Invalid filter value | An invalid filter, date, or list with more than 1000 items was passed ||
|| `400` | `PERIOD_TOO_LARGE` | The requested period exceeds the maximum of 1 year | The period is longer than 366 days ||
|| `400` | `OFFSET_TOO_LARGE` | Offset is too large | `offset` is greater than `10000` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [imopenlines.v2.Stat.get](./imopenlines-v2-stat-get.md)
- [imopenlines.v2.Session.list](./imopenlines-v2-session-list.md)
- [Open Channels Statistics Data Types](./data-types.md)
