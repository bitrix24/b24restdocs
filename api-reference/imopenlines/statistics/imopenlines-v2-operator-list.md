# Get Operators with Load imopenlines.v2.Operator.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to Open Channels reports

The method `imopenlines.v2.Operator.list` retrieves operators with their current status and load.

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
|| **userId**
[`integer`](../../data-types.md) | Operator ID.

You can obtain the ID using [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md) ||
|| **userIdList**
[`integer[]`](../../data-types.md) | List of operator IDs.

You can obtain the IDs using [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md).

If both `userIdList` and `userId` are passed, `userIdList` is used.

Maximum: 1000 items ||
|| **status**
[`string`](../../data-types.md) | Operator status.

Possible values:

- `online` — operator is online and not paused
- `offline` — operator is offline
- `pause` — operator has paused themselves ||
|| **hasFreeSlots**
[`boolean`](../../data-types.md) | Filter by available free slots.

Possible values:

- `true`, `Y`, `1` — free slots are available
- `false`, `N`, `0` — no free slots are available ||
|| **offset**
[`integer`](../../data-types.md) | Pagination offset.

Default: `0` ||
|| **limit**
[`integer`](../../data-types.md) | Page size.

Possible values: from `1` to `200`.

Default: `50` ||
|#

{% note info "" %}

Status and active session count data may be updated at different times. For a real-time load widget, request the method no more than once every 30 seconds.

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
        "status": "online",
        "limit": 50
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.v2.Operator.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "configId": 3,
        "status": "online",
        "limit": 50,
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imopenlines.v2.Operator.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type OperatorListResult = {
      operators: Array<{
        userId: number
        configId: number
        status: string
        activeSessions: number
        maxChat: number
        freeSlots: number
        lastActivityDate: string | null
      }>
      hasNextPage: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<OperatorListResult>({
        method: 'imopenlines.v2.Operator.list',
        params: {
          configId: 3,
          status: 'online',
          limit: 50,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info(response.getData()!.result.operators)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getOpenlinesOperators() {
        try {
          const $b24 = await B24Js.initializeB24Frame()
          const response = await $b24.actions.v2.call.make({
            method: 'imopenlines.v2.Operator.list',
            params: {
              configId: 3,
              status: 'online',
              limit: 50,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          console.info(response.getData().result.operators)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getOpenlinesOperators)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.call(
            "imopenlines.v2.Operator.list",
            {
                "configId": 3,
                "status": "online",
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
                'imopenlines.v2.Operator.list',
                [
                    'configId' => 3,
                    'status' => 'online',
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
        'imopenlines.v2.Operator.list',
        {
            configId: 3,
            status: 'online',
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
        'imopenlines.v2.Operator.list',
        [
            'configId' => 3,
            'status' => 'online',
            'limit' => 50,
        ]
    );

    print_r($result);
    ```

- Go

    ```go
    res, err := client.Core().Call(ctx, "imopenlines.v2.Operator.list", b24.Params{
    	"configId": 3,
    	"status":   "online",
    	"limit":    50,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("imopenlines.v2.Operator.list: %w", err)
    }

    fmt.Println(string(res.Result))
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "operators": [
            {
                "userId": 42,
                "configId": 3,
                "status": "online",
                "activeSessions": 2,
                "maxChat": 5,
                "freeSlots": 3,
                "lastActivityDate": "2026-06-15T15:01:00+02:00"
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
|| **result.operators**
[`operatorLoad[]`](./data-types.md#operator-load) | List of operators.

See all fields of the [`operatorLoad`](./data-types.md#operator-load) type in [Open Channels Statistics Data Types](./data-types.md#operator-load) ||
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
    "error_description": "Invalid status filter value"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `TARIFF_RESTRICTION` | Statistics reports are not available on the current tariff plan | The plan does not allow using Open Channels reports ||
|| `400` | `INVALID_FILTER` | Invalid status filter value | An unknown `status` or invalid list of values was passed ||
|| `400` | `OFFSET_TOO_LARGE` | Offset is too large | `offset` is greater than `10000` when filtering by `status` or `hasFreeSlots` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [imopenlines.v2.Stat.get](./imopenlines-v2-stat-get.md)
- [imopenlines.v2.Session.list](./imopenlines-v2-session-list.md)
- [Open Channels Statistics Data Types](./data-types.md)
