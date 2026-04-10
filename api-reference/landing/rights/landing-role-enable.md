# Enable or Disable Role-Based Access Model landing.role.enable

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: administrator or user with "full access" permission to the "Sites and Stores" section

The method `landing.role.enable` enables or disables the role-based access model for the "Sites and Stores" section.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **mode***
[`integer`](../../data-types.md) | Access model mode. You can check the current model using the [landing.role.isEnabled](./landing-role-is-enabled.md) method. Possible values:
- `1` - enable role-based access model
- `0` - disable role-based access model and use the extended model ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "mode": 1
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.role.enable.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "mode": 1,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.role.enable.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.role.enable',
            {
                mode: 1
            }
        );

        const result = response.getData().result;
        console.info(result);
    }
    catch (error)
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
                'landing.role.enable',
                [
                    'mode' => 1,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error enabling role model: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.role.enable',
        {
            mode: 1
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.role.enable',
        [
            'mode' => 1,
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo '<pre>';
        print_r($result['result']);
        echo '</pre>';
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1775073692,
        "finish": 1775073692.6858,
        "duration": 0.6858000755310059,
        "processing": 0,
        "date_start": "2026-04-01T23:01:32+02:00",
        "date_finish": "2026-04-01T23:01:32+02:00",
        "operating_reset_at": 1775074292,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Indicates successful execution of the method.

The method returns `true` if the call was executed without errors. If the desired model was already enabled, the response will still be `true` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough parameters for the call, missing: mode"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions to access the "Sites and Stores" section ||
|| `IS_NOT_ADMIN` | The method requires administrator rights or "full access" permission to the "Sites and Stores" section ||
|| `FEATURE_NOT_AVAIL` | Managing permissions in the "Sites and Stores" section is not available on the current plan ||
|| `MISSING_PARAMS` | The required parameter `mode` is missing ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-role-is-enabled.md)
- [{#T}](./extended-model/landing-site-get-rights.md)
- [{#T}](./extended-model/landing-site-set-rights.md)
- [{#T}](./role-model/landing-role-get-list.md)
- [{#T}](./role-model/landing-role-get-rights.md)
- [{#T}](./role-model/landing-role-set-access-codes.md)
- [{#T}](./role-model/landing-role-set-rights.md)