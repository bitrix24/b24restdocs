# Get e-mail thread mail.message.thread

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the mailbox where the e-mail is located

The `mail.message.thread` method returns an e-mail thread by the identifier of a single e-mail.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | The identifier of any e-mail from the thread.

The identifier can be obtained using the [mail.message.list](./mail-message-list.md) method. ||
|| **limit**
[`integer`](../../../data-types.md) | Maximum number of e-mails in the response.

Default — `20`, maximum — `50` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter to the request:

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

- JS

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl, fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'mail.message.thread',
            {
                id: 15,
                limit: 20
            }
        );

        const result = response.getData().result;
        console.log('Thread:', result);
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

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl, fetch.

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

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl, fetch.

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
            "body": "Good afternoon! I am sending the draft agreement."
        },
        {
            "id": 16,
            "subject": "Re: Supply Agreement",
            "from": "manager@example.com",
            "to": "John Smith <john.smith@example.com>",
            "cc": "",
            "date": "2026-05-27 12:17:35",
            "body": "Hello! Thank you, we have received it; we will review it and get back to you with comments."
        }
    ],
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
[`array`](../../../data-types.md) | List of e-mails from the thread ||
|| **result[]**
[`object`](../../../data-types.md) | E-mail object from the thread ||
|| **id**
[`integer`](../../../data-types.md) | E-mail identifier ||
|| **subject**
[`string`](../../../data-types.md) | E-mail subject ||
|| **from**
[`string`](../../../data-types.md) | E-mail sender ||
|| **to**
[`string`](../../../data-types.md) | E-mail recipient ||
|| **cc**
[`string`](../../../data-types.md) | CC recipients ||
|| **date**
[`string`](../../../data-types.md) | E-mail date and time ||
|| **body**
[`string`](../../../data-types.md) | E-mail text ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entry with ID = `15` not found"
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors
Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `id` | Required field `id` is not specified | Add `id` to the request body ||
|#

#### Data Not Found Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `id` | Record with the specified identifier not found | Specify the identifier of an existing object ||
|#

#### Access Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `id` | Access denied | Check user permissions and `mail` scope ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-get.md)
- [{#T}](./mail-message-list.md)