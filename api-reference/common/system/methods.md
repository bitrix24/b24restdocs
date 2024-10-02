# Get a List of Available Methods

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `methods` method retrieves a list of available methods.

{% note alert %}

This method is deprecated; it is strongly recommended to use [method.get](./method-get.md) instead.

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **full**
[`boolean`](../../data-types.md) | If the parameter is set to `true`, the method will return a list of all methods ||
|| **scope**
[`string`](../../data-types.md) | Displays methods included in the specified permission. If the parameter is provided without a value (`methods?scope=&auth=xxxxx`), all common methods will be displayed. ||
|#

> If the method is called without parameters, it will return a list of all methods available to the current application.

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "scope": "user"
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/methods
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "scope": "user",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/methods
    ```

- JS

    ```js
    BX24.callMethod(
        "methods",
        {
            "scope": "user"
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'methods',
        [
            'scope' => 'user'
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
    "result": [
        "user.fields",
        "user.current",
        "user.get",
        "user.search",
        "user.add",
        "user.update",
        "user.online",
        "user.counters",
        "user.history.list",
        "user.history.fields.list"
    ],
    "time": {
        "start": 1721986432.32646,
        "finish": 1721986432.3598,
        "duration": 0.0333478450775147,
        "processing": 0.000032901763916015,
        "date_start": "2024-07-26T09:33:52+00:00",
        "date_finish": "2024-07-26T09:33:52+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | An array containing the list of permissions ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./method-get.md)
- [{#T}](./scope.md)
- [{#T}](./app-info.md)
- [{#T}](./access-name.md)
- [{#T}](./feature-get.md)
- [{#T}](./server-time.md)