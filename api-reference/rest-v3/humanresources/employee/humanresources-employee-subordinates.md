# Get Subordinates of Employee humanresources.employee.subordinates

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: authorized user linked to a department in the company structure

The method `humanresources.employee.subordinates` returns the number of subordinates of the user by departments.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the user for whom to get subordinates.

The identifier can be obtained using the [user.get](../../../user/user-get.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

```plaintext
https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.employee.subordinates
```

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.employee.subordinates
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.employee.subordinates
    ```


- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SubordinatesResult = {
      userId: number
      departments: Array<{
        nodeId: number
        name: string
        role: string
        subordinatesCount: number
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<SubordinatesResult>({
        method: 'humanresources.employee.subordinates',
        params: {
          id: 7,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.userId, result.departments)
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
      async function getEmployeeSubordinates() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'humanresources.employee.subordinates',
            params: {
              id: 7,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.userId, result.departments)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getEmployeeSubordinates)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.employee.subordinates',
                [
                    'id' => 7,
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
        'humanresources.employee.subordinates',
        {
            id: 7
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
        'humanresources.employee.subordinates',
        [
            'id' => 7,
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
        "userId": 7,
        "departments": [
            {
                "nodeId": 15,
                "name": "Sales Department",
                "role": "HEAD",
                "subordinatesCount": 12
            },
            {
                "nodeId": 22,
                "name": "Development Department",
                "role": "HEAD",
                "subordinatesCount": 5
            },
            {
                "nodeId": 31,
                "name": "Support Department",
                "role": "HEAD",
                "subordinatesCount": 3
            }
        ]
    },
    "time": {
        "start": 1780407500,
        "finish": 1780407500.128491,
        "duration": 0.12849116325378418,
        "processing": 0.09751296043395996,
        "date_start": "2026-06-02T16:38:20+02:00",
        "date_finish": "2026-06-02T16:38:20+02:00",
        "operating_reset_at": 1780408100,
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
|| **userId**
[`integer`](../../../data-types.md) | Identifier of the user from the request ||
|| **departments[]**
[`array`](../../../data-types.md) | Array of departments with the number of the user's subordinates ||
|| **departments[].nodeId**
[`integer`](../../../data-types.md) | Identifier of the department ||
|| **departments[].name**
[`string`](../../../data-types.md) | Name of the department ||
|| **departments[].role**
[`string`](../../../data-types.md) | User's role in the department ||
|| **departments[].subordinatesCount**
[`integer`](../../../data-types.md) | Number of the user's subordinates in the department ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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
                "message": "Parameter \"id\" is required and must be a positive integer.",
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
|| `id` | Required field `id` is not specified | Provide the user identifier ||
|| `id` | Parameter `"id"` is required and must be a positive integer | Provide a positive user identifier ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Ensure that the current user is an employee of Bitrix24 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-employee-search.md)
- [{#T}](./index.md)
