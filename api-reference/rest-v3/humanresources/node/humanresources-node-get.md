# Get humanresources.node.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "View Departments" or "View Teams" access permission.

The method `humanresources.node.get` returns a department or team by its identifier.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md) | Identifier of the department or team.

The identifier can be obtained using the [humanresources.node.list](./humanresources-node-list.md) method. ||
|| **select** 
[`array`](../../../data-types.md) | List of department fields to return.

Available fields:

- `id` — department identifier
- `name` — department name
- `type` — type of the structure element
- `structureId` — company structure identifier
- `parentId` — parent department identifier
- `description` — department description
- `accessCode` — department access code
- `userCount` — number of users in the department
- `colorName` — team color
- `xmlId` — external department identifier
- `createdAt` — creation date and time
- `updatedAt` — last update date and time
- `members` — list of department users ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"select":["id","name","type","structureId","parentId","description","accessCode","userCount","colorName","xmlId","createdAt","updatedAt","members"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"select":["id","name","type","structureId","parentId","description","accessCode","userCount","colorName","xmlId","createdAt","updatedAt","members"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type NodeGetResult = {
      item: {
        id: number
        name: string
        type: string
        structureId: number
        parentId: number | null
        description: string
        accessCode: string
        userCount: number
        colorName: string | null
        xmlId: string | null
        createdAt: ISODate
        updatedAt: ISODate
        members: Array<{
          userId: number
          name: string
          workPosition: string
          role: string
          avatar: string
          url: string
        }>
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<NodeGetResult>({
        method: 'humanresources.node.get',
        params: {
          id: 1,
          select: [
            'id',
            'name',
            'type',
            'structureId',
            'parentId',
            'description',
            'accessCode',
            'userCount',
            'colorName',
            'xmlId',
            'createdAt',
            'updatedAt',
            'members',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.id, result.item.name, result.item.type)
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
      async function getNode() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'humanresources.node.get',
            params: {
              id: 1,
              select: [
                'id',
                'name',
                'type',
                'structureId',
                'parentId',
                'description',
                'accessCode',
                'userCount',
                'colorName',
                'xmlId',
                'createdAt',
                'updatedAt',
                'members',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item.id, result.item.name, result.item.type)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getNode)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.get',
                [
                    'id' => 1,
                    'select' => [
                        'id',
                        'name',
                        'type',
                        'structureId',
                        'parentId',
                        'description',
                        'accessCode',
                        'userCount',
                        'colorName',
                        'xmlId',
                        'createdAt',
                        'updatedAt',
                        'members'
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'humanresources.node.get',
        {
            id: 1,
            select: [
                'id',
                'name',
                'type',
                'structureId',
                'parentId',
                'description',
                'accessCode',
                'userCount',
                'colorName',
                'xmlId',
                'createdAt',
                'updatedAt',
                'members'
            ]
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
        'humanresources.node.get',
        [
            'id' => 1,
            'select' => [
                'id',
                'name',
                'type',
                'structureId',
                'parentId',
                'description',
                'accessCode',
                'userCount',
                'colorName',
                'xmlId',
                'createdAt',
                'updatedAt',
                'members'
            ]
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
        "item": {
            "id": 1,
            "name": "Sales Department",
            "type": "DEPARTMENT",
            "structureId": 1,
            "parentId": null,
            "description": "Main sales department",
            "accessCode": "DR1",
            "userCount": 18,
            "colorName": null,
            "xmlId": null,
            "createdAt": "2026-05-20T10:15:00+02:00",
            "updatedAt": "2026-06-02T10:30:00+02:00",
            "members": [
                {
                    "userId": 7,
                    "name": "Anna Smith",
                    "workPosition": "Sales Department Head",
                    "role": "MEMBER_HEAD",
                    "avatar": "https://example.bitrix24.com/upload/main/1/avatar.jpg",
                    "url": "/company/personal/user/7/"
                }
            ]
        }
    },
    "time": {
        "start": 1780403400,
        "finish": 1780403400.213411,
        "duration": 0.2134110927581787,
        "processing": 0.1820049285888672,
        "date_start": "2026-06-02T15:30:00+02:00",
        "date_finish": "2026-06-02T15:30:00+02:00",
        "operating_reset_at": 1780404000,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result** 
[`object`](../../../data-types.md) | Object containing the response data ||
|| **item** 
[`object`](../../../data-types.md) | Object with department or team data. The structure of the response depends on `select` ||
|| **time** 
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Record with ID = `1` not found"
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Required field `id` is not specified | Add `id` to the request body ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | The user does not have permission to view the specified department or team ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Insufficient access rights: required scope is missing | Check the `humanresources` scope ||
|#

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Record with the specified identifier not found | Specify the identifier of an existing department or team ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` for entity `NodeDto` | Only pass fields from the `select` list ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unable to recognize select expression `#SELECT#` | Pass `select` as an array of strings, e.g., `["id","name"]` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-node-list.md)
- [{#T}](./humanresources-node-field-list.md)
- [{#T}](./index.md)
