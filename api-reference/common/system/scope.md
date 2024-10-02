# Get a List of Available Permissions Scope

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `scope` method returns a list of permissions.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **full**
[`boolean`](../../data-types.md) | If the parameter is set to `true`, the method will return the complete [list of permissions](../../scopes/permissions.md) ||
|#

> If the method is called without parameters, it will return all permissions available for this application.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "full": true
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/scope
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "full": true,
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/scope
    ```

- JS

    ```js
    BX24.callMethod(
        "scope",
        {
            "full": true
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
        'scope',
        [
            'full' => true,
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
        "department",
        "entity",
        "log",
        "user"
    ],
    "time": {
        "start": 1721993326.41019,
        "finish": 1721993326.44899,
        "duration": 0.0387959480285645,
        "processing": 0.000020980834960937,
        "date_start": "2024-07-26T11:28:46+00:00",
        "date_finish": "2024-07-26T11:28:46+00:00",
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
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./method-get.md)
- [{#T}](./app-info.md)
- [{#T}](./access-name.md)
- [{#T}](./feature-get.md)
- [{#T}](./server-time.md)
- [{#T}](./methods.md)