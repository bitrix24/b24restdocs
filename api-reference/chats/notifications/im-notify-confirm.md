# Interacting with Notification Buttons im.notify.confirm

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.confirm` clicks a button in a notification of the current user and deletes that notification.

{% note info "" %}

Notifications with buttons are created by Bitrix24 modules, for example meeting invitations in the calendar. The [im.notify.personal.add](./im-notify-personal-add.md) and [im.notify.system.add](./im-notify-system-add.md) methods do not create such notifications

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **NOTIFY_ID***
[`integer`](../../data-types.md) | Identifier of the notification with buttons. You can retrieve the identifier using the [im.notify.get](./im-notify-get.md) method; for such notifications, the `notify_type` field equals `1`.

Instead of `NOTIFY_ID`, you can pass the `ID` parameter. If both are passed, the method uses `ID` ||
|| **NOTIFY_VALUE***
[`string`](../../data-types.md) | Value of the selected button. The values are set by the module that sent the notification.

For example, a notification with the "Accept" and "Decline" buttons has the values `Y` and `N`.

The method treats the string `0` as an empty value and returns the `NOTIFY_VALUE_ERROR` error ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"NOTIFY_ID":288,"NOTIFY_VALUE":"Y"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify.confirm
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"NOTIFY_ID":288,"NOTIFY_VALUE":"Y","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.notify.confirm
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type NotifyConfirmResult = {
      result_message: string[] | false
    }

    try {
      const response = await $b24.actions.v2.call.make<NotifyConfirmResult>({
        method: 'im.notify.confirm',
        params: {
          NOTIFY_ID: 288,
          NOTIFY_VALUE: 'Y',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.result_message)
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
      async function confirmNotification() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.notify.confirm',
            params: {
              NOTIFY_ID: 288,
              NOTIFY_VALUE: 'Y',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.result_message)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', confirmNotification)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.notify.confirm(
            notify_id=288,
            notify_value=True,
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
            'im.notify.confirm',
            [
                'NOTIFY_ID' => 288,
                'NOTIFY_VALUE' => 'Y',
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
        'im.notify.confirm',
        {
            NOTIFY_ID: 288,
            NOTIFY_VALUE: 'Y',
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
        'im.notify.confirm',
        [
            'NOTIFY_ID' => 288,
            'NOTIFY_VALUE' => 'Y',
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
    res, err := client.Core().Call(ctx, "im.notify.confirm", b24.Params{
    	"NOTIFY_ID":    288,
    	"NOTIFY_VALUE": "Y",
    })
    if err != nil {
    	return fmt.Errorf("im.notify.confirm: %w", err)
    }

    // The method wraps the response in an object with the "result_message" key.
    raw, ok := b24.Unwrap(res.Result, "result_message")
    if !ok {
    	return fmt.Errorf("no result_message key in the response")
    }

    fmt.Printf("%s\n", raw)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "result_message": [
            "Your selection: \"Accept\""
        ]
    },
    "time": {
        "start": 1794317487,
        "finish": 1794317487.750427,
        "duration": 0.7504270076751709,
        "processing": 0,
        "date_start": "2026-11-10T16:31:27+01:00",
        "date_finish": "2026-11-10T16:31:27+01:00",
        "operating_reset_at": 1794318060,
        "operating": 0
    }
}
```

If the notification is not found among the current user's notifications or has no buttons, the method returns HTTP status **200** and `false` in the `result_message` field:

```json
{
    "result": {
        "result_message": false
    },
    "time": {
        "start": 1794317487,
        "finish": 1794317487.750427,
        "duration": 0.7504270076751709,
        "processing": 0,
        "date_start": "2026-11-10T16:31:27+01:00",
        "date_finish": "2026-11-10T16:31:27+01:00",
        "operating_reset_at": 1794318060,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing the result of the button press [(detailed description)](#result-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result-object}

#|
|| **Name**
`Type` | **Description** ||
|| **result_message**
[`array`](../../data-types.md) 
[`boolean`](../../data-types.md) | Result of the button press:
- an array of strings — messages from the module that processed the button press. If the module returned no message, the array contains the text `Your selection: "<button name>"`. If the `NOTIFY_VALUE` value does not match any button, the array may be empty
- `false` — the notification with the specified identifier is not found among the current user's notifications, or the notification has no buttons

After the button is pressed, the notification is deleted, even if the array is empty ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOTIFY_VALUE_ERROR",
    "error_description": "Notification Value can't be empty"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ID_ERROR` | Notification ID can't be empty | The `NOTIFY_ID` and `ID` parameters are not provided, or the value is less than or equal to `0` ||
|| `400` | `NOTIFY_VALUE_ERROR` | Notification Value  can't be empty | The `NOTIFY_VALUE` parameter is not provided, is empty, or equals the string `0` ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-notify.md)
- [{#T}](./im-notify-personal-add.md)
- [{#T}](./im-notify-system-add.md)
- [{#T}](./im-notify-get.md)
- [{#T}](./im-notify-schema-get.md)
- [{#T}](./im-notify-read-list.md)
- [{#T}](./im-notify-read.md)
- [{#T}](./im-notify-read-all.md)
- [{#T}](./im-notify-answer.md)
- [{#T}](./im-notify-delete.md)
- [{#T}](./im-notify-history-search.md)
