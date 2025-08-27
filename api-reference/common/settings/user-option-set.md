# Bind Data to User and Application user.option.set

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `user.option.set` binds data to the application and user.

The application can be bound to the user who installed it if it is a [headless application](../../../local-integrations/serverside-local-app-with-no-ui.md) or to the user with whom it interacts if it is a [UI application](../../../local-integrations/serverside-local-app-with-ui.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **options***
[`array`](../../data-types.md) | An array where the key is the name of the property to be saved, and the value is the property value. If a value with a new key is passed, the method will write it; if an existing one, it will update it. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "options": {
            "data": "value",
            "data2": "value2"
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.option.set
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "options": {
            "data": "value",
            "data2": "value2"
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/user.option.set
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'user.option.set',
            {
                "options": {
                    "data": "value",
                    "data2": "value2",
                }
            }
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
                'user.option.set',
                [
                    'options' => [
                        'data'  => 'value',
                        'data2' => 'value2',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting user options: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'user.option.set',
        {
            "options": {
                "data": "value",
                "data2": "value2",
            }
        },
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
        'user.option.set',
        [
            'options' => [
                'data' => 'value',
                'data2' => 'value2'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Error Handling

HTTP status: **400**

```json
{
    "error":"ArgumentNullException",
    "error_description":"options is empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ArgumentNullException` | options is empty | Empty array `options`  ||
|| `AccessException` | Application context required / Administrator authorization required | Access denied ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./app-option-set.md)
- [{#T}](./app-option-get.md)
- [{#T}](./user-option-get.md)