# Get e-mail mail.message.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`mail`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the mailbox where the e-mail is located

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `mail.message.get` method retrieves an e-mail by its identifier. Unlike [mail.message.list](./mail-message-list.md), the method returns the CC recipients `cc` and the e-mail text `body`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | E-mail identifier.

The identifier can be obtained using the [mail.message.list](./mail-message-list.md) method. ||
|| **select**
[`array`](../../data-types.md) | List of e-mail fields. The method checks the field names and returns an error for an unknown field, but currently the response always contains all e-mail fields.

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

{% include [Note on examples](../../../_includes/examples.md) %}

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
    import type { B24Frame } from '@bitrix24/b24jssdk'

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
        cc: string
        date: string
        isSeen: boolean
        hasAttachments: boolean
        url: string
        bindings: Array<{
          type: 'crm' | 'task' | 'chat' | 'calendarEvent' | 'blogPost'
          entityTypeId?: number
          entityId: number
        }>
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
        console.info('E-mail:', result.item.id, result.item.subject, result.item.from)
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
          console.info('E-mail:', result.item.id, result.item.subject, result.item.from)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchMessage)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    select = [
        "id",
        "mailboxId",
        "mailboxEmail",
        "subject",
        "from",
        "to",
        "cc",
        "date",
        "isSeen",
        "hasAttachments",
        "url",
        "bindings",
        "body",
    ]

    try:
        bitrix_response = client.mail.message.get(
            bitrix_id=15,
            select=select,
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "mail.message.get", b24.Params{
    	"id":     15,
    	"select": []string{"id", "mailboxId", "mailboxEmail", "subject", "from", "to", "cc", "date", "isSeen", "hasAttachments", "url", "bindings", "body"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("mail.message.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID           b24.ID `json:"id"`
    	MailboxID    b24.ID `json:"mailboxId"`
    	MailboxEmail string `json:"mailboxEmail"`
    	Subject      string `json:"subject"`
    	From         string `json:"from"`
    	To           string `json:"to"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.MailboxID)
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
            "cc": "",
            "date": "2026-05-25 10:00:00",
            "isSeen": true,
            "hasAttachments": false,
            "url": "https://example.bitrix24.com/mail/message/15",
            "bindings": [],
            "body": "Good afternoon! I am sending the draft agreement."
        }
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
[`object`](../../data-types.md) | Response data object ||
|| **item**
[`object`](../../data-types.md) | E-mail object. All fields are returned regardless of `select` ||
|| **id**
[`integer`](../../data-types.md) | E-mail identifier ||
|| **mailboxId**
[`integer`](../../data-types.md) | Mailbox identifier ||
|| **mailboxEmail**
[`string`](../../data-types.md) | Mailbox address ||
|| **subject**
[`string`](../../data-types.md) | E-mail subject ||
|| **from**
[`string`](../../data-types.md) | E-mail sender ||
|| **to**
[`string`](../../data-types.md) | E-mail recipients ||
|| **cc**
[`string`](../../data-types.md) | CC recipients. If there are none, an empty string is returned ||
|| **date**
[`string`](../../data-types.md) | Date and time the e-mail was received in the `YYYY-MM-DD HH:MM:SS` format, without a time zone ||
|| **isSeen**
[`boolean`](../../data-types.md) | Read flag: `true` — read, `false` — unread ||
|| **hasAttachments**
[`boolean`](../../data-types.md) | Attachments flag ||
|| **url**
[`string`](../../data-types.md) | Full link to the e-mail in Bitrix24 ||
|| **bindings**
[`array`](../../data-types.md) | Links of the e-mail to Bitrix24 objects. If there are none, an empty array `[]` is returned. The item format is described on the page of the [mail.message.list](./mail-message-list.md#returned-data) method ||
|| **body**
[`string`](../../data-types.md) | E-mail text without HTML markup. Lines are separated by `\r\n`. Special characters are returned as HTML entities, for example, `&lt;` instead of `<`. In replies and forwards sent from Bitrix24, the text includes the quote of the original e-mail ||
|| **time**
[`time`](../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID `15` was not found"
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors
Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `id` | Required field `id` is missing | Add `id` to the request body ||
|#

#### Data Not Found Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `id` | Entity with ID `15` was not found | The e-mail does not exist, the user has no access to the e-mail's mailbox, or the mailbox is disabled. The method treats a non-numeric `id` value, such as `abc`, as `0` and returns the same error with ID `0`. Retrieve the identifier using the [mail.message.list](./mail-message-list.md) method ||
|#

#### Errors in the select Parameter
Error code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `select` | Unknown field `foo` found in entity `MessageDto` | Pass only fields from the list of the `select` parameter ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `select` | Invalid field in `select`: `["id"]` | Pass `select` as an array of strings, for example, `["id","subject"]` ||
|#

#### Access Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `-` | Access denied | Make sure the application or webhook has the `mail` scope ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-list.md)
- [{#T}](./mail-message-field-list.md)
- [{#T}](./index.md)
