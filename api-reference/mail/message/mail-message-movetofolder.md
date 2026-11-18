# Move e-mails to folder mail.message.movetofolder

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`mail`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the mailbox where the e-mails are located. For `action: "delete"`, the user also needs the permission to delete e-mails

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `mail.message.movetofolder` method moves e-mails to a folder, spam, or the trash of the mailbox.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **messageIds***
[`array`](../../data-types.md) | A non-empty array of e-mail IDs, up to 100 elements.

All e-mails in the request must be in the same folder. For `action: "move"`, the e-mails must also belong to the same mailbox.

IDs can be obtained using the [mail.message.list](./mail-message-list.md) method ||
|| **action***
[`string`](../../data-types.md) | Action for e-mails. The value is case-sensitive.

Possible values:

- `move` — move to the folder from `folder`
- `spam` — move to the spam folder of the mailbox
- `delete` — move to the trash of the mailbox. If the e-mails are already in the trash, the method deletes them permanently ||
|| **folder**
[`string`](../../data-types.md) | Folder to move the e-mails to. Required if `action` is `move`.

Pass the folder name or its full path on the mail server, for example `INBOX/Archive`. The value is case-insensitive.

If you specify the spam or trash folder, the method works as `action: "spam"` or `action: "delete"`. If you specify the inbox folder, the method returns e-mails from spam to the inbox ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/mail.message.movetofolder`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"messageIds":[15,16],"action":"spam"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/mail.message.movetofolder
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"messageIds":[15,16],"action":"spam","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/mail.message.movetofolder
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MoveToFolderResult = {
      success: boolean
      movedCount: number
      action: string
    }

    try {
      const response = await $b24.actions.v3.call.make<MoveToFolderResult>({
        method: 'mail.message.movetofolder',
        params: {
          messageIds: [15, 16],
          action: 'spam',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('E-mails processed:', result.movedCount, 'Action:', result.action)
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
      async function moveToFolder() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'mail.message.movetofolder',
            params: {
              messageIds: [15, 16],
              action: 'spam',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('E-mails processed:', result.movedCount, 'Action:', result.action)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', moveToFolder)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.mail.message.movetofolder(
            message_ids=[
                15,
                16,
            ],
            action='spam',
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'mail.message.movetofolder',
                [
                    'messageIds' => [15, 16],
                    'action' => 'spam'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'mail.message.movetofolder',
        {
            messageIds: [15, 16],
            action: 'spam'
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'mail.message.movetofolder',
        [
            'messageIds' => [15, 16],
            'action' => 'spam'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "mail.message.movetofolder", b24.Params{
    	"messageIds": []int{15, 16},
    	"action":     "spam",
    })
    if err != nil {
    	return fmt.Errorf("mail.message.movetofolder: %w", err)
    }

    var item struct {
    	Success    bool   `json:"success"`
    	MovedCount int    `json:"movedCount"`
    	Action     string `json:"action"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Success, item.MovedCount)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "success": true,
        "movedCount": 2,
        "action": "spam"
    },
    "time": {
        "start": 1779819678,
        "finish": 1779819678.84803,
        "duration": 0.8480300903320312,
        "processing": 0,
        "date_start": "2026-05-26T21:21:18+01:00",
        "date_finish": "2026-05-26T21:21:18+01:00",
        "operating_reset_at": 1779820278,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the operation result ||
|| **success**
[`boolean`](../../data-types.md) | Operation result. Always `true` in a successful response: if the e-mails are not moved, the method returns an error ||
|| **movedCount**
[`integer`](../../data-types.md) | Number of processed e-mails. Only e-mails the user has access to are counted: the method skips inaccessible and non-existent IDs ||
|| **action**
[`string`](../../data-types.md) | Performed action: `move`, `spam`, or `delete` ||
|| **time**
[`time`](../../data-types.md#time) | Information about request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "message": "Parameter \"action\" must be \"move\", \"spam\", or \"delete\".",
                "field": "INVALID_ACTION"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

HTTP status: **400**

Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

In a response with this code, the `field` field contains the error reason code rather than the parameter name. Some errors have no `field` field. Error texts returned by the mail module are in the Bitrix24 language.

#|
|| **field value** | **Error description** | **How to fix** ||
|| `MISSING_MESSAGEIDS` | Parameter "messageIds" is required and must be a non-empty array. | Pass `messageIds` as a non-empty array. A number or a string instead of an array also causes this error ||
|| `MISSING_ACTION` | Parameter "action" is required. | Pass `action` ||
|| `INVALID_ACTION` | Parameter "action" must be "move", "spam", or "delete". | Pass `move`, `spam`, or `delete` in lowercase ||
|| `MISSING_FOLDER` | Parameter "folder" is required when action is "move". | Pass `folder` or use `action: "spam"`, `action: "delete"` ||
|| none | No accessible messages found. | None of the e-mails from `messageIds` were found, or the user has no access to their mailboxes. Check the IDs using the [mail.message.list](./mail-message-list.md) method ||
|| none | All messages must belong to the same mailbox. | For `action: "move"`, e-mails from different mailboxes were passed. Split the request by mailbox ||
|| `MAIL_CLIENT_MESSAGES_MULTIPLE_FOLDERS` | Select messages from only one folder | The e-mails are in different folders. Split the request by folder ||
|| `MAIL_CLIENT_MOVE_TO_SELF_FOLDER` | Message is already in selected folder | The e-mails are already in the `folder` folder. Exclude them from the request ||
|| `MAIL_CLIENT_MAILBOX_NOT_FOUND` | Mailbox was not found | The `folder` folder was not found in the mailbox. Check the folder name or path ||
|| `MAIL_CLIENT_FOLDER_IS_DISABLED` | Archive is not a folder | The `folder` folder is disabled for synchronization. Choose another folder ||
|| `MAIL_CLIENT_SPAM_FOLDER_NOT_SELECTED_ERROR` | Destination folder for spam is not selected | The spam folder is not selected in the mailbox settings. Select it in the mailbox settings ||
|| `MAIL_CLIENT_TRASH_FOLDER_NOT_SELECTED_ERROR` | Destination folder for deleted messages is not selected | The trash folder is not selected in the mailbox settings. Select it in the mailbox settings ||
|| `MAIL_CLIENT_MESSAGES_NOT_FOUND` | Messages are now being synchronized. Please try again later. | The e-mails are still being synchronized with the mail server. This happens with e-mails that have just been sent using the `mail.message.send`, `mail.message.reply`, and `mail.message.forward` methods. Retry the request later ||
|| `MAIL_CLIENT_SYNC_ERROR` | Data sync error | The mail server did not perform the operation. Retry the request later ||
|| `MAILBOX_MIGRATION_ACTION_BLOCKED` | The mailbox action is unavailable while its migration is active | The mailbox is being migrated. Retry the request after the migration is complete ||
|| `MAILBOX_MIGRATION_STATUS_UNAVAILABLE` | The mailbox migration status is temporarily unavailable | The mailbox migration status is temporarily unavailable. Retry the request later ||
|#

If `messageIds` contains more than 100 elements, Bitrix24 returns HTTP status **500** and the `INTERNAL_SERVER_ERROR` error. Split the e-mails into several requests.

#### Access Error

HTTP status: **403**

Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `-` | Access denied | For `action: "delete"` or moving to the trash folder, the user does not have the permission to delete e-mails. Ask the administrator to grant the permission in the mail permission settings ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-list.md)
- [{#T}](./mail-message-get.md)
