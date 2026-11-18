# Get e-mail thread mail.message.thread

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

The `mail.message.thread` method returns the conversation branch that contains the e-mail. Parallel branches, such as other replies to the same e-mail, are not included in the result.

E-mails are returned in pages, starting with the newest ones. Within a page, e-mails go from oldest to newest.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | The identifier of any e-mail from the thread.

The identifier can be obtained using the [mail.message.list](./mail-message-list.md) method. ||
|| **limit**
[`integer`](../../data-types.md) | Number of e-mails per page.

Default is `20`. The maximum is `50` if `withBodies` is `true`, and `100` if it is `false`. The method reduces a larger value to the maximum and replaces `0` or a smaller value with `20` ||
|| **offset**
[`integer`](../../data-types.md) | How many e-mails to skip, counting from the newest one. Default is `0`.

To retrieve the next page, increase `offset` by `limit` while `hasMore` is `true` in the response ||
|| **withBodies**
[`boolean`](../../data-types.md) | Whether to return the e-mail text.

Possible values:
- `true` — return the `body`, `truncated`, and `bodyLength` fields
- `false` — return only the e-mail headers

Default is `true` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/mail.message.thread`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"limit":20}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/mail.message.thread
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"limit":20,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/mail.message.thread
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each MailMessage returned in result[]
    type MailMessage = {
      id: number
      subject: string
      from: string
      to: string
      cc: string
      date: string
      body?: string
      truncated?: boolean
      bodyLength?: number
    }

    try {
      const response = await $b24.actions.v3.call.make<MailMessage[]>({
        method: 'mail.message.thread',
        params: {
          id: 15,
          limit: 20,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('E-mails on the page:', result.length)
        console.info('First message subject:', result[0]?.subject)
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
      async function getMailMessageThread() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'mail.message.thread',
            params: {
              id: 15,
              limit: 20,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('E-mails on the page:', result.length)
          console.info('First message subject:', result[0]?.subject)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getMailMessageThread)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.mail.message.thread(
            bitrix_id=15,
            limit=20,
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
                'mail.message.thread',
                [
                    'id' => 15,
                    'limit' => 20
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
        'mail.message.thread',
        {
            id: 15,
            limit: 20
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
        'mail.message.thread',
        [
            'id' => 15,
            'limit' => 20
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "mail.message.thread", b24.Params{
    	"id":    15,
    	"limit": 20,
    })
    if err != nil {
    	return fmt.Errorf("mail.message.thread: %w", err)
    }

    var items []struct {
    	ID      b24.ID `json:"id"`
    	Subject string `json:"subject"`
    	From    string `json:"from"`
    	To      string `json:"to"`
    	Cc      string `json:"cc"`
    	Date    string `json:"date"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Subject)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "id": 15,
            "subject": "Supply Agreement",
            "from": "John Smith <john.smith@example.com>",
            "to": "manager@example.com",
            "cc": "",
            "date": "2026-05-26 12:17:35",
            "body": "Good afternoon! I am sending the draft agreement.",
            "truncated": false,
            "bodyLength": 49
        },
        {
            "id": 16,
            "subject": "Re: Supply Agreement",
            "from": "manager@example.com",
            "to": "John Smith <john.smith@example.com>",
            "cc": "",
            "date": "2026-05-27 12:17:35",
            "body": "Hello! Thank you, we have received it; we will review it and get back to you with comments.",
            "truncated": false,
            "bodyLength": 91
        }
    ],
    "total": 2,
    "hasMore": false,
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
[`array`](../../data-types.md) | E-mails of the branch on the current page, from oldest to newest. If the branch has no available e-mails, an empty array `[]` is returned ||
|| **result[]**
[`object`](../../data-types.md) | E-mail object from the branch ||
|| **id**
[`integer`](../../data-types.md) | E-mail identifier ||
|| **subject**
[`string`](../../data-types.md) | E-mail subject ||
|| **from**
[`string`](../../data-types.md) | E-mail sender ||
|| **to**
[`string`](../../data-types.md) | E-mail recipient ||
|| **cc**
[`string`](../../data-types.md) | CC recipients ||
|| **date**
[`string`](../../data-types.md) | Date and time the e-mail was received in the `YYYY-MM-DD HH:MM:SS` format, without a time zone ||
|| **body**
[`string`](../../data-types.md) | E-mail text without HTML markup and without the quote of the previous e-mail. Returned if `withBodies` is `true`.

The method takes the first 10,000 characters of the text. If the text is longer, a note is appended at the end, for example, `[truncated: showing first 10000 of 15000 characters]` ||
|| **truncated**
[`boolean`](../../data-types.md) | Indicates that the e-mail text is truncated. Returned if `withBodies` is `true` ||
|| **bodyLength**
[`integer`](../../data-types.md) | Length of the original e-mail text in characters. Returned if `withBodies` is `true` ||
|| **total**
[`integer`](../../data-types.md) | Number of e-mails in the branch available to the user ||
|| **hasMore**
[`boolean`](../../data-types.md) | Whether there are older e-mails: `true` means there are; request the next page with an increased `offset` ||
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

#### Access Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `-` | Access denied | Make sure the application or webhook has the `mail` scope ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-get.md)
- [{#T}](./mail-message-list.md)
