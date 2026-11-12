# Mark Messages as Read im.dialog.read

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.dialog.read` marks chat messages as read up to and including the specified one. The method marks messages as read only for the user on whose behalf it is called.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier of the dialog in the format:

- `chatXXX` — group chat, where `XXX` is the chat identifier
- `sgXXX` — group or project chat, where `XXX` is the group identifier
- `XXX` — personal dialog, where `XXX` is the user identifier

The [im.chat.get](../im-chat-get.md) method returns the chat identifier. The [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods return the user identifier ||
|| **MESSAGE_ID**
[`integer`](../../data-types.md) | Identifier of the last message to mark as read. If you omit it, the method marks all unread messages as read ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat1489","MESSAGE_ID":84875}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.dialog.read
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat1489","MESSAGE_ID":84875,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.dialog.read
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ImDialogReadResult = {
      dialogId: string | number,
      chatId: number,
      lastId: number,
      counter: number,
    }

    try {
      const response = await $b24.actions.v2.call.make<ImDialogReadResult | false>({
        method: 'im.dialog.read',
        params: {
          DIALOG_ID: 'chat1489',
          MESSAGE_ID: 84875,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        if (result === false) {
          console.info('Messages are not marked as read')
        } else {
          console.info(result.dialogId, result.chatId, result.lastId, result.counter)
        }
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
      async function markDialogAsRead() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.dialog.read',
            params: {
              DIALOG_ID: 'chat1489',
              MESSAGE_ID: 84875,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          if (result === false) {
            console.info('Messages are not marked as read')
            return
          }
          console.info(result.dialogId, result.chatId, result.lastId, result.counter)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', markDialogAsRead)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.dialog.read(
            dialog_id="chat1489",
            message_id=84875,
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
            'im.dialog.read',
            [
                'DIALOG_ID' => 'chat1489',
                'MESSAGE_ID' => 84875,
            ]
        );

        $result = $response->getResponseData()->getResult();
        print_r($result);
    } catch (Throwable $e) {
        error_log($e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.dialog.read',
        {
            DIALOG_ID: 'chat1489',
            MESSAGE_ID: 84875
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
        'im.dialog.read',
        [
            'DIALOG_ID' => 'chat1489',
            'MESSAGE_ID' => 84875,
        ]
    );

    print_r($result);
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.dialog.read", b24.Params{
    	"DIALOG_ID":  "chat1489",
    	"MESSAGE_ID": 84875,
    })
    if err != nil {
    	return fmt.Errorf("im.dialog.read: %w", err)
    }

    // If marking the messages fails, result is false
    if string(res.Result) == "false" {
    	fmt.Println("Messages are not marked as read")
    	return nil
    }

    var item struct {
    	DialogID any    `json:"dialogId"`
    	ChatID   b24.ID `json:"chatId"`
    	LastID   b24.ID `json:"lastId"`
    	Counter  int    `json:"counter"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.DialogID, item.ChatID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "dialogId": "chat1489",
        "chatId": 1489,
        "lastId": 84875,
        "counter": 3
    },
    "time": {
        "start": 1772624912,
        "finish": 1772624912.615753,
        "duration": 0.6157529354095459,
        "processing": 0,
        "date_start": "2026-03-04T14:48:32+01:00",
        "date_finish": "2026-03-04T14:48:32+01:00",
        "operating_reset_at": 1772625512,
        "operating": 0
    }
}
```

If the current user is not a chat member or the messages could not be marked, the method returns HTTP status **200** and `false`:

```json
{
    "result": false,
    "time": {
        "start": 1772624912,
        "finish": 1772624912.615753,
        "duration": 0.6157529354095459,
        "processing": 0,
        "date_start": "2026-03-04T14:48:32+01:00",
        "date_finish": "2026-03-04T14:48:32+01:00",
        "operating_reset_at": 1772625512,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md)
[`boolean`](../../data-types.md) | Root element of the response [(detailed description)](#result).

Equals `false` if the current user is not a chat member or the messages could not be marked ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **dialogId**
[`string`](../../data-types.md)
[`integer`](../../data-types.md) | Chat identifier in the `DIALOG_ID` format. For a personal dialog, it is a number, the user identifier ||
|| **chatId**
[`integer`](../../data-types.md) | Identifier of the chat ||
|| **lastId**
[`integer`](../../data-types.md) | Identifier of the last read message ||
|| **counter**
[`integer`](../../data-types.md) | Number of unread messages after executing the method. In a group chat, a value greater than 99 is returned as `100` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `DIALOG_ID_EMPTY` | Dialog ID can't be empty | The `DIALOG_ID` parameter was not provided, was empty, or was in an incorrect format ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-message-add.md)
- [{#T}](./im-message-update.md)
- [{#T}](./im-message-delete.md)
- [{#T}](./im-message-like.md)
- [{#T}](./im-message-share.md)
- [{#T}](./im-message-command.md)
- [{#T}](./im-dialog-messages-get.md)
- [{#T}](./im-dialog-messages-search.md)
- [{#T}](./im-dialog-unread.md)
- [{#T}](./im-dialog-writing.md)