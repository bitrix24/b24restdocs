# Get Employees in Multiple Departments humanresources.employee.multidepartment

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: authorized user linked to a department in the company structure

The method `humanresources.employee.multidepartment` returns employees who belong to multiple departments.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

```plaintext
https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.employee.multidepartment
```

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.employee.multidepartment
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.employee.multidepartment
    ```


- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MultidepartmentResult = {
      employees: {
        userId: number
        name: string
        workPosition: string
        departments: {
          id: number
          name: string
        }[]
      }[]
      total: number
    }

    try {
      const response = await $b24.actions.v3.call.make<MultidepartmentResult>({
        method: 'humanresources.employee.multidepartment',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Total employees in multiple departments:', result.total)
        console.info('Employees:', result.employees)
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
      async function getMultidepartmentEmployees() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'humanresources.employee.multidepartment',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Total employees in multiple departments:', result.total)
          console.info('Employees:', result.employees)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getMultidepartmentEmployees)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.employee.multidepartment',
                []
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
        'humanresources.employee.multidepartment',
        {},
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
        'humanresources.employee.multidepartment',
        []
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
        "employees": [
            {
                "userId": 7,
                "name": "John Smith",
                "workPosition": "",
                "departments": [
                    {
                        "id": 15,
                        "name": "Sales Department"
                    },
                    {
                        "id": 22,
                        "name": "Development Department"
                    }
                ]
            },
            {
                "userId": 11,
                "name": "Mary Johnson",
                "workPosition": "Department Head",
                "departments": [
                    {
                        "id": 22,
                        "name": "Development Department"
                    },
                    {
                        "id": 31,
                        "name": "Project Office"
                    }
                ]
            }
        ],
        "total": 2
    },
    "time": {
        "start": 1780407700,
        "finish": 1780407700.166208,
        "duration": 0.16620802879333496,
        "processing": 0.12675189971923828,
        "date_start": "2026-06-02T16:41:40+02:00",
        "date_finish": "2026-06-02T16:41:40+02:00",
        "operating_reset_at": 1780408300,
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
|| **employees**
[`array`](../../../data-types.md) | Array of employees who belong to multiple departments ||
|| **employees[].userId**
[`integer`](../../../data-types.md) | User identifier ||
|| **employees[].name**
[`string`](../../../data-types.md) | Employee name ||
|| **employees[].workPosition**
[`string`](../../../data-types.md) | Employee position ||
|| **employees[].departments[]**
[`array`](../../../data-types.md) | Array of employee departments ||
|| **employees[].departments[].id**
[`integer`](../../../data-types.md) | Department identifier ||
|| **employees[].departments[].name**
[`string`](../../../data-types.md) | Department name ||
|| **total**
[`integer`](../../../data-types.md) | Number of found employees ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Ensure the current user is an employee of Bitrix24 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-employee-search.md)
- [{#T}](./humanresources-employee-count.md)
- [{#T}](./index.md)
