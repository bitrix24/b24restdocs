# Remove the link between an e-mail and a CRM deal mail.message.removecrmactivity

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the mailbox where the e-mail is located

The `mail.message.removecrmactivity` method removes the link between an e-mail and a CRM deal.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **messageId***
[`integer`](../../../data-types.md) | E-mail identifier. 

The identifier can be obtained using the [mail.message.list](./mail-message-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter to the request:

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

- JS

    The SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'mail.message.removecrmactivity',
            {
                messageId: 15
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

    The SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

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

    The SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

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

    The SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

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

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
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
[`boolean`](../../../data-types.md) | `true`, if the link between the e-mail and the CRM task is deleted ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Request object validation error",
        "validation": [
            {
                "message": "Required field is missing",
                "field": "messageId"
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
|| `messageId` | Required field is not specified | Pass `messageId` in the request body ||
|| `messageId` | Access denied: no access to CRM | Check the CRM access permissions for the current user ||
|| `messageId` | Access denied: cannot delete CRM activity | Check the permissions to delete CRM tasks ||
|| `messageId` | Message is deleted or moved to another folder | Verify that the e-mail exists and is accessible to the current user ||
|#

#### Access Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `-` | Access denied | Check user permissions and the `mail` scope ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-createcrmactivity.md)
- [{#T}](./index.md)

<!-- Generated by skill-1-gen-docs from source: C:\Users\g.m.tagirova\original-bitrix\modules\mail\lib\Infrastructure\Rest\Controller\Message.php -->
<!-- Generated: 2026-05-26 -->
<!-- Requires verification: no -->