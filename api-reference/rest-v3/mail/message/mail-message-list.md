# Get a list of e-mails mail.message.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `mail.message.list` method returns a list of e-mails based on specified conditions.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **mailboxId**
[`integer`](../../../data-types.md) | Mailbox identifier.

The identifier can be obtained using the [mail.mailbox.list](../mailbox/mail-mailbox-list.md) ||
|| **searchQuery**
[`string`](../../../data-types.md) | A string for searching e-mails by content and e-mail metadata ||
|| **dateFrom**
[`string`](../../../data-types.md) | The start of the e-mail selection period in ISO 8601 format, for example `2026-01-01T00:00:00+03:00` ||
|| **dateTo**
[`string`](../../../data-types.md) | The end of the e-mail selection period in ISO 8601 format, for example `2026-01-31T23:59:59+03:00` ||
|| **isSeen**
[`boolean`](../../../data-types.md) | Filter by e-mail read status.

Possible values:

- `true` — read only
- `false` — unread only ||
|| **hasAttachments**
[`boolean`](../../../data-types.md) | Filter by attachment presence.

Possible values:

- `true` — e-mails with attachments only
- `false` — e-mails without attachments only ||
|| **folder**
[`string`](../../../data-types.md) | Mail folder name or path ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters:
- `page` — page number
- `limit` — number of records per page, default `25`, maximum `200`
- `offset` — record offset. If `page` and `limit` are provided, the offset is calculated automatically ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter to the request:

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

- JS

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl, fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
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
            }
        );

        const result = response.getData().result;
        console.log('Message list:', result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl, fetch.

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

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl, fetch.

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

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl, fetch.

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
                "cc": "",
                "date": "2026-05-25T10:00:00+03:00",
                "isSeen": true,
                "hasAttachments": true,
                "url": "https://example.bitrix24.com/mail/message/15/",
                "bindings": [],
                "body": "Letter text"
            }
        ]
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
|| **items**
[`array`](../../../data-types.md) | Array of e-mail objects ||
|| **items[]**
[`object`](../../../data-types.md) | E-mail object ||
|| **id**
[`integer`](../../../data-types.md) | E-mail identifier ||
|| **mailboxId**
[`integer`](../../../data-types.md) | Mailbox identifier ||
|| **mailboxEmail**
[`string`](../../../data-types.md) | Mailbox e-mail address ||
|| **subject**
[`string`](../../../data-types.md) | E-mail subject ||
|| **from**
[`string`](../../../data-types.md) | E-mail sender ||
|| **to**
[`string`](../../../data-types.md) | E-mail recipients ||
|| **cc**
[`string`](../../../data-types.md) | E-mail CC ||
|| **date**
[`string`](../../../data-types.md) | E-mail date and time ||
|| **isSeen**
[`boolean`](../../../data-types.md) | E-mail read status ||
|| **hasAttachments**
[`boolean`](../../../data-types.md) | Attachment presence indicator ||
|| **url**
[`string`](../../../data-types.md) | Link to the e-mail ||
|| **bindings**
[`array`](../../../data-types.md) | E-mail relations to objects ||
|| **body**
[`string`](../../../data-types.md) | E-mail body ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION",
        "message": "Unable to recognize pagination parameter `{\"limit\":\"abc\"}`"
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `-` | Access denied | Check user permissions and scope `mail` ||
|#

#### Pagination Errors
Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `limit`
`offset`
`page` | Unable to recognize pagination parameter `#PAGE#` | Provide numeric values. `limit` must not be equal to `0` ||
|#

#### Request Validation Errors
Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `dateFrom` | Parameter `dateFrom` must be in ISO 8601 format (DATE_ATOM), for example `2026-01-01T00:00:00+00:00` | Provide the date in ISO 8601 format ||
|| `dateTo` | Parameter `dateTo` must be in ISO 8601 format (DATE_ATOM), for example `2026-01-01T00:00:00+00:00` | Provide the date in ISO 8601 format ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-get.md)
- [{#T}](./mail-message-field-list.md)
- [{#T}](./index.md)