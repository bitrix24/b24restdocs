# Get a list of e-mails mail.message.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`mail`](../../scopes/permissions.md)
>
> Who can execute the method: any user. The method searches for e-mails only in mailboxes the user has access to

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `mail.message.list` method returns a list of e-mails based on specified conditions. E-mails are sorted from newest to oldest.

Search conditions are passed as separate parameters at the root of the request. The method does not support the standard REST 3.0 parameters `filter` and `order`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **mailboxId**
[`integer`](../../data-types.md) | Mailbox identifier. A positive integer.

If the parameter is not passed, the method searches for e-mails in all of the user's mailboxes.

The identifier can be obtained using the [mail.mailbox.list](../mailbox/mail-mailbox-list.md) ||
|| **searchQuery**
[`string`](../../data-types.md) | A string for searching e-mails by content and e-mail metadata. An empty string is ignored ||
|| **dateFrom**
[`string`](../../data-types.md) | The start of the e-mail selection period. Possible formats:
- a date `YYYY-MM-DD`, for example, `2026-01-01`. The method uses the start of the day, `00:00:00`
- a date and time with a time zone, for example, `2026-01-01T10:00:00+01:00`, `2026-01-01T10:00:00Z`, or `2026-01-01T10:00:00.123+01:00`

A date and time without a time zone, for example, `2026-01-01T10:00:00`, causes an error. The `dateFrom` value must not be later than `dateTo` ||
|| **dateTo**
[`string`](../../data-types.md) | The end of the e-mail selection period. The formats are the same as for `dateFrom`. If only a date is passed, for example, `2026-01-31`, the method uses the end of the day, `23:59:59` ||
|| **isSeen**
[`boolean`](../../data-types.md) | Filter by e-mail read status.

Possible values:

- `true` — read only
- `false` — unread only ||
|| **hasAttachments**
[`boolean`](../../data-types.md) | Filter by attachment presence.

Possible values:

- `true` — e-mails with attachments only
- `false` — e-mails without attachments only ||
|| **folder**
[`string`](../../data-types.md) | Mail folder name or path, for example, `INBOX` ||
|| **select**
[`array`](../../data-types.md) | List of e-mail fields. The method checks the field names and returns an error for an unknown field, but currently the response always contains all e-mail fields.

Available fields: `id`, `mailboxId`, `mailboxEmail`, `subject`, `from`, `to`, `cc`, `date`, `isSeen`, `hasAttachments`, `url`, `bindings`, `body`. The fields are described in the [Returned Data](#returned-data) section ||
|| **pagination**
[`object`](../../data-types.md) | Pagination parameters:
- `page` — page number
- `limit` — number of records per page, default `25`. The method reduces a value greater than `200` to `200`; the value `0` causes an error
- `offset` — record offset, a non-negative number. If `page` and `limit` are provided, the offset is calculated automatically ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/mail.message.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"mailboxId":1,"searchQuery":"contract","hasAttachments":true,"pagination":{"page":1,"limit":20,"offset":0}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/mail.message.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"mailboxId":1,"searchQuery":"contract","hasAttachments":true,"pagination":{"page":1,"limit":20,"offset":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/mail.message.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MailMessageListResult = {
      items: MailMessageItem[]
    }

    type MailMessageBinding = {
      type: 'crm' | 'task' | 'chat' | 'calendarEvent' | 'blogPost'
      entityTypeId?: number
      entityId: number
    }

    type MailMessageItem = {
      id: number
      mailboxId: number
      mailboxEmail: string
      subject: string
      from: string
      to: string
      cc: string | null
      date: string
      isSeen: boolean
      hasAttachments: boolean
      url: string
      bindings: MailMessageBinding[]
      body: string | null
    }

    try {
      // mail.message.list returns a single page (default 25, max 200 records). For the whole result set
      // use a list helper: $b24.actions.v3.callList.make() returns every record as one
      // array, $b24.actions.v3.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `pagination` variant when sort matters.
      const response = await $b24.actions.v3.call.make<MailMessageListResult>({
        method: 'mail.message.list',
        params: {
          mailboxId: 1,
          searchQuery: 'contract',
          hasAttachments: true,
          pagination: {
            page: 1,
            limit: 20,
            offset: 0,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Messages fetched:', result.items.length, result.items[0])
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
      async function fetchMessageList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // mail.message.list returns a single page (default 25, max 200 records). For the whole result set
          // use a list helper: $b24.actions.v3.callList.make() returns every record as one
          // array, $b24.actions.v3.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `pagination` variant when sort matters.
          const response = await $b24.actions.v3.call.make({
            method: 'mail.message.list',
            params: {
              mailboxId: 1,
              searchQuery: 'contract',
              hasAttachments: true,
              pagination: {
                page: 1,
                limit: 20,
                offset: 0,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Messages fetched:', result.items.length, result.items[0])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchMessageList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    pagination = {
        "page": 1,
        "limit": 20,
        "offset": 0,
    }

    try:
        bitrix_response = client.mail.message.list(
            mailbox_id=1,
            search_query='contract',
            has_attachments=True,
            pagination=pagination,
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
                'mail.message.list',
                [
                    'mailboxId' => 1,
                    'searchQuery' => 'contract',
                    'hasAttachments' => true,
                    'pagination' => [
                        'page' => 1,
                        'limit' => 20,
                        'offset' => 0
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
        'mail.message.list',
        {
            mailboxId: 1,
            searchQuery: 'contract',
            hasAttachments: true,
            pagination: {
                page: 1,
                limit: 20,
                offset: 0
            }
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
        'mail.message.list',
        [
            'mailboxId' => 1,
            'searchQuery' => 'contract',
            'hasAttachments' => true,
            'pagination' => [
                'page' => 1,
                'limit' => 20,
                'offset' => 0
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
    res, err := client.Core().Call(ctx, "mail.message.list", b24.Params{
    	"mailboxId":      1,
    	"searchQuery":    "contract",
    	"hasAttachments": true,
    	"pagination": b24.Params{
    		"page":   1,
    		"limit":  20,
    		"offset": 0,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("mail.message.list: %w", err)
    }

    // The method wraps the response in an object with the "items" key.
    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("no items key in the response")
    }

    var items []struct {
    	ID           b24.ID `json:"id"`
    	MailboxID    b24.ID `json:"mailboxId"`
    	MailboxEmail string `json:"mailboxEmail"`
    	Subject      string `json:"subject"`
    	From         string `json:"from"`
    	To           string `json:"to"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 15,
                "mailboxId": 1,
                "mailboxEmail": "user@example.com",
                "subject": "Contract",
                "from": "client@example.com",
                "to": "user@example.com",
                "cc": null,
                "date": "2026-05-25 10:00:00",
                "isSeen": true,
                "hasAttachments": true,
                "url": "https://example.bitrix24.com/mail/message/15",
                "bindings": [
                    {
                        "type": "crm",
                        "entityTypeId": 3,
                        "entityId": 27
                    }
                ],
                "body": null
            }
        ]
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

### Returned Data {#returned-data}

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Response data object ||
|| **items**
[`array`](../../data-types.md) | Array of e-mail objects. If there are no e-mails, an empty array `[]` is returned ||
|| **items[]**
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
[`string`](../../data-types.md) | CC recipients. Not filled in the list and returned as `null`; retrieve it using the [mail.message.get](./mail-message-get.md) method ||
|| **date**
[`string`](../../data-types.md) | Date and time the e-mail was received in the `YYYY-MM-DD HH:MM:SS` format, without a time zone ||
|| **isSeen**
[`boolean`](../../data-types.md) | Read flag: `true` — read, `false` — unread ||
|| **hasAttachments**
[`boolean`](../../data-types.md) | Attachment presence indicator ||
|| **url**
[`string`](../../data-types.md) | Full link to the e-mail in Bitrix24 ||
|| **bindings**
[`array`](../../data-types.md) | Links of the e-mail to Bitrix24 objects. If there are none, an empty array `[]` is returned. Each item contains:
- `type` — object type: `crm` — CRM activity, `task` — task, `chat` — chat, `calendarEvent` — calendar event, `blogPost` — Feed post
- `entityTypeId` — [CRM object type identifier](../../crm/data-types.md#object_type) of the object the activity is linked to. Present only in a `crm` link
- `entityId` — object identifier. For a `crm` link, the identifier of the CRM object the activity is linked to ||
|| **body**
[`string`](../../data-types.md) | E-mail text. Not filled in the list and returned as `null`; retrieve it using the [mail.message.get](./mail-message-get.md) method ||
|| **time**
[`time`](../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "The selected mailbox is not available to the current user.",
                "field": "MESSAGE_LIST_FAILED"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors
Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

Errors in the search parameters are returned in the `validation` array with the `field` value `MESSAGE_LIST_FAILED`. The error text shows which parameter is invalid.

#|
|| **Field** | **Error description** | **How to fix** ||
|| `MESSAGE_LIST_FAILED` | mailboxId must be a positive integer. | Pass a positive integer in `mailboxId` ||
|| `MESSAGE_LIST_FAILED` | The selected mailbox is not available to the current user. | The mailbox was not found, or the user has no access to it. Retrieve the available mailboxes using the [mail.mailbox.list](../mailbox/mail-mailbox-list.md) method ||
|| `MESSAGE_LIST_FAILED` | Invalid dateFrom. Use an ISO 8601 date like 'YYYY-MM-DD' or a full date-time. | Pass a `YYYY-MM-DD` date or a date and time with a time zone in `dateFrom` ||
|| `MESSAGE_LIST_FAILED` | Invalid dateTo. Use an ISO 8601 date like 'YYYY-MM-DD' or a full date-time. | Pass a `YYYY-MM-DD` date or a date and time with a time zone in `dateTo` ||
|| `MESSAGE_LIST_FAILED` | dateFrom must be earlier than or equal to dateTo. | Pass a `dateFrom` value that is not later than `dateTo` ||
|| `id` | DTO `MessageDto` in field `id` requires attribute `Filterable` to perform this request | The method does not support the `filter` parameter. Pass the search conditions as separate parameters: `mailboxId`, `searchQuery`, `dateFrom`, and others ||
|| `id` | DTO `MessageDto` in field `id` requires attribute `Sortable` to perform this request | The method does not support the `order` parameter. Remove it from the request: e-mails are always sorted from newest to oldest ||
|#

#### Errors in the filter Parameter
Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | The method does not support the `filter` parameter. Pass the search conditions as separate parameters at the root of the request ||
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

#### Pagination Errors
Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":"abc"}` | Pass numbers in `limit`, `page`, and `offset` ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass a `limit` greater than `0` ||
|| `pagination` | Incorrect `pagination` parameter value: `{"offset":-5}` | Pass an `offset` greater than or equal to `0` ||
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
- [{#T}](./mail-message-field-list.md)
- [{#T}](./index.md)
