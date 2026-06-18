# Find Departments humanresources.node.search

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Departments" or "View Teams" access permission.

The method `humanresources.node.search` searches for departments or teams by name.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../../../data-types.md) | Type of the structure element.

Possible values:

- `DEPARTMENT` — department
- `TEAM` — team ||
|| **name***
[`string`](../../../data-types.md) | Search string for part of the department or team name ||
|| **parentId**
[`integer`](../../../data-types.md) | Identifier of the parent department to limit the search.

The identifier can be obtained using the [humanresources.node.list](./humanresources-node-list.md) method ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameter.

The method uses `limit` — the number of records per page. Default is `50`, maximum is `200` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.search`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"DEPARTMENT","name":"Sales","parentId":1,"pagination":{"limit":20}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.search
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"DEPARTMENT","name":"Sales","parentId":1,"pagination":{"limit":20},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.search
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type NodeSearchResult = {
      items: NodeItem[]
    }
    type NodeItem = {
      id: number
      name: string
      type: string
      structureId: number
      parentId: number | null
      description: string | null
      accessCode: string
      userCount: number
      colorName: string | null
      xmlId: string | null
      createdAt: ISODate | null
      updatedAt: ISODate | null
    }

    try {
      const response = await $b24.actions.v3.call.make<NodeSearchResult>({
        method: 'humanresources.node.search',
        params: {
          type: 'DEPARTMENT',
          name: 'Sales',
          parentId: 1,
          pagination: {
            limit: 20,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Found departments:', result.items.length, result.items)
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
      async function searchNodes() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'humanresources.node.search',
            params: {
              type: 'DEPARTMENT',
              name: 'Sales',
              parentId: 1,
              pagination: {
                limit: 20,
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
          console.info('Found departments:', result.items.length, result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', searchNodes)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.search',
                [
                    'type' => 'DEPARTMENT',
                    'name' => 'Sales',
                    'parentId' => 1,
                    'pagination' => [
                        'limit' => 20,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error searching departments: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'humanresources.node.search',
        {
            type: 'DEPARTMENT',
            name: 'Sales',
            parentId: 1,
            pagination: {
                limit: 20
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
        'humanresources.node.search',
        [
            'type' => 'DEPARTMENT',
            'name' => 'Sales',
            'parentId' => 1,
            'pagination' => [
                'limit' => 20
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
                "id": 12,
                "name": "B2B Sales",
                "type": "DEPARTMENT",
                "structureId": 1,
                "parentId": 1,
                "description": "Working with corporate clients",
                "accessCode": "DR12",
                "userCount": 9,
                "colorName": null,
                "xmlId": null,
                "createdAt": "2026-05-20T10:15:00+02:00",
                "updatedAt": "2026-06-01T16:30:00+02:00"
            }
        ]
    },
    "time": {
        "start": 1780399800,
        "finish": 1780399800.314519,
        "duration": 0.31451892852783203,
        "processing": 0.2811400890350342,
        "date_start": "2026-06-02T14:30:00+02:00",
        "date_finish": "2026-06-02T14:30:00+02:00",
        "operating_reset_at": 1780400400,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object containing response data ||
|| **items**
[`array`](../../../data-types.md) | Array of found departments and teams ||
|| **items[]**
[`object`](../../../data-types.md) | Object of the found department or team ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the department ||
|| **name**
[`string`](../../../data-types.md) | Name of the department ||
|| **type**
[`string`](../../../data-types.md) | Type of the structure element ||
|| **structureId**
[`integer`](../../../data-types.md) | Identifier of the company structure ||
|| **parentId**
[`integer`](../../../data-types.md) | Identifier of the parent department ||
|| **description**
[`string`](../../../data-types.md) | Description of the department ||
|| **accessCode**
[`string`](../../../data-types.md) | Access code of the department ||
|| **userCount**
[`integer`](../../../data-types.md) | Number of users in the department ||
|| **colorName**
[`string`](../../../data-types.md) | Team color, if specified ||
|| **xmlId**
[`string`](../../../data-types.md) | External identifier of the department ||
|| **createdAt**
[`datetime`](../../../data-types.md) | Date and time of department creation ||
|| **updatedAt**
[`datetime`](../../../data-types.md) | Date and time of the last update of the department ||
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
                "message": "Required field `name` is not specified",
                "field": "name"
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
|| `type`
`name` | Required field `#FIELD#` is not specified | Add the specified field to the request body ||
|| `#FIELD#` | Field `#FIELD#` requires data type `#TYPE#` for this request | Ensure the value passed is of the correct type ||
|| `type` | An invalid department type value was provided | Use `DEPARTMENT` for department or `TEAM` for team ||
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

- [{#T}](./humanresources-node-list.md)
- [{#T}](./humanresources-node-children.md)
- [{#T}](./index.md)
