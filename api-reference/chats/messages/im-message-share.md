# Create a Chat, Task, Post, or Event from a Message im.message.share

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant

The method `im.message.share` creates a chat, a task, a feed post, or a calendar event from a message. For example, you can turn a colleague's request in a chat into a task right away.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **MESSAGE_ID***
[`integer`](../../data-types.md) | Identifier of the message.

The identifier is returned by the methods [im.message.add](./im-message-add.md) and [im.dialog.messages.get](./im-dialog-messages-get.md) ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier of the dialog in the format:

- `chatXXX` — group chat, where `XXX` is the chat identifier
- `sgXXX` — group or project chat, where `XXX` is the group identifier
- `XXX` — personal dialog, where `XXX` is the user identifier

The [im.chat.get](../im-chat-get.md) method returns the chat identifier. The [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods return the user identifier ||
|| **TYPE***
[`string`](../../data-types.md) | Type of the object being created.

Allowed values:
- `CHAT` — chat
- `TASK` — task
- `POST` — post in the feed
- `CALEND` — calendar event

If you pass another value or omit the parameter, the method returns `true` but creates nothing ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34261,"DIALOG_ID":"chat2941","TYPE":"TASK"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.share
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34261,"DIALOG_ID":"chat2941","TYPE":"TASK","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.message.share
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
        method: 'im.message.share',
        params: {
          MESSAGE_ID: 34261,
          DIALOG_ID: 'chat2941',
          TYPE: 'TASK',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Request completed:', result)
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
      async function shareMessage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.message.share',
            params: {
              MESSAGE_ID: 34261,
              DIALOG_ID: 'chat2941',
              TYPE: 'TASK',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Request completed:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', shareMessage)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.message.share(
            message_id=34261,
            dialog_id="chat2941",
            type="TASK",
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
        $response = $b24Service
            ->core
            ->call(
                'im.message.share',
                [
                    'MESSAGE_ID' => 34261,
                    'DIALOG_ID' => 'chat2941',
                    'TYPE' => 'TASK'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sharing message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.message.share',
        {
            MESSAGE_ID: 34261,
            DIALOG_ID: 'chat2941',
            TYPE: 'TASK'
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.message.share',
        [
            'MESSAGE_ID' => 34261,
            'DIALOG_ID' => 'chat2941',
            'TYPE' => 'TASK'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.message.share", b24.Params{
    	"MESSAGE_ID": 34261,
    	"DIALOG_ID":  "chat2941",
    	"TYPE":       "TASK",
    })
    if err != nil {
    	return fmt.Errorf("im.message.share: %w", err)
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
        "start": 1772632079,
        "finish": 1772632079.779834,
        "duration": 0.7798340320587158,
        "processing": 0,
        "date_start": "2026-03-04T16:47:59+01:00",
        "date_finish": "2026-03-04T16:47:59+01:00",
        "operating_reset_at": 1772632679,
        "operating": 0.5939757823944092
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` — the request is processed. The method returns `true` even when no object is created because of an invalid `TYPE`.

The method does not return the identifier of the created object. You can locate the object using the system message that appears in the chat after the call. This message is returned by the [im.dialog.messages.get](./im-dialog-messages-get.md) method. For a task, a post, and an event, the message text contains a link to the object; for a new chat, the identifier is passed in the `params.CHAT_ID` field ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "PARAMS_ERROR",
    "error_description": "Incorrect params"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `MESSAGE_ID_ERROR` | Message ID can't be empty | The `MESSAGE_ID` parameter is missing or passed in an invalid format ||
|| `400` | `DIALOG_ID_EMPTY` | Dialog ID can't be empty | The `DIALOG_ID` parameter is missing or passed in an invalid format ||
|| `400` | `PARAMS_ERROR` | Incorrect params | The object is not created: the message is not found, the user has no permission to post in the group feed, the user is an external (extranet) user, or the creation failed ||
|| `403` | `ACCESS_ERROR` | You do not have access to the specified dialog | The chat is not found or the current user has no access to it ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-message-add.md)
- [{#T}](./im-message-update.md)
- [{#T}](./im-message-delete.md)
- [{#T}](./im-message-like.md)
- [{#T}](./im-message-command.md)
- [{#T}](./im-dialog-messages-get.md)
- [{#T}](./im-dialog-messages-search.md)
- [{#T}](./im-dialog-read.md)
- [{#T}](./im-dialog-unread.md)
- [{#T}](./im-dialog-writing.md)