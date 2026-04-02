# Close Call Card from the CallCardClose Application

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `CallCardClose` method closes the call card.

{% note info "" %}

The method operates within the context of the application in the `PAGE_BACKGROUND_WORKER` placement.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT***
[`string`](../../../data-types.md) | The name of the interface command.

For this method — `CallCardClose` ||
|| **PARAMS***
[`object`](../../../data-types.md) | The parameters object for the command.

For this method, an empty object is passed: `{}` ||
|#

{% note warning %}

After calling `CallCardClose`, further management of the card is not possible.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

It is recommended to call the method after the [BackgroundCallCard::initialized](./events/initialized.md) event.

{% endnote %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"PLACEMENT":"CallCardClose","PARAMS":{}}' \
      "https://**put_your_bitrix24_address**/rest/placement.call?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.call('CallCardClose', {}, function(result) {
        console.log(result);
    });
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'placement.call',
                [
                    'PLACEMENT' => 'CallCardClose',
                    'PARAMS' => []
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'placement.call',
        {
            PLACEMENT: 'CallCardClose',
            PARAMS: {}
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
        'placement.call',
        [
            'PLACEMENT' => 'CallCardClose',
            'PARAMS' => (object)[]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

```json
[]
```

### Returned Data

An empty array upon successful call.

## Error Handling

### REST Call Error

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Application context required"
}
```

### Interface Call Error

```json
[
    {
        "result": "error",
        "errorCode": "Call card is undefined"
    }
]
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Application context required | The method was called outside the application context in the `PAGE_BACKGROUND_WORKER` placement ||
|| `Call card is undefined` | The call card is unavailable | No active call card to close ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./call-card-set-mute.md)
- [{#T}](./call-card-set-hold.md)
- [{#T}](./call-card-set-ui-state.md)
- [{#T}](./call-card-get-list-ui-states.md)
- [{#T}](./call-card-set-card-title.md)
- [{#T}](./call-card-set-status-text.md)
- [{#T}](./events/index.md)