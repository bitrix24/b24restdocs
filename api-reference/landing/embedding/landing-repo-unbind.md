# Remove Embedding Placement landing.repo.unbind

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: user with View access permission in the Sites section

The method `landing.repo.unbind` removes the embedding placement registered by the current application in the `landing` section.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code**^*^
[`string`](../../data-types.md) | Code of the embedding placement.

Available codes for the `landing` section can be found on the [LANDING_SETTINGS](./settings.md) and [LANDING_BLOCK](./block.md) pages. ||
|| **handler**
[`string`](../../data-types.md) | Path of the embedding placement handler.

The `handler` value must match the `PLACEMENT_HANDLER` field that was passed during the registration of the embedding placement using the `landing.repo.bind` method.

For examples of passing `PLACEMENT_HANDLER`, refer to the [LANDING_SETTINGS](./settings.md) and [LANDING_BLOCK](./block.md) pages.

If the parameter is not provided, the method removes all embedding placements of the current application with the specified `code`.

If the parameter is provided, the method removes only the embedding placement with the specified `code` and `handler`. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of removing an embedding placement, where:
- `code` — code of the embedding placement
- `handler` — path of the embedding placement handler

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "code": "LANDING_SETTINGS",
        "handler": "https://your-domain.com/widgets/landing-settings-handler.php",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.repo.unbind.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.repo.unbind',
            {
                code: 'LANDING_SETTINGS',
                handler: 'https://your-domain.com/widgets/landing-settings-handler.php'
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
                'landing.repo.unbind',
                [
                    'code' => 'LANDING_SETTINGS',
                    'handler' => 'https://your-domain.com/widgets/landing-settings-handler.php',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error unbinding landing placement: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.repo.unbind',
        {
            code: 'LANDING_SETTINGS',
            handler: 'https://your-domain.com/widgets/landing-settings-handler.php'
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
        'landing.repo.unbind',
        [
            'code' => 'LANDING_SETTINGS',
            'handler' => 'https://your-domain.com/widgets/landing-settings-handler.php',
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
        "start": 1775203200,
        "finish": 1775203200.764211,
        "duration": 0.7642109394073486,
        "processing": 0,
        "date_start": "2026-04-03T11:00:00+02:00",
        "date_finish": "2026-04-03T11:00:00+02:00",
        "operating_reset_at": 1775203800,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of successful removal of the embedding placement ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "PLACEMENT_NO_EXIST",
    "error_description": "Such embedding placement does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `MISSING_PARAMS` | Not enough parameters for the call, missing: code | Method call without `code` ||
|| `400` | `PLACEMENT_NO_EXIST` | Such embedding placement does not exist | The current application does not have an embedding placement with the specified `code` and `handler` ||
|| `400` | `ACCESS_DENIED` | Insufficient permissions. | User did not pass the general access checks for the landing module ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./settings.md)
- [{#T}](./block.md)