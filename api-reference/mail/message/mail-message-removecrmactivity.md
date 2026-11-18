# Delete CRM Activities of an E-mail mail.message.removecrmactivity

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`mail`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Manage CRM related mailbox options" permission, full access to the e-mail, and the permission to delete CRM activities

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `mail.message.removecrmactivity` method deletes CRM activities created from an e-mail.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **messageId***
[`integer`](../../data-types.md) | E-mail identifier.

The identifier can be obtained using the [mail.message.list](./mail-message-list.md) method ||
|#

The method deletes only the activities the user is allowed to delete under CRM permissions. The other activities remain in CRM together with their link to the e-mail, and the response contains `complete: false`.

If all activities are deleted, the method adds the sender address of an incoming e-mail to the CRM exclusion list so that new e-mails from this sender do not get into CRM. The address is added only if the user has the permission to edit the exclusion list.

Users with access to the mailbox have full access to the e-mail. If the e-mail is available to the user only through a linked task, chat, event, or Feed post, the method returns an error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/mail.message.removecrmactivity`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"messageId":15}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/mail.message.removecrmactivity
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"messageId":15,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/mail.message.removecrmactivity
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type RemoveCrmActivityResult = {
      result: boolean
      deletedCount: number
      totalCount: number
      complete: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<RemoveCrmActivityResult>({
        method: 'mail.message.removecrmactivity',
        params: {
          messageId: 15,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Deleted activities:', result.deletedCount, 'of', result.totalCount, 'all deleted:', result.complete)
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
      async function removeCrmActivity() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'mail.message.removecrmactivity',
            params: {
              messageId: 15,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Deleted activities:', result.deletedCount, 'of', result.totalCount, 'all deleted:', result.complete)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', removeCrmActivity)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.mail.message.removecrmactivity(
            message_id=15,
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
                'mail.message.removecrmactivity',
                [
                    'messageId' => 15
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'mail.message.removecrmactivity',
        {
            messageId: 15
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'mail.message.removecrmactivity',
        [
            'messageId' => 15
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "mail.message.removecrmactivity", b24.Params{
    	"messageId": 15,
    })
    if err != nil {
    	return fmt.Errorf("mail.message.removecrmactivity: %w", err)
    }

    var item struct {
    	Result       bool `json:"result"`
    	DeletedCount int  `json:"deletedCount"`
    	TotalCount   int  `json:"totalCount"`
    	Complete     bool `json:"complete"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.DeletedCount, item.TotalCount, item.Complete)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": true,
        "deletedCount": 1,
        "totalCount": 1,
        "complete": true
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
[`object`](../../data-types.md) | Object with the deletion result ||
|| **result.result**
[`boolean`](../../data-types.md) | Always `true` in a successful response ||
|| **result.deletedCount**
[`integer`](../../data-types.md) | Number of deleted activities ||
|| **result.totalCount**
[`integer`](../../data-types.md) | Number of CRM activities created from the e-mail ||
|| **result.complete**
[`boolean`](../../data-types.md) | Whether all activities were deleted. Possible values:
- `true` — all activities of the e-mail were deleted
- `false` — some activities remain in CRM because the user has no permission to delete them ||
|| **time**
[`time`](../../data-types.md#time) | Request execution time information ||
|#

{% note warning "" %}

Currently, Bitrix24 returns only `result.result`, without the `deletedCount`, `totalCount`, and `complete` counters. The method responds this way even if the e-mail has no CRM activities. To check the result, retrieve the e-mail using [mail.message.get](./mail-message-get.md) and look for links of the `crm` type in the `bindings` field.

{% endnote %}

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Request object validation error",
        "validation": [
            {
                "message": "Parameter \"messageId\" is required.",
                "field": "MISSING_MESSAGE_ID"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors
Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

The "Field" column shows the `field` value from the `validation` element. The method passes a reason code in it, not a parameter name. Errors without a code have no `field` in the response.

#|
|| **Field** | **Error description** | **How to fix** ||
|| `MISSING_MESSAGE_ID` | Parameter "messageId" is required. | `messageId` is not provided, is not a number, or is less than `1`. Pass the e-mail ID as a positive integer ||
|| `-` | The CRM module was not found. Please install this module. | The CRM module is not installed in Bitrix24 ||
|| `MAIL_MESSAGE_CRM_ACCESS_DENIED` | Access to CRM denied. | The user does not have the "Manage CRM related mailbox options" permission in the mail access permissions. Grant the permission or call the method on behalf of another user ||
|| `-` | Message was deleted or moved to another folder. | There is no e-mail with this `messageId`, or the user has no access to it. Check the ID using [mail.message.list](./mail-message-list.md) ||
|| `MAIL_MESSAGE_CRM_ACCESS_DENIED` | Access denied: full access to the message is required | The e-mail is available to the user only through a linked object. Call the method on behalf of a user with access to the mailbox ||
|| `MAIL_MESSAGE_CRM_ACCESS_DENIED` | Access denied: cannot delete CRM activity | The user has no permission to delete any of the e-mail's activities. Check the user's permissions to delete activities in CRM ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-createcrmactivity.md)
- [{#T}](./index.md)
