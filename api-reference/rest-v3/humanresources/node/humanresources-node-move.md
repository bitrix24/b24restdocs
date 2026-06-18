# Move Department humanresources.node.move

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "Edit Departments" or "Edit Teams" access permission.

The method `humanresources.node.move` changes the parent department for a department or team.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md) | Identifier of the department or team to be moved.

The identifier can be obtained using the [humanresources.node.list](./humanresources-node-list.md) method. ||
|| **parentId*** 
[`integer`](../../../data-types.md) | Identifier of the new parent department.

The identifier can be obtained using the [humanresources.node.list](./humanresources-node-list.md) method. ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.move`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":44,"parentId":2}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.move
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":44,"parentId":2,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.move
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type NodeMoveResult = {
      id: number
      name: string
      type: string
      structureId: number
      parentId: number
      description: string
      accessCode: string
      userCount: number
      colorName: string | null
      xmlId: string | null
      createdAt: ISODate | null
      updatedAt: ISODate | null
    }

    try {
      const response = await $b24.actions.v3.call.make<NodeMoveResult>({
        method: 'humanresources.node.move',
        params: {
          id: 44,
          parentId: 2,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Department moved, id:', result.id, 'parentId:', result.parentId)
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
      async function moveNode() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'humanresources.node.move',
            params: {
              id: 44,
              parentId: 2,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Department moved, id:', result.id, 'parentId:', result.parentId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', moveNode)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.move',
                [
                    'id' => 44,
                    'parentId' => 2,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error moving department: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'humanresources.node.move',
        {
            id: 44,
            parentId: 2
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
        'humanresources.node.move',
        [
            'id' => 44,
            'parentId' => 2
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
        "id": 44,
        "name": "B2B Sales Department",
        "type": "DEPARTMENT",
        "structureId": 1,
        "parentId": 2,
        "description": "Working with corporate clients",
        "accessCode": "DR44",
        "userCount": 18,
        "colorName": null,
        "xmlId": null,
        "createdAt": "2026-05-30T09:20:15+02:00",
        "updatedAt": "2026-06-02T14:40:18+02:00"
    },
    "time": {
        "start": 1780400418,
        "finish": 1780400418.402915,
        "duration": 0.40291500091552734,
        "processing": 0.3614809513092041,
        "date_start": "2026-06-02T14:40:18+02:00",
        "date_finish": "2026-06-02T14:40:18+02:00",
        "operating_reset_at": 1780401018,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object containing data of the moved department or team ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the department or team ||
|| **name**
[`string`](../../../data-types.md) | Name of the department or team ||
|| **type**
[`string`](../../../data-types.md) | Type of the structure element ||
|| **structureId**
[`integer`](../../../data-types.md) | Identifier of the company structure ||
|| **parentId**
[`integer`](../../../data-types.md) | Identifier of the new parent department ||
|| **description**
[`string`](../../../data-types.md) | Description of the department or team ||
|| **accessCode**
[`string`](../../../data-types.md) | Access code of the department or team ||
|| **userCount**
[`integer`](../../../data-types.md) | Number of users in the department or team ||
|| **colorName**
[`string`](../../../data-types.md) | Team color, if specified ||
|| **xmlId**
[`string`](../../../data-types.md) | External identifier of the department or team ||
|| **createdAt**
[`datetime`](../../../data-types.md) | Date and time of creation of the department or team ||
|| **updatedAt**
[`datetime`](../../../data-types.md) | Date and time of the last update of the department or team ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating the request object",
        "validation": [
            {
                "message": "Required field `parentId` is not specified",
                "field": "parentId"
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
|| `id`
`parentId` | Required field `#FIELD#` is not specified | Add the specified field to the request body ||
|| `#FIELD#` | Field `#FIELD#` requires data type `#TYPE#` for this request | Ensure the provided value is of the correct type ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | The user does not have permission to move the structure element to the specified parent department ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Insufficient access rights: required scope is missing | Check the `humanresources` scope ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-node-edit.md)
- [{#T}](./humanresources-node-get.md)
- [{#T}](./index.md)
