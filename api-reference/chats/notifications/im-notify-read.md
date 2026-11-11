# Read or Mark Notification as Unread im.notify.read

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.read` marks the current user's notifications as read or unread.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | The identifier of the notification. You can retrieve the identifier using the [im.notify.get](./im-notify-get.md) method.

If the parameter is not provided or its value is less than `0`, the method changes nothing and returns `true`.

The value `0` with `ACTION` = `Y` marks all of the user's notifications as read ||
|| **ACTION**
[`string`](../../data-types.md) | Action on the notifications:
- `Y` — mark as read
- `N` — mark as unread

Any value other than the string `Y` is treated as `N`, including the boolean `true`.

Default value is `Y` ||
|| **ONLY_CURRENT**
[`string`](../../data-types.md) | Which notifications to change:
- `Y` — only the notification with the specified `ID`
- any other value — the notification with the specified `ID` and all notifications with a greater identifier

If the parameter is not provided, the method behaves as with any value other than `Y` ||
|#

{% note warning "" %}

By default, the method changes the status of the notification with the specified `ID` and of all notifications with a greater identifier. To change only one notification, pass `ONLY_CURRENT` with the value `Y`

{% endnote %}

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":101,"ACTION":"Y","ONLY_CURRENT":"Y"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify.read
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":101,"ACTION":"Y","ONLY_CURRENT":"Y","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.notify.read
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'im.notify.read',
        params: {
          ID: 101,
          ACTION: 'Y',
          ONLY_CURRENT: 'Y',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Notification marked as read:', result)
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
      async function markNotificationRead() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.notify.read',
            params: {
              ID: 101,
              ACTION: 'Y',
              ONLY_CURRENT: 'Y',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Notification marked as read:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', markNotificationRead)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.notify.read(
            bitrix_id=101,
            action=True,
            only_current=True,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.notify.read',
            [
                'ID' => 101,
                'ACTION' => 'Y',
                'ONLY_CURRENT' => 'Y',
            ]
        );

        $result = $response->getResponseData()->getResult();

        print_r($result);
    } catch (Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.notify.read',
        {
            ID: 101,
            ACTION: 'Y',
            ONLY_CURRENT: 'Y',
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.notify.read',
        [
            'ID' => 101,
            'ACTION' => 'Y',
            'ONLY_CURRENT' => 'Y',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        var_dump($result['result']);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.notify.read", b24.Params{
    	"ID":           101,
    	"ACTION":       "Y",
    	"ONLY_CURRENT": "Y",
    })
    if err != nil {
    	return fmt.Errorf("im.notify.read: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1794317405,
        "finish": 1794317406.001192,
        "duration": 1.0011920928955078,
        "processing": 1,
        "date_start": "2026-11-10T16:30:05+01:00",
        "date_finish": "2026-11-10T16:30:06+01:00",
        "operating_reset_at": 1794318000,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Always `true`. The method returns `true` even if the notification with the specified `ID` is not found or its status has not changed. To check the status, use the `notify_read` field in the response of the [im.notify.get](./im-notify-get.md) method ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **401**

```json
{
    "error": "INVALID_CREDENTIALS",
    "error_description": "Invalid request credentials"
}
```

The method has no error codes of its own — only system REST API errors are possible.

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-notify.md)
- [{#T}](./im-notify-personal-add.md)
- [{#T}](./im-notify-system-add.md)
- [{#T}](./im-notify-get.md)
- [{#T}](./im-notify-schema-get.md)
- [{#T}](./im-notify-read-list.md)
- [{#T}](./im-notify-read-all.md)
- [{#T}](./im-notify-answer.md)
- [{#T}](./im-notify-confirm.md)
- [{#T}](./im-notify-delete.md)
- [{#T}](./im-notify-history-search.md)