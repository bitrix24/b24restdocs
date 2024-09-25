# Get User Data Associated with the Application user.option.get

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `user.option.get` retrieves user data associated with the application. If no input is provided, it will return all properties recorded through [user.option.set](./user-option-set.md).

## Parameters

#|
|| **Name**
`type` | **Description** ||
|| **option**
[`string`](../../data-types.md) | A string, one of the keys from the property [user.option.set](./user-option-set.md). ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}


{% list tabs %}

- cURL (Webhook)

    Example #1

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "option": "data"
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.option.get
    ```

    Example #2

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.option.get
    ```

- cURL (OAuth)

    Example #1

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "option": "data",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/user.option.get
    ```
    
    Example #2
    
    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/user.option.get
    ```

- JS

    Example #1

    ```js
    BX24.callMethod(
        'user.option.get',
        {
            "option":"data"
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
    
    Example #2
    
    ```js
    BX24.callMethod(
        'user.option.get', {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    Example #1
    
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'user.option.get',
        [
            'option' => 'data'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

    Example #2
    
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'user.option.get',
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
    "data": "value",
    "data2": "value2"
}
```

The method returns user data associated with the application.


## Error Handling

HTTP Status: **400**

```json
{
    "error":"AccessException",
    "error_description":"Application context required / User authorization required"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `AccessException` | Application context required / Administrator authorization required | Access denied ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./app-option-set.md)
- [{#T}](./app-option-get.md)
- [{#T}](./user-option-set.md)