# Get a List of Available Methods method.get

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `method.get` method returns two parameters `isExisting` and `isAvailable`, which determine the existence of the method on the account and its availability for invocation.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | The name of the method to check ||
|#

> If the method is called without parameters, it will return a list of all methods available to the current application.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "name": "user.get"
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/method.get
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "name": "user.get",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/method.get
    ```

- JS

    ```js
    BX24.callMethod(
        "method.get",
        {
            "name": "user.get"
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
        'method.get',
        [
            'name' => 'user.get'
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
        "isExisting": true,
        "isAvailable": true
    },
    "time": {
        "start": 1721983189.4629,
        "finish": 1721983189.50346,
        "duration": 0.0405528545379639,
        "processing": 0.000152111053466796,
        "date_start": "2024-07-26T08:39:49+00:00",
        "date_finish": "2024-07-26T08:39:49+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Two parameters are returned:

- `isExisting => true/false` — indicates whether the method exists on this account
- `isAvailable => true/false` — indicates the availability of the method for invocation with the current access permissions ([scope](./scope.md)) of the application ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}


## Continue Learning

- [{#T}](./scope.md)
- [{#T}](./app-info.md)
- [{#T}](./access-name.md)
- [{#T}](./feature-get.md)
- [{#T}](./server-time.md)
- [{#T}](./methods.md)