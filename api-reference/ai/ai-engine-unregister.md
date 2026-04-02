# Remove the ai.engine.unregister Service

> Scope: [`ai_admin`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `ai.engine.unregister` removes a registered AI service.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../data-types.md) | Character code of the service to be removed ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "code": "acme_gpt"
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_webhook_id**/**put_your_webhook_code**/ai.engine.unregister.json
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "code": "acme_gpt",
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/ai.engine.unregister
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'ai.engine.unregister',
            {
                code: 'acme_gpt'
            }
        );

        const result = response.getData().result;
        console.log('Engine removed:', result);
    }
    catch (error)
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
                'ai.engine.unregister',
                [
                    'code' => 'acme_gpt',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error unregistering AI engine: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'ai.engine.unregister',
        {
            code: 'acme_gpt'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'ai.engine.unregister',
        [
            'code' => 'acme_gpt',
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
    "result": true,
    "time": {
        "start": 1774078200,
        "finish": 1774078200.184271,
        "duration": 0.18427085876464844,
        "processing": 0.03,
        "date_start": "2026-03-20T09:50:00+01:00",
        "date_finish": "2026-03-20T09:50:00+01:00",
        "operating_reset_at": 1774078800,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Result of the service removal:

- `true` — service removed
- `false` — service not found, does not belong to the current application, or removal not completed ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../_includes/error-info.md) %}

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./ai-engine-register.md)
- [{#T}](./ai-engine-list.md)