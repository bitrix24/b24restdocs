# Find Employess humanresourses.employee.search

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: an authorized user linked to a department in the company structure.

The method `humanresources.employee.search` searches for employees by name.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Search string for the employee's name ||
|| **nodeId**
[`integer`](../../../data-types.md) | Identifier of the department or team to limit the search.

The identifier can be obtained using the [humanresources.node.list](../node/humanresources-node-list.md) method. ||
|| **select**
[`array`](../../../data-types.md) | List of employee fields to return.

Available fields:

- `userId` — user identifier
- `name` — employee name
- `workPosition` — position
- `avatar` — link to avatar
- `url` — link to profile
- `departments` — employee's departments
- `teams` — employee's teams ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.employee.search`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"John","select":["userId","name","workPosition","avatar","url","departments","teams"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.employee.search
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"John","select":["userId","name","workPosition","avatar","url","departments","teams"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.employee.search
    ```


- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EmployeeSearchResult = {
      items: {
        userId: number
        name: string
        workPosition: string
        avatar: string
        url: string
        departments: { id: number; name: string }[]
        teams: { id: number; name: string }[]
      }[]
    }

    try {
      const response = await $b24.actions.v3.call.make<EmployeeSearchResult>({
        method: 'humanresources.employee.search',
        params: {
          name: 'Ivan',
          select: [
            'userId',
            'name',
            'workPosition',
            'avatar',
            'url',
            'departments',
            'teams',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.items)
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
      async function searchEmployees() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'humanresources.employee.search',
            params: {
              name: 'Ivan',
              select: [
                'userId',
                'name',
                'workPosition',
                'avatar',
                'url',
                'departments',
                'teams',
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
          console.info(result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', searchEmployees)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.employee.search',
                [
                    'name' => 'John',
                    'select' => [
                        'userId',
                        'name',
                        'workPosition',
                        'avatar',
                        'url',
                        'departments',
                        'teams',
                    ],
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
        'humanresources.employee.search',
        {
            name: 'John',
            select: [
                'userId',
                'name',
                'workPosition',
                'avatar',
                'url',
                'departments',
                'teams'
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
        'humanresources.employee.search',
        [
            'name' => 'John',
            'select' => [
                'userId',
                'name',
                'workPosition',
                'avatar',
                'url',
                'departments',
                'teams',
            ],
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
                "userId": 7,
                "name": "John Smith",
                "workPosition": "Sales Manager",
                "avatar": "https://example.bitrix24.com/upload/main/avatar.jpg",
                "url": "/company/personal/user/7/",
                "departments": [
                    {
                        "id": 15,
                        "name": "Sales Department"
                    }
                ],
                "teams": []
            }
        ]
    },
    "time": {
        "start": 1780407400,
        "finish": 1780407400.143201,
        "duration": 0.14320111274719238,
        "processing": 0.11092114448547363,
        "date_start": "2026-06-02T16:36:40+02:00",
        "date_finish": "2026-06-02T16:36:40+02:00",
        "operating_reset_at": 1780408000,
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
|| **items**
[`array`](../../../data-types.md) | Array of found employees. The structure of the element fields depends on `select` ||
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
                "message": "Parameter \"name\" is required.",
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
|| `name` | Parameter `"name"` is required. | Provide a search string ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` for object `EmployeeDto` | Provide only employee fields from the list ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unable to recognize select expression `#SELECT#` | Provide `select` as an array of strings ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Ensure that the current user is an employee of Bitrix24 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-employee-subordinates.md)
- [{#T}](./humanresources-employee-count.md)
- [{#T}](./index.md)
