# Get a List of Departments humanresources.node.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "View Departments" or "View Teams" access permission.

The method `humanresources.node.list` returns a list of departments or teams.

## Method Parameters

{% include [Note on Parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **type*** 
[`string`](../../../data-types.md) | Type of the structure element.

Possible values:

- `DEPARTMENT` — department
- `TEAM` — team ||
|| **select**
[`array`](../../../data-types.md) | List of fields for the department or team to return.

Available fields:

- `id` — identifier of the structure element
- `name` — name of the department or team
- `type` — type of the structure element
- `structureId` — identifier of the company structure
- `parentId` — identifier of the parent department or team
- `description` — description of the structure element
- `accessCode` — access code of the structure element
- `userCount` — number of users in the department or team
- `colorName` — color of the team
- `xmlId` — external identifier of the structure element
- `createdAt` — creation date and time
- `updatedAt` — last update date and time ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters:
- `page` — page number
- `limit` — number of records per page, default is `50`, maximum is `200`
- `offset` — record offset ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"DEPARTMENT","select":["id","name","type","structureId","parentId","description","accessCode","userCount","colorName","xmlId","createdAt","updatedAt"],"pagination":{"page":1,"limit":20,"offset":0}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"DEPARTMENT","select":["id","name","type","structureId","parentId","description","accessCode","userCount","colorName","xmlId","createdAt","updatedAt"],"pagination":{"page":1,"limit":20,"offset":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type NodeItem = {
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
      createdAt: ISODate | null
      updatedAt: ISODate | null
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type NodeListResult = {
      items: NodeItem[]
    }

    try {
      // humanresources.node.list returns a single page (max 200 records). For the whole result set
      // use a list helper: $b24.actions.v3.callList.make() returns every record as one
      // array, $b24.actions.v3.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + pagination variant when sort matters.
      const response = await $b24.actions.v3.call.make<NodeListResult>({
        method: 'humanresources.node.list',
        params: {
          type: 'DEPARTMENT',
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
          ],
          pagination: {
            page: 1,
            limit: 20,
            offset: 0,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Nodes retrieved:', result.items.length, result.items)
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
      async function listNodes() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // humanresources.node.list returns a single page (max 200 records). For the whole result set
          // use a list helper: $b24.actions.v3.callList.make() returns every record as one
          // array, $b24.actions.v3.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + pagination variant when sort matters.
          const response = await $b24.actions.v3.call.make({
            method: 'humanresources.node.list',
            params: {
              type: 'DEPARTMENT',
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
              ],
              pagination: {
                page: 1,
                limit: 20,
                offset: 0,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Nodes retrieved:', result.items.length, result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listNodes)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.list',
                [
                    'type' => 'DEPARTMENT',
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
                        'updatedAt'
                    ],
                    'pagination' => [
                        'page' => 1,
                        'limit' => 20,
                        'offset' => 0
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
        'humanresources.node.list',
        {
            type: 'DEPARTMENT',
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
                'updatedAt'
            ],
            pagination: {
                page: 1,
                limit: 20,
                offset: 0
            }
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
        'humanresources.node.list',
        [
            'type' => 'DEPARTMENT',
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
                'updatedAt'
            ],
            'pagination' => [
                'page' => 1,
                'limit' => 20,
                'offset' => 0
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
            "items": [
                {
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
                    "updatedAt": "2026-06-02T10:30:00+02:00"
                },
                {
                    "id": 2,
                    "name": "Marketing Department",
                    "type": "DEPARTMENT",
                    "structureId": 1,
                    "parentId": 1,
                    "description": "Department responsible for promoting the company's products",
                    "accessCode": "DR2",
                    "userCount": 9,
                    "colorName": null,
                    "xmlId": "marketing_department",
                    "createdAt": "2026-05-22T09:00:00+02:00",
                    "updatedAt": "2026-06-02T11:45:00+02:00"
                }
            ]
        },
    "time": {
        "start": 1780403500,
        "finish": 1780403500.248911,
        "duration": 0.24891114234924316,
        "processing": 0.21900415420532227,
        "date_start": "2026-06-02T15:31:40+02:00",
        "date_finish": "2026-06-02T15:31:40+02:00",
        "operating_reset_at": 1780404100,
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
|| **items**
[`array`](../../../data-types.md) | Array of department or team objects. The composition of the element fields depends on `select` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION",
        "message": "Cannot recognize pagination parameter `{\"limit\":\"abc\"}`"
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Pagination Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `limit`
`offset`
`page` | Cannot recognize pagination parameter `#PAGE#` | Provide numeric values. `limit` must not be `0` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `type` | Required field `type` is not specified | Provide `type` with the value `DEPARTMENT` or `TEAM` ||
|| `type` | An invalid value for the structure element type was provided | Use `DEPARTMENT` for department or `TEAM` for team ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` for entity `NodeDto` | Provide only fields from the `select` list ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Cannot recognize select expression `#SELECT#` | Provide `select` as an array of strings, e.g., `["id","name"]` ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | The user does not have permission to view departments and teams ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Insufficient access rights: required scope is missing | Check the `humanresources` scope ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-node-get.md)
- [{#T}](./humanresources-node-search.md)
- [{#T}](./index.md)
