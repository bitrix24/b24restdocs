# Create a calendar event from an e-mail mail.message.createcalendarevent

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the e-mail box where the e-mail is located

The `mail.message.createcalendarevent` method creates a calendar event from an e-mail.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **messageId***
[`integer`](../../../data-types.md) | E-mail identifier.

The identifier can be obtained using the [mail.message.list](./mail-message-list.md) method ||
|| **dateFrom***
[`string`](../../../data-types.md) | Event start date and time in `Y-m-d H:i:s` format, for example `2026-04-15 10:00:00` ||
|| **dateTo***
[`string`](../../../data-types.md) | Event end date and time in `Y-m-d H:i:s` format, for example `2026-04-15 11:00:00` ||
|| **name**
[`string`](../../../data-types.md) | Event name.

If not provided, the e-mail subject is used ||
|| **description**
[`string`](../../../data-types.md) | Event description ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter to the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/mail.message.createcalendarevent`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"messageId":15,"dateFrom":"2026-04-15 10:00:00","dateTo":"2026-04-15 11:00:00","name":"Meeting regarding the contract"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/mail.message.createcalendarevent
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"messageId":15,"dateFrom":"2026-04-15 10:00:00","dateTo":"2026-04-15 11:00:00","name":"Meeting regarding the contract","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/mail.message.createcalendarevent
    ```

- JS

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl, fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'mail.message.createcalendarevent',
            {
                messageId: 15,
                dateFrom: '2026-04-15 10:00:00',
                dateTo: '2026-04-15 11:00:00',
                name: 'Meeting regarding the contract'
            }
        );

        const result = response.getData().result;
        console.log('Calendar event:', result);
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
                'mail.message.createcalendarevent',
                [
                    'messageId' => 15,
                    'dateFrom' => '2026-04-15 10:00:00',
                    'dateTo' => '2026-04-15 11:00:00',
                    'name' => 'Meeting regarding the contract'
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
        'mail.message.createcalendarevent',
        {
            messageId: 15,
            dateFrom: '2026-04-15 10:00:00',
            dateTo: '2026-04-15 11:00:00',
            name: 'Meeting regarding the contract'
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
        'mail.message.createcalendarevent',
        [
            'messageId' => 15,
            'dateFrom' => '2026-04-15 10:00:00',
            'dateTo' => '2026-04-15 11:00:00',
            'name' => 'Meeting regarding the contract'
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
        "eventId": 31,
        "messageId": 15
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
[`object`](../../../data-types.md) | Object with the operation execution result ||
|| **success**
[`boolean`](../../../data-types.md) | Success flag of the operation ||
|| **eventId**
[`integer`](../../../data-types.md) | Created event identifier ||
|| **messageId**
[`integer`](../../../data-types.md) | Original e-mail identifier ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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
                "message": "Mandatory field `messageId` is not specified",
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
|| `messageId` | Mandatory field `messageId` is not specified or an incorrect value is provided | Provide `messageId` as a positive integer greater than `0` ||
|| `dateFrom` | Mandatory field `dateFrom` is not specified | Provide `dateFrom` in the request body ||
|| `dateTo` | Mandatory field `dateTo` is not specified | Provide `dateTo` in the request body ||
|| `messageId` | Message deleted or moved to another folder | Check that the e-mail exists and is accessible to the current user ||
|| `dateFrom`, `dateTo` | Invalid date format | Provide the date in `Y-m-d H:i:s` format ||
|| `-` | Failed to create calendar event | Check the correctness of parameters and the availability of the calendar module ||
|| `-` | Module calendar is not installed | Install and configure the calendar module ||
|#

#### Access Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `-` | Access denied | Check user permissions and `mail` scope ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-get.md)
- [{#T}](./index.md)