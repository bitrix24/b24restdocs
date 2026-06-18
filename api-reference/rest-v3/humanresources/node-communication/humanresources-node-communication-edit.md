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

The new API call differs by adding the `/api/` segment to the request URL:

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


- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type NodeCommunicationEditResult = {
      success: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<NodeCommunicationEditResult>({
        method: 'humanresources.node.communication.edit',
        params: {
          nodeId: 15,
          communicationType: 'CHAT',
          ids: [21],
          removeIds: [18],
          createDefault: false,
          withChildren: false,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Communication edited successfully:', result.success)
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
      async function editNodeCommunication() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'humanresources.node.communication.edit',
            params: {
              nodeId: 15,
              communicationType: 'CHAT',
              ids: [21],
              removeIds: [18],
              createDefault: false,
              withChildren: false,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Communication edited successfully:', result.success)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', editNodeCommunication)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

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
