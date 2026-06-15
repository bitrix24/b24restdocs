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

The call to the new API differs by adding the `/api/` parameter in the request:

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


- JS

    SDKs do not currently support calls to the `/rest/api/` address. Use direct HTTP requests, for example, with curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'humanresources.employee.subordinates',
            {
                id: 7
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

    SDKs do not currently support calls to the `/rest/api/` address. Use direct HTTP requests, for example, with curl or fetch.

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

    SDKs do not currently support calls to the `/rest/api/` address. Use direct HTTP requests, for example, with curl or fetch.

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
