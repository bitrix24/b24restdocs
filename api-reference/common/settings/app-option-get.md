# Get Application-Linked Data app.option.get

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `app.option.get` retrieves data linked to the application. If no input is provided, it will return all properties recorded via [app.option.set](./app-option-set.md).

## Parameters

#|
|| **Name**
`type` | **Description** ||
|| **option**
[`string`](../../data-types.md) | A string, one of the keys from the property [app.option.set](./app-option-set.md). ||
|#

## Code Examples

{% include [Example Notes](../../../_includes/examples.md) %}


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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/app.option.get
    ```

    Example #2

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/app.option.get
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
    https://**put_your_bitrix24_address**/rest/app.option.get
    ```
    
    Example #2
    
    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/app.option.get
    ```

- JS

    Example #1

    ```js
    BX24.callMethod(
        'app.option.get',
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
        'app.option.get', {},
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
        'app.option.get',
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
        'app.option.get',
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

The method returns data linked to the application.

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
- [{#T}](./user-option-set.md)
- [{#T}](./user-option-get.md)