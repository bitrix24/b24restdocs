# Get Communications of the humanresources.node.communication.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Departments" or "View Teams" access permission.

The method `humanresources.node.communication.list` returns chats, channels, and collabs associated with a department or team.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the department or team.

The identifier can be obtained using the [humanresources.node.list](../node/humanresources-node-list.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The call to the new API differs by adding the `/api/` parameter in the request:

```plaintext
https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.communication.list
```

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.communication.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.communication.list
    ```


- JS

    SDKs do not currently support calls to the `/rest/api/` address. Use direct HTTP requests, for example, with curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'humanresources.node.communication.list',
            {
                id: 15
            }
        );

        const result = response.getData().result;
        console.log(result);
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
                'humanresources.node.communication.list',
                [
                    'id' => 15,
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
        'humanresources.node.communication.list',
        {
            id: 15
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
        'humanresources.node.communication.list',
        [
            'id' => 15,
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
        "channels": [
            {
                "avatar": "",
                "color": "#8474c8",
                "dialogId": "chat21",
                "hasAccess": true,
                "id": 21,
                "isExtranet": false,
                "originalNodeId": null,
                "subtitle": "Private channel",
                "title": "Department channel",
                "type": "CHANNEL"
            }
        ],
        "channelsNoAccess": 0,
        "chats": [
            {
                "avatar": "",
                "color": "#1eb4aa",
                "dialogId": "chat22",
                "hasAccess": true,
                "id": 22,
                "isExtranet": false,
                "originalNodeId": null,
                "subtitle": "Private chat",
                "title": "Department chat",
                "type": "CHAT"
            }
        ],
        "chatsNoAccess": 0,
        "collabs": [
            {
                "avatar": null,
                "dialogId": "chat23",
                "hasAccess": true,
                "id": 23,
                "originalNodeId": null,
                "subtitle": "Collab",
                "title": "Department collab",
                "type": "COLLAB"
            }
        ],
        "collabsNoAccess": 0
    },
    "time": {
        "start": 1780407000,
        "finish": 1780407000.104211,
        "duration": 0.10421109199523926,
        "processing": 0.08111310005187988,
        "date_start": "2026-06-02T16:30:00+02:00",
        "date_finish": "2026-06-02T16:30:00+02:00",
        "operating_reset_at": 1780407600,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **chats[]**
[`array`](../../../data-types.md) | Array of chats associated with the department or team. [Description of communication fields](#communication) ||
|| **chatsNoAccess**
[`integer`](../../../data-types.md) | Number of associated chats not accessible to the current user ||
|| **channels[]**
[`array`](../../../data-types.md) | Array of channels associated with the department or team. [Description of communication fields](#communication) ||
|| **channelsNoAccess**
[`integer`](../../../data-types.md) | Number of associated channels not accessible to the current user ||
|| **collabs[]**
[`array`](../../../data-types.md) | Array of collabs associated with the department or team. [Description of communication fields](#communication) ||
|| **collabsNoAccess**
[`integer`](../../../data-types.md) | Number of associated collabs not accessible to the current user ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

### Communication Fields {#communication}

#|
|| **Name**
`type` | **Description** ||
|| **avatar**
[`string`](../../../data-types.md) | Link to the communication avatar ||
|| **color**
[`string`](../../../data-types.md) | Color of the communication. This field is returned for chats and channels ||
|| **dialogId**
[`string`](../../../data-types.md) | Identifier of the dialog ||
|| **hasAccess**
[`boolean`](../../../data-types.md) | Indicator of the current user's access to the communication ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the communication ||
|| **isExtranet**
[`boolean`](../../../data-types.md) | Indicator of extranet communication. This field is returned for chats and channels ||
|| **originalNodeId**
[`integer`](../../../data-types.md) | Identifier of the original department or team ||
|| **subtitle**
[`string`](../../../data-types.md) | Subtitle of the communication ||
|| **title**
[`string`](../../../data-types.md) | Title of the communication ||
|| **type**
[`string`](../../../data-types.md) | Type of communication.

Possible values:

- `CHAT` — chat
- `CHANNEL` — channel
- `COLLAB` — collab ||
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
                "message": "Required field `id` is missing",
                "field": "id"
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
|| `id` | Required field `id` is missing | Provide the identifier of the department or team ||
|#

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Object with identifier `#ID#` not found | Specify an existing identifier of the department or team ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Check the access permission to view the department or team ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-node-communication-edit.md)
- [{#T}](./index.md)
