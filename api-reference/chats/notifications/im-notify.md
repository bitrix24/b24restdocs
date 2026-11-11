# Send Notification im.notify

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `im.notify` method sends a notification to a user.

{% note info "" %}

The method cannot be called with session authorization — it returns the `WRONG_AUTH_TYPE` error. Call it via a webhook or with an application token.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **USER_ID***
[`integer`](../../data-types.md) | The identifier of the user receiving the notification. The recipient must be an active Bitrix24 user: if the user is not found or is inactive, the method returns `result: false`.

You can retrieve the user ID using the [user.get](../../user/user-get.md), [user.search](../../user/user-search.md), or [im.user.get](../users/im-user-get.md) methods. ||
|| **TYPE**
[`string`](../../data-types.md) | The type of notification.

Allowed values:
- `USER` — personal notification on behalf of the current user, as in the [im.notify.personal.add](./im-notify-personal-add.md) method
- `SYSTEM` — system notification without an author, as in the [im.notify.system.add](./im-notify-system-add.md) method

Default value — `USER`. The method also replaces any other value with `USER` ||
|| **MESSAGE***
[`string`](../../data-types.md) | The text of the notification. The method trims whitespace from the ends of the string; a string consisting only of spaces causes the `MESSAGE_EMPTY` error. Supports BB codes — for details, see [Message Formatting](../messages/formatting.md).

For `TYPE: SYSTEM` called from an application, the method adds the application name as the first line of the text ||
|| **MESSAGE_OUT**
[`string`](../../data-types.md) | The text of the notification for external channels, such as email. For `TYPE: SYSTEM` called from an application, the method adds the application name as the first line of the text ||
|| **TAG**
[`string`](../../data-types.md) | The notification tag. If the recipient already has a notification from this application with the same tag, the previous notification is deleted.

The tag is retained only when the method is called from an application: the method appends the application key to it, so tags of different applications do not overlap. When the method is called via a webhook, the parameter is ignored ||
|| **SUB_TAG**
[`string`](../../data-types.md) | An additional notification tag without uniqueness checks. Notifications can be deleted by the `TAG` and `SUB_TAG` tags using the [im.notify.delete](./im-notify-delete.md) method.

The tag is retained only when the method is called from an application. When the method is called via a webhook, the parameter is ignored ||
|| **ATTACH**
[`array`](../../data-types.md)
[`object`](../../data-types.md) 
[`string`](../../data-types.md) | A notification attachment: an array of blocks, an object with the `BLOCKS` key, or a JSON string. The attachment size in JSON must be less than 60,000 characters. The block format is described in the [Attachments](../messages/attachments.md) section ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USER_ID":5,"TYPE":"SYSTEM","MESSAGE":"Invoice [B]#42[/B] has been paid","MESSAGE_OUT":"Invoice #42 has been paid","ATTACH":[{"MESSAGE":"Amount: 15,000 EUR"},{"LINK":{"NAME":"Open invoice","LINK":"https://example.com/invoice/42"}}]}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USER_ID":5,"TYPE":"SYSTEM","MESSAGE":"Invoice [B]#42[/B] has been paid","MESSAGE_OUT":"Invoice #42 has been paid","ATTACH":[{"MESSAGE":"Amount: 15,000 EUR"},{"LINK":{"NAME":"Open invoice","LINK":"https://example.com/invoice/42"}}],"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.notify
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number | false>({
        method: 'im.notify',
        params: {
          USER_ID: 5,
          TYPE: 'SYSTEM',
          MESSAGE: 'Invoice [B]#42[/B] has been paid',
          MESSAGE_OUT: 'Invoice #42 has been paid',
          ATTACH: [
            { MESSAGE: 'Amount: 15,000 EUR' },
            { LINK: { NAME: 'Open invoice', LINK: 'https://example.com/invoice/42' } },
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Notification ID:', result)
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
      async function sendNotification() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.notify',
            params: {
              USER_ID: 5,
              TYPE: 'SYSTEM',
              MESSAGE: 'Invoice [B]#42[/B] has been paid',
              MESSAGE_OUT: 'Invoice #42 has been paid',
              ATTACH: [
                { MESSAGE: 'Amount: 15,000 EUR' },
                { LINK: { NAME: 'Open invoice', LINK: 'https://example.com/invoice/42' } },
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Notification ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', sendNotification)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.notify(
            user_id=5,
            type="SYSTEM",
            message="Invoice [B]#42[/B] has been paid",
            message_out="Invoice #42 has been paid",
            attach=[
                {"MESSAGE": "Amount: 15,000 EUR"},
                {"LINK": {"NAME": "Open invoice", "LINK": "https://example.com/invoice/42"}},
            ],
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
            'im.notify',
            [
                'USER_ID' => 5,
                'TYPE' => 'SYSTEM',
                'MESSAGE' => 'Invoice [B]#42[/B] has been paid',
                'MESSAGE_OUT' => 'Invoice #42 has been paid',
                'ATTACH' => [
                    ['MESSAGE' => 'Amount: 15,000 EUR'],
                    ['LINK' => ['NAME' => 'Open invoice', 'LINK' => 'https://example.com/invoice/42']],
                ],
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
        'im.notify',
        {
            USER_ID: 5,
            TYPE: 'SYSTEM',
            MESSAGE: 'Invoice [B]#42[/B] has been paid',
            MESSAGE_OUT: 'Invoice #42 has been paid',
            ATTACH: [
                { MESSAGE: 'Amount: 15,000 EUR' },
                { LINK: { NAME: 'Open invoice', LINK: 'https://example.com/invoice/42' } },
            ],
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
        'im.notify',
        [
            'USER_ID' => 5,
            'TYPE' => 'SYSTEM',
            'MESSAGE' => 'Invoice [B]#42[/B] has been paid',
            'MESSAGE_OUT' => 'Invoice #42 has been paid',
            'ATTACH' => [
                ['MESSAGE' => 'Amount: 15,000 EUR'],
                ['LINK' => ['NAME' => 'Open invoice', 'LINK' => 'https://example.com/invoice/42']],
            ],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } elseif ($result['result'] === false) {
        echo 'Notification not sent';
    } else {
        echo 'Notification ID: ' . $result['result'];
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.notify", b24.Params{
    	"USER_ID":     5,
    	"TYPE":        "SYSTEM",
    	"MESSAGE":     "Invoice [B]#42[/B] has been paid",
    	"MESSAGE_OUT": "Invoice #42 has been paid",
    	"ATTACH": []map[string]any{
    		{"MESSAGE": "Amount: 15,000 EUR"},
    		{"LINK": map[string]any{"NAME": "Open invoice", "LINK": "https://example.com/invoice/42"}},
    	},
    })
    if err != nil {
    	return fmt.Errorf("im.notify: %w", err)
    }

    // result is the notification ID, or false if the notification was not created
    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("notification not created: %s", res.Result)
    }
    fmt.Println("ID:", newID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 44003,
    "time": {
        "start": 1794317396,
        "finish": 1794317396.101083,
        "duration": 0.10108304023742676,
        "processing": 0,
        "date_start": "2026-11-10T16:29:56+01:00",
        "date_finish": "2026-11-10T16:29:56+01:00",
        "operating_reset_at": 1794317940,
        "operating": 0
    }
}
```

If the recipient is not found or is inactive, the method returns HTTP status **200** and `false`:

```json
{
    "result": false,
    "time": {
        "start": 1794317624,
        "finish": 1794317624.580945,
        "duration": 0.5809450149536133,
        "processing": 0,
        "date_start": "2026-11-10T16:33:44+01:00",
        "date_finish": "2026-11-10T16:33:44+01:00",
        "operating_reset_at": 1794318180,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) 
[`boolean`](../../data-types.md) | The identifier of the created notification. Pass it in `ID` of the [im.notify.delete](./im-notify-delete.md) method to delete the notification.

The `false` value means that the notification was not created: the recipient is not found or is inactive ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "USER_ID_EMPTY",
    "error_description": "User ID can't be empty"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Access for this method not allowed by session authorization. | The method was called with session authorization, which is prohibited for it ||
|| `400` | `USER_ID_EMPTY` | User ID can't be empty | The `USER_ID` parameter is not passed or its value is not greater than `0` ||
|| `400` | `MESSAGE_EMPTY` | Message can't be empty | The `MESSAGE` parameter is not passed, is not a string, or contains only spaces ||
|| `400` | `ATTACH_OVERSIZE` | You have exceeded the maximum allowable size of attach | The size of the `ATTACH` attachment in JSON is 60,000 characters or more ||
|| `400` | `ATTACH_ERROR` | Incorrect attach params | The `ATTACH` attachment contains no valid blocks, or a string that is not JSON is passed ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-notify-personal-add.md)
- [{#T}](./im-notify-system-add.md)
- [{#T}](./im-notify-get.md)
- [{#T}](./im-notify-schema-get.md)
- [{#T}](./im-notify-read-list.md)
- [{#T}](./im-notify-read.md)
- [{#T}](./im-notify-read-all.md)
- [{#T}](./im-notify-answer.md)
- [{#T}](./im-notify-confirm.md)
- [{#T}](./im-notify-delete.md)
- [{#T}](./im-notify-history-search.md)