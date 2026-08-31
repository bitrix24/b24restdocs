# Get Aggregate Statistics imopenlines.v2.Stat.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to Open Channels reports

The method `imopenlines.v2.Stat.get` retrieves aggregate Open Channels statistics for a period.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **dateFrom***
[`string`](../../data-types.md) | Start of the period in ISO 8601 format ||
|| **dateTo***
[`string`](../../data-types.md) | End of the period in ISO 8601 format.

Maximum period: 366 days ||
|| **configId**
[`integer`](../../data-types.md) | Open channel ID.

You can obtain the ID using [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md) ||
|| **configIdList**
[`integer[]`](../../data-types.md) | List of open channel IDs.

You can obtain the IDs using [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md).

If both `configIdList` and `configId` are passed, `configIdList` is used.

Maximum: 1000 items ||
|| **source**
[`string`](../../data-types.md) | Channel code.

You can obtain the code using [imconnector.list](../imconnector/imconnector-list.md) ||
|| **sourceList**
[`string[]`](../../data-types.md) | List of channel codes.

You can obtain the codes using [imconnector.list](../imconnector/imconnector-list.md).

If both `sourceList` and `source` are passed, `sourceList` is used.

Maximum: 1000 items ||
|| **operatorId**
[`integer`](../../data-types.md) | Operator ID.

You can obtain the ID using [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md) ||
|| **operatorIdList**
[`integer[]`](../../data-types.md) | List of operator IDs.

You can obtain the IDs using [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md).

If both `operatorIdList` and `operatorId` are passed, `operatorIdList` is used.

Maximum: 1000 items ||
|#

{% note info "" %}

If you do not pass line, channel, or operator filters, the method calculates statistics for the lines available to the user.

If there is no data for the period, numeric metrics are returned as `0`, not `null`.

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
        "dateFrom": "2026-06-01T00:00:00+02:00",
        "dateTo": "2026-06-30T23:59:59+02:00",
        "configId": 3
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.v2.Stat.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "dateFrom": "2026-06-01T00:00:00+02:00",
        "dateTo": "2026-06-30T23:59:59+02:00",
        "configId": 3,
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imopenlines.v2.Stat.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StatResult = {
      totalSessions: number
      closedSessions: number
      spamSessions: number
      avgWaitAnswer: number
      avgSessionDuration: number
      likeCount: number
      dislikeCount: number
      votedSessions: number
      positiveRate: number
      kpiFirstAnswerOk: number
      kpiFirstAnswerFail: number
      sessionsBySource: Array<{ source: string, count: number }>
      sessionsByHour: number[]
      sessionsByOperator: Array<{ operatorId: number, count: number, avgWaitAnswer: number, positiveRate: number }>
    }

    try {
      const response = await $b24.actions.v2.call.make<StatResult>({
        method: 'imopenlines.v2.Stat.get',
        params: {
          dateFrom: '2026-06-01T00:00:00+02:00',
          dateTo: '2026-06-30T23:59:59+02:00',
          configId: 3,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info(response.getData()!.result)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getOpenlinesStat() {
        try {
          const $b24 = await B24Js.initializeB24Frame()
          const response = await $b24.actions.v2.call.make({
            method: 'imopenlines.v2.Stat.get',
            params: {
              dateFrom: '2026-06-01T00:00:00+02:00',
              dateTo: '2026-06-30T23:59:59+02:00',
              configId: 3,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          console.info(response.getData().result)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getOpenlinesStat)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.call(
            "imopenlines.v2.Stat.get",
            {
                "dateFrom": "2026-06-01T00:00:00+02:00",
                "dateTo": "2026-06-30T23:59:59+02:00",
                "configId": 3,
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
                'imopenlines.v2.Stat.get',
                [
                    'dateFrom' => '2026-06-01T00:00:00+02:00',
                    'dateTo' => '2026-06-30T23:59:59+02:00',
                    'configId' => 3,
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
        'imopenlines.v2.Stat.get',
        {
            dateFrom: '2026-06-01T00:00:00+02:00',
            dateTo: '2026-06-30T23:59:59+02:00',
            configId: 3,
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
        'imopenlines.v2.Stat.get',
        [
            'dateFrom' => '2026-06-01T00:00:00+02:00',
            'dateTo' => '2026-06-30T23:59:59+02:00',
            'configId' => 3,
        ]
    );

    print_r($result);
    ```

- Go

    ```go
    res, err := client.Core().Call(ctx, "imopenlines.v2.Stat.get", b24.Params{
    	"dateFrom": "2026-06-01T00:00:00+02:00",
    	"dateTo":   "2026-06-30T23:59:59+02:00",
    	"configId": 3,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("imopenlines.v2.Stat.get: %w", err)
    }

    fmt.Println(string(res.Result))
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "totalSessions": 340,
        "closedSessions": 318,
        "spamSessions": 4,
        "avgWaitAnswer": 42.7,
        "avgSessionDuration": 612.3,
        "likeCount": 210,
        "dislikeCount": 15,
        "votedSessions": 225,
        "positiveRate": 0.9333,
        "kpiFirstAnswerOk": 300,
        "kpiFirstAnswerFail": 18,
        "sessionsBySource": [
            {
                "source": "livechat",
                "count": 200
            }
        ],
        "sessionsByHour": [0, 0, 0, 0, 0, 0, 2, 10, 25, 40, 38, 30, 28, 22, 20, 25, 30, 20, 15, 10, 8, 5, 3, 1],
        "sessionsByOperator": [
            {
                "operatorId": 42,
                "count": 120,
                "avgWaitAnswer": 38.1,
                "positiveRate": 0.95
            }
        ]
    },
    "time": {
        "start": 1782810000,
        "finish": 1782810000.3,
        "duration": 0.3,
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
[`statResult`](./data-types.md#stat-result) | Aggregate statistics for the period.

See all fields of the [`statResult`](./data-types.md#stat-result) type in [Open Channels Statistics Data Types](./data-types.md#stat-result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "PERIOD_REQUIRED",
    "error_description": "dateFrom and dateTo are required"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `TARIFF_RESTRICTION` | Statistics reports are not available on the current tariff plan | The plan does not allow using Open Channels reports ||
|| `400` | `PERIOD_REQUIRED` | dateFrom and dateTo are required | The `dateFrom` or `dateTo` parameter is not provided ||
|| `400` | `INVALID_FILTER` | Invalid filter value | An invalid filter, date, or list with more than 1000 items was passed ||
|| `400` | `PERIOD_TOO_LARGE` | The requested period exceeds the maximum of 1 year | The period is longer than 366 days ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [imopenlines.v2.Operator.list](./imopenlines-v2-operator-list.md)
- [imopenlines.v2.Session.list](./imopenlines-v2-session-list.md)
- [Open Channels Statistics Data Types](./data-types.md)
