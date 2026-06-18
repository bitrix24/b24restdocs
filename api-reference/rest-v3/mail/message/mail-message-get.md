# Get e-mail mail.message.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the mailbox where the e-mail is located

The `mail.message.get` method retrieves an e-mail by its identifier.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | E-mail identifier.

The identifier can be obtained using the [mail.message.list](./mail-message-list.md) method. ||
|| **select**
[`array`](../../../data-types.md) | List of e-mail fields to be returned.

Available fields:
- `id` — e-mail identifier
- `mailboxId` — mailbox identifier
- `mailboxEmail` — mailbox e-mail address
- `subject` — e-mail subject
- `from` — sender
- `to` — recipient
- `cc` — copy
- `date` — e-mail date
- `isSeen` — read flag
- `hasAttachments` — attachment flag
- `url` — link to the e-mail
- `bindings` — object bindings
- `body` — e-mail text ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/mail.message.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"select":["id","mailboxId","mailboxEmail","subject","from","to","cc","date","isSeen","hasAttachments","url","bindings","body"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/mail.message.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"select":["id","mailboxId","mailboxEmail","subject","from","to","cc","date","isSeen","hasAttachments","url","bindings","body"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/mail.message.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MessageGetResult = {
      item: {
        id: number
        mailboxId: number
        mailboxEmail: string
        subject: string
        from: string
        to: string
        cc: string | null
        date: ISODate | null
        isSeen: boolean
        hasAttachments: boolean
        url: string
        bindings: unknown[]
        body: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<MessageGetResult>({
        method: 'mail.message.get',
        params: {
          id: 15,
          select: [
            'id',
            'mailboxId',
            'mailboxEmail',
            'subject',
            'from',
            'to',
            'cc',
            'date',
            'isSeen',
            'hasAttachments',
            'url',
            'bindings',
            'body',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Message:', result.item.id, result.item.subject, result.item.from)
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
      async function fetchMessage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'mail.message.get',
            params: {
              id: 15,
              select: [
                'id',
                'mailboxId',
                'mailboxEmail',
                'subject',
                'from',
                'to',
                'cc',
                'date',
                'isSeen',
                'hasAttachments',
                'url',
                'bindings',
                'body',
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
          console.info('Message:', result.item.id, result.item.subject, result.item.from)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchMessage)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'mail.message.get',
                [
                    'id' => 15,
                    'select' => [
                        'id',
                        'mailboxId',
                        'mailboxEmail',
                        'subject',
                        'from',
                        'to',
                        'cc',
                        'date',
                        'isSeen',
                        'hasAttachments',
                        'url',
                        'bindings',
                        'body'
                    ]
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'mail.message.get',
        {
            id: 15,
            select: [
                'id',
                'mailboxId',
                'mailboxEmail',
                'subject',
                'from',
                'to',
                'cc',
                'date',
                'isSeen',
                'hasAttachments',
                'url',
                'bindings',
                'body'
            ]
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'mail.message.get',
        [
            'id' => 15,
            'select' => [
                'id',
                'mailboxId',
                'mailboxEmail',
                'subject',
                'from',
                'to',
                'cc',
                'date',
                'isSeen',
                'hasAttachments',
                'url',
                'bindings',
                'body'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 15,
            "mailboxId": 1,
            "mailboxEmail": "user@example.com",
            "subject": "Email subject",
            "from": "client@example.com",
            "to": "user@example.com",
            "cc": null,
            "date": "2026-05-25T10:00:00+03:00",
            "isSeen": true,
            "hasAttachments": false,
            "url": "/mail/message/15",
            "bindings": [],
            "body": "Email body"
        }
    },
    "time": {
        "start": 1779819678,
        "finish": 1779819678.84803,
        "duration": 0.8480300903320312,
        "processing": 0,
        "date_start": "2026-05-26T21:21:18+03:00",
        "date_finish": "2026-05-26T21:21:18+03:00",
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
[`object`](../../../data-types.md) | Response data object ||
|| **item**
[`object`](../../../data-types.md) | E-mail object. The response structure depends on `select` ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Record with ID = `15` not found"
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors
Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `id` | Mandatory field `id` is not specified | Add `id` to the request body ||
|#

#### Access Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `-` | Access denied | Check user permissions and `mail` scope ||
|#

#### Data Not Found Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `id` | Record with the specified identifier not found | Specify the identifier of an existing object ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-list.md)
- [{#T}](./mail-message-field-list.md)
- [{#T}](./index.md)
