# Get the number of employees humanresources.employee.count

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: authorized user linked to a department in the company structure

The method `humanresources.employee.count` returns the number of unique employees in the company structure.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The call to the new API differs by adding the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.employee.count`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.employee.count
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.employee.count
    ```


- JS

    SDKs do not currently support calls to the `/rest/api/` address. Use direct HTTP requests, for example, with curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'humanresources.employee.count',
            {}
        );

        const result = response.getData().result;
        console.log(result.total);
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
                'humanresources.employee.count',
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

    SDKs do not currently support calls to the `/rest/api/` address. Use direct HTTP requests, for example, with curl or fetch.

    ```js
    BX24.callMethod(
        'humanresources.employee.count',
        {},
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
        'humanresources.employee.count',
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
        "total": 128
    },
    "time": {
        "start": 1780407600,
        "finish": 1780407600.094112,
        "duration": 0.09411191940307617,
        "processing": 0.06981205940246582,
        "date_start": "2026-06-02T16:40:00+02:00",
        "date_finish": "2026-06-02T16:40:00+02:00",
        "operating_reset_at": 1780408200,
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
|| **total**
[`integer`](../../../data-types.md) | Number of unique employees in the company structure ||
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
- [{#T}](./humanresources-employee-multidepartment.md)
- [{#T}](./index.md)
