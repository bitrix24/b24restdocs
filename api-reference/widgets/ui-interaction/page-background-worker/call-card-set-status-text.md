# Change Text in the Call Card Center via CallCardSetStatusText Application

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `CallCardSetStatusText` method changes the status text in the center of the call card.

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

For this method — `CallCardSetStatusText` ||
|| **PARAMS***
[`object`](../../../data-types.md) | The parameters object for the command.

For this method, an object is passed with the `statusText` property [(detailed description)](#params) ||
|#

### PARAMS Parameter {#params}

#|
|| **Name**
`type` | **Description** ||
|| **statusText***
[`string`](../../../data-types.md) | The new status text in the center of the card ||
|#

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
      -d '{"PLACEMENT":"CallCardSetStatusText","PARAMS":{"statusText":"Waiting for client response"}}' \
      "https://**put_your_bitrix24_address**/rest/placement.call?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.call(
        'CallCardSetStatusText',
        { statusText: 'Waiting for client response' },
        function(result) {
            console.log(result);
        }
    );
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'placement.call',
                [
                    'PLACEMENT' => 'CallCardSetStatusText',
                    'PARAMS' => [
                        'statusText' => 'Waiting for client response',
                    ]
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
            PLACEMENT: 'CallCardSetStatusText',
            PARAMS: {
                statusText: 'Waiting for client response'
            }
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
            'PLACEMENT' => 'CallCardSetStatusText',
            'PARAMS' => [
                'statusText' => 'Waiting for client response',
            ]
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
|| `WRONG_AUTH_TYPE` | Application context required | Method called outside the application context in the `PAGE_BACKGROUND_WORKER` placement ||
|| `Call card is undefined` | Call card is unavailable | No active call card for management ||
|| `missing field statusText` | Required parameter `statusText` not provided | In the desktop scenario, the `statusText` field is mandatory ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./call-card-set-card-title.md)
- [{#T}](./call-card-set-mute.md)
- [{#T}](./call-card-set-hold.md)
- [{#T}](./call-card-set-ui-state.md)
- [{#T}](./call-card-get-list-ui-states.md)
- [{#T}](./call-card-close.md)
- [{#T}](./events/index.md)