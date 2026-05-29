# Get mailbox mail.mailbox.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the mailbox

The `mail.mailbox.get` method retrieves a mailbox by its identifier.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Mailbox identifier.

The identifier can be obtained using the [mail.mailbox.list](./mail-mailbox-list.md) method. ||
|| **select**
[`array`](../../../data-types.md) | A list of mailbox fields to be returned.

Available fields:
- `id` — mailbox identifier
- `name` — mailbox name
- `email` — email address
- `senderName` — sender name ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter to the request:

`https://{install_address}/rest/api/{user_id}/{webhook_token}/mail.mailbox.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"select":["id","name","email","senderName"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/mail.mailbox.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"select":["id","name","email","senderName"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/mail.mailbox.get
    ```

- JS

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'mail.mailbox.get',
            {
                id: 1,
                select: [
                    'id',
                    'name',
                    'email',
                    'senderName'
                ]
            }
        );

        const result = response.getData().result;
        console.log('Mailbox item:', result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'mail.mailbox.get',
                [
                    'id' => 1,
                    'select' => [
                        'id',
                        'name',
                        'email',
                        'senderName'
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

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'mail.mailbox.get',
        {
            id: 1,
            select: [
                'id',
                'name',
                'email',
                'senderName'
            ]
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'mail.mailbox.get',
        [
            'id' => 1,
            'select' => [
                'id',
                'name',
                'email',
                'senderName'
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
            "id": 1,
            "name": "Work email",
            "email": "user@example.com",
            "senderName": "John Smith"
        }
    },
    "time": {
        "start": 1779819427,
        "finish": 1779819427.356922,
        "duration": 0.356921911239624,
        "processing": 0,
        "date_start": "2026-05-26T21:17:07+03:00",
        "date_finish": "2026-05-26T21:17:07+03:00",
        "operating_reset_at": 1779820027,
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
[`object`](../../../data-types.md) | Mailbox object. The response structure depends on `select` ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Record with ID = `1` not found"
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

- [{#T}](./mail-mailbox-list.md)
- [{#T}](./mail-mailbox-field-list.md)
- [{#T}](./index.md)