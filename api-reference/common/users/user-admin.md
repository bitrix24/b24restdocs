# Determine Access Permissions for Managing Application Settings user.admin

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `user.admin` determines whether the current user has the permissions to manage application settings.

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.admin
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/user.admin
    ```

- JS

    ```js
    BX24.callMethod(
        "user.admin",
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'user.admin',
        []
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
        "start": 1721998816.3694,
        "finish": 1721998816.43663,
        "duration": 0.0672309398651123,
        "processing": 0.000064849853515625,
        "date_start": "2024-07-26T13:00:16+00:00",
        "date_finish": "2024-07-26T13:00:16+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the current user has permissions to manage application settings, `false` otherwise ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-access.md)
- [{#T}](./profile.md)

## See Also

- [BX24.isAdmin](../../bx24-js-sdk/additional-functions/bx24-is-admin.md)