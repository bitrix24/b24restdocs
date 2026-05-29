# Reply to e-mail mail.message.reply

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the mailbox where the e-mail is located

The `mail.message.reply` method sends a reply to an e-mail.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **replyToMessageId***
[`integer`](../../../data-types.md) | The e-mail identifier that needs to be replied to.

The identifier can be obtained using the [mail.message.list](./mail-message-list.md) method. ||
|| **from***
[`string`](../../../data-types.md) | Sender's e-mail.

Available addresses can be obtained using the [mail.mailbox.senders](../mailbox/mail-mailbox-senders.md) method. ||
|| **to***
[`array`](../../../data-types.md) | An array of recipient e-mail addresses ||
|| **subject***
[`string`](../../../data-types.md) | E-mail subject ||
|| **body***
[`string`](../../../data-types.md) | E-mail text: plain text or basic HTML ||
|| **cc**
[`array`](../../../data-types.md) | An array of CC recipient e-mail addresses ||
|| **bcc**
[`array`](../../../data-types.md) | An array of BCC recipient e-mail addresses ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter to the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/mail.message.reply`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"replyToMessageId":15,"from":"user@example.com","to":["client@example.com"],"subject":"Re: Agreement","body":"Thank you, received."}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/mail.message.reply
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"replyToMessageId":15,"from":"user@example.com","to":["client@example.com"],"subject":"Re: Agreement","body":"Thank you, received.","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/mail.message.reply
    ```

- JS

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl, fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'mail.message.reply',
            {
                replyToMessageId: 15,
                from: 'user@example.com',
                to: ['client@example.com'],
                subject: 'Re: Agreement',
                body: 'Thank you, received.'
            }
        );

        const result = response.getData().result;
        console.log('Success:', result);
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
                'mail.message.reply',
                [
                    'replyToMessageId' => 15,
                    'from' => 'user@example.com',
                    'to' => ['client@example.com'],
                    'subject' => 'Re: Agreement',
                    'body' => 'Thank you, received.'
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
        'mail.message.reply',
        {
            replyToMessageId: 15,
            from: 'user@example.com',
            to: ['client@example.com'],
            subject: 'Re: Agreement',
            body: 'Thank you, received.'
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
        'mail.message.reply',
        [
            'replyToMessageId' => 15,
            'from' => 'user@example.com',
            'to' => ['client@example.com'],
            'subject' => 'Re: Agreement',
            'body' => 'Thank you, received.'
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
        "success": true,
        "to": [
            "client@example.com"
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
[`object`](../../../data-types.md) | Object with the sending result ||
|| **success**
[`boolean`](../../../data-types.md) | Success flag ||
|| **to**
[`array`](../../../data-types.md) | Recipients to whom the e-mail was sent ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "message": "Mandatory field is missing",
                "field": "replyToMessageId"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors
Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `replyToMessageId` | Mandatory field is not specified | Pass `replyToMessageId` in the request body ||
|| `from` | Mandatory field `from` is not specified | Pass `from` in the request body ||
|| `to` | Mandatory field `to` is not specified | Pass `to` in the request body ||
|| `subject` | Mandatory field `subject` is not specified | Pass `subject` in the request body ||
|| `body` | Mandatory field `body` is not specified | Pass `body` in the request body ||
|| `replyToMessageId` | MESSAGE_REPLY_FAILED | Check the existence of the e-mail, access to it, and the correctness of the e-mail data ||
|| `to` | NO_RECIPIENTS | Check that the `to` array contains correct recipient addresses ||
|| `to`, `cc`, `bcc` | RESOLVE_RECIPIENTS_ERROR | Check the value format and recipient availability in `to`, `cc`, `bcc` ||
|#

#### Access Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `-` | Access denied | Check user permissions and `mail` scope ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-forward.md)
- [{#T}](./mail-message-send.md)
- [{#T}](./mail-message-get.md)

<!-- Generated by skill-1-gen-docs from source: C:\Users\g.m.tagirova\original-bitrix\modules\mail\lib\Infrastructure\Rest\Controller\Message.php -->
<!-- Generated: 2026-05-26 -->
<!-- Requires verification: no -->