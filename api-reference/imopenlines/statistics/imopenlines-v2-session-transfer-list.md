# Get Transfer History imopenlines.v2.Session.Transfer.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to Open Channels reports

The method `imopenlines.v2.Session.Transfer.list` retrieves transfer history for Open Channel sessions.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **sessionId***
[`integer[]`](../../data-types.md) | Array of session IDs.

You can obtain the IDs using [imopenlines.v2.Session.list](./imopenlines-v2-session-list.md).

Maximum: `50` unique IDs ||
|#

{% note info "" %}

Duplicates in `sessionId` are removed.

If a session does not exist or is not available to the user, there will be no records for it in the `transfers` array.

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
        "sessionId": [1024, 1025]
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.v2.Session.Transfer.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "sessionId": [1024, 1025],
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imopenlines.v2.Session.Transfer.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make({
        method: 'imopenlines.v2.Session.Transfer.list',
        params: {
          sessionId: [1024, 1025],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info(response.getData()!.result.transfers)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getOpenlinesTransfers() {
        try {
          const $b24 = await B24Js.initializeB24Frame()
          const response = await $b24.actions.v2.call.make({
            method: 'imopenlines.v2.Session.Transfer.list',
            params: {
              sessionId: [1024, 1025],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          console.info(response.getData().result.transfers)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getOpenlinesTransfers)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.call(
            "imopenlines.v2.Session.Transfer.list",
            {
                "sessionId": [
                    1024,
                    1025,
                ],
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
                'imopenlines.v2.Session.Transfer.list',
                [
                    'sessionId' => [1024, 1025],
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
        'imopenlines.v2.Session.Transfer.list',
        {
            sessionId: [1024, 1025],
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
        'imopenlines.v2.Session.Transfer.list',
        [
            'sessionId' => [1024, 1025],
        ]
    );

    print_r($result);
    ```

- Go

    ```go
    res, err := client.Core().Call(ctx, "imopenlines.v2.Session.Transfer.list", b24.Params{
    	"sessionId": []int{1024, 1025},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("imopenlines.v2.Session.Transfer.list: %w", err)
    }

    fmt.Println(string(res.Result))
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "transfers": [
            {
                "sessionId": 1024,
                "date": "2026-06-15T14:30:05+02:00",
                "fromOperatorId": null,
                "toOperatorId": 42,
                "fromConfigId": 3,
                "toConfigId": null,
                "reason": "auto",
                "mode": "AUTO",
                "type": "USER",
                "initiatorId": null
            },
            {
                "sessionId": 1024,
                "date": "2026-06-15T14:40:00+02:00",
                "fromOperatorId": 42,
                "toOperatorId": 51,
                "fromConfigId": 3,
                "toConfigId": null,
                "reason": "manual",
                "mode": "MANUAL",
                "type": "USER",
                "initiatorId": 42
            }
        ]
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
|| **result.transfers**
[`transfer[]`](./data-types.md#transfer) | Transfer history for the requested sessions.

See all fields of the [`transfer`](./data-types.md#transfer) type in [Open Channels Statistics Data Types](./data-types.md#transfer) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "BATCH_LIMIT_EXCEEDED",
    "error_description": "sessionId batch must not exceed 50 items"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `TARIFF_RESTRICTION` | Statistics reports are not available on the current tariff plan | The plan does not allow using Open Channels reports ||
|| `400` | `100` | Could not find value for parameter {sessionId} | The required `sessionId` parameter is not provided ||
|| `400` | `100` | Invalid value {value} to match with parameter {sessionId}. Should be value of type array. | `sessionId` was not passed as an array ||
|| `400` | `BATCH_LIMIT_EXCEEDED` | sessionId batch must not exceed 50 items | More than 50 unique session IDs were passed ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [imopenlines.v2.Session.list](./imopenlines-v2-session-list.md)
- [imopenlines.v2.Session.Stat.get](./imopenlines-v2-session-stat-get.md)
- [Open Channels Statistics Data Types](./data-types.md)
