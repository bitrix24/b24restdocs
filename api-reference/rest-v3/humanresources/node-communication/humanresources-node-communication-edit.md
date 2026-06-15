# Edit Communications of the humanresources.node.communication.edit

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit Departments" or "Edit Teams" permission

The method `humanresources.node.communication.edit` binds, unbinds, or creates a chat, channel, or collab for a department or team.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **nodeId***
[`integer`](../../../data-types.md) | Identifier of the department or team.

The identifier can be obtained using the [humanresources.node.list](../node/humanresources-node-list.md) method. ||
|| **communicationType***
[`string`](../../../data-types.md) | Type of communication.

Possible values:

- `CHAT` — chat
- `CHANNEL` — channel
- `COLLAB` — collab ||
|| **createDefault**
[`boolean`](../../../data-types.md) | Creates a default communication for the department or team.

Possible values:

- `true` — create default communication
- `false` — do not create default communication

Default is `false` ||
|| **ids**
[`array`](../../../data-types.md) | Identifiers of communications to be bound.

Identifiers of chats and channels can be obtained using the [im.recent.list](../../../chats/im-recent-list.md) method, and identifiers of collabs can be obtained using the [socialnetwork.api.workgroup.list](../../../sonet-group/socialnetwork-api-workgroup-list.md) method. ||
|| **removeIds**
[`array`](../../../data-types.md) | Identifiers of communications to be unbound.

Identifiers of related communications can be obtained using the [humanresources.node.communication.list](./humanresources-node-communication-list.md) method. ||
|| **withChildren**
[`boolean`](../../../data-types.md) | Applies the change to child departments and teams.

Possible values:

- `true` — apply the change to child departments and teams
- `false` — apply the change only to the department or team specified in the `nodeId` parameter

Default is `false` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The call to the new API differs by adding the `/api/` parameter in the request:

```plaintext
https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.communication.edit
```

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"nodeId":15,"communicationType":"CHAT","ids":[21],"removeIds":[18],"createDefault":false,"withChildren":false}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.communication.edit
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"nodeId":15,"communicationType":"CHAT","ids":[21],"removeIds":[18],"createDefault":false,"withChildren":false,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.communication.edit
    ```


- JS

    SDKs do not currently support calls to the `/rest/api/` address. Use direct HTTP requests, for example, with curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'humanresources.node.communication.edit',
            {
                nodeId: 15,
                communicationType: 'CHAT',
                ids: [21],
                removeIds: [18],
                createDefault: false,
                withChildren: false
            }
        );

        const result = response.getData().result;
        console.log(result.success);
    }
    catch (error)
    {
        console.error('Error:', error);
    }
    ```

- PHP

    SDKs do not currently support calls to the `/rest/api/` address. Use direct HTTP requests, for example, with curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.communication.edit',
                [
                    'nodeId' => 15,
                    'communicationType' => 'CHAT',
                    'ids' => [21],
                    'removeIds' => [18],
                    'createDefault' => false,
                    'withChildren' => false,
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

    SDKs do not currently support calls to the `/rest/api/` address. Use direct HTTP requests, for example, with curl or fetch.

    ```js
    BX24.callMethod(
        'humanresources.node.communication.edit',
        {
            nodeId: 15,
            communicationType: 'CHAT',
            ids: [21],
            removeIds: [18],
            createDefault: false,
            withChildren: false
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not currently support calls to the `/rest/api/` address. Use direct HTTP requests, for example, with curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'humanresources.node.communication.edit',
        [
            'nodeId' => 15,
            'communicationType' => 'CHAT',
            'ids' => [21],
            'removeIds' => [18],
            'createDefault' => false,
            'withChildren' => false,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "success": true
    },
    "time": {
        "start": 1780407100,
        "finish": 1780407100.186404,
        "duration": 0.18640398979187012,
        "processing": 0.1328411102294922,
        "date_start": "2026-06-02T16:31:40+02:00",
        "date_finish": "2026-06-02T16:31:40+02:00",
        "operating_reset_at": 1780407700,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the result of the operation ||
|| **success**
[`boolean`](../../../data-types.md) | Value `true` if communications were changed ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "message": "Parameter \"communicationType\" is required.",
                "field": "communicationType"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `nodeId` | Parameter `"nodeId"` is required. | Provide the identifier of the department or team. ||
|| `communicationType` | Parameter `"communicationType"` is required. | Provide the type of communication. ||
|| `communicationType` | Invalid `"communicationType"` value. Allowed: `CHAT`, `CHANNEL`, `COLLAB`. | Provide one of the allowed values. ||
|#

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `nodeId` | Object with identifier `#ID#` not found. | Specify an existing identifier of the department or team. ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied. | Check the permission to modify communications of the department or team. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-node-communication-list.md)
- [{#T}](./index.md)
