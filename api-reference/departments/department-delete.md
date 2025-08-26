# Delete department.department.delete

> Scope: [`department`](../scopes/permissions.md)
>
> Who can execute the method: user with rights to modify the structure

The method `department.delete` removes a department from the company structure.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`int`](../data-types.md) | Identifier of the department ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":18}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/department.delete
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":18,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/department.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"department.delete", {
    			"ID": 18
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'department.delete',
                [
                    'ID' => 18
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting department: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "department.delete", {
            "ID": 18
        },
        function(result) {
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'department.delete',
        [
            'ID' => 18
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
  "result": true,
  "time": {
    "start": 1736929002.119324,
    "finish": 1736929002.469626,
    "duration": 0.35030198097229004,
    "processing": 0.13488411903381348,
    "date_start": "2025-01-15T08:16:42+00:00",
    "date_finish": "2025-01-15T08:16:42+00:00",
    "operating": 0.13484978675842285
  }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Result of the department deletion in the company structure ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"Department not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ERROR_CORE` | Department not found | The department to be deleted was not found ||
|| `ERROR_CORE` | Access denied | Insufficient rights to delete the department ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./department-add.md)
- [{#T}](./department-update.md)
- [{#T}](./department-get.md)
- [{#T}](./department-fields.md)