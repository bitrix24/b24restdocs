# Get Basic Information About the Current User Profile

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `profile` method allows you to retrieve basic information about the current user without any scopes, unlike [user.current](../../user/user-current.md).

No parameters required.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/profile
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/profile
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"profile",
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'profile',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling profile method: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "profile",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'profile',
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
        "ID": "1",
        "ADMIN": true,
        "NAME": "Vadim",
        "LAST_NAME": "Valeev",
        "PERSONAL_GENDER": "",
        "TIME_ZONE": ""
    },
    "time": {
        "start": 1722848182.67776,
        "finish": 1722848182.71787,
        "duration": 0.0401120185852051,
        "processing": 0.00115704536437988,
        "date_start": "2024-08-05T08:56:22+00:00",
        "date_finish": "2024-08-05T08:56:22+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The object contains information about the user ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-admin.md)
- [{#T}](./user-access.md)