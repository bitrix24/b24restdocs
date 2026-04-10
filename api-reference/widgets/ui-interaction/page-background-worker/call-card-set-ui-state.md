# Change the Call Card Interface State via the CallCardSetUiState Application Method

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `CallCardSetUiState` method changes the state of the call card interface.

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

For this method — `CallCardSetUiState` ||
|| **PARAMS***
[`object`](../../../data-types.md) | The parameters object for the command.

For this method, an object with properties `uiState` and `disableAutoStartTimer` is passed [(detailed description)](#params) ||
|#

### PARAMS Parameter {#params}

#|
|| **Name**
`type` | **Description** ||
|| **uiState***
[`string`](../../../data-types.md) | The state of the call card interface.

Available values can be obtained using the [CallCardGetListUiStates](./call-card-get-list-ui-states.md) method ||
|| **disableAutoStartTimer**
[`boolean`](../../../data-types.md) | An additional parameter for `uiState = connected`.

Possible values:

- `true` — do not start the timer automatically
- `false` or absence of the parameter — start the timer automatically ||
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
      -d '{"PLACEMENT":"CallCardSetUiState","PARAMS":{"uiState":"connected","disableAutoStartTimer":true}}' \
      "https://**put_your_bitrix24_address**/rest/placement.call?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.call(
        'CallCardSetUiState',
        {
            uiState: 'connected',
            disableAutoStartTimer: true
        },
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
                    'PLACEMENT' => 'CallCardSetUiState',
                    'PARAMS' => [
                        'uiState' => 'connected',
                        'disableAutoStartTimer' => true,
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
            PLACEMENT: 'CallCardSetUiState',
            PARAMS: {
                uiState: 'connected',
                disableAutoStartTimer: true
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
            'PLACEMENT' => 'CallCardSetUiState',
            'PARAMS' => [
                'uiState' => 'connected',
                'disableAutoStartTimer' => true,
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
        "errorCode": "Invalid ui state"
    }
]
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Application context required | Method called outside the application context in the `PAGE_BACKGROUND_WORKER` placement ||
|| `Call card is undefined` | Call card is unavailable | No active call card to manage ||
|| `Invalid ui state` | Invalid `uiState` value | The provided state is not in the list of supported states ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./call-card-set-mute.md)
- [{#T}](./call-card-set-hold.md)
- [{#T}](./call-card-get-list-ui-states.md)
- [{#T}](./call-card-set-card-title.md)
- [{#T}](./call-card-set-status-text.md)
- [{#T}](./call-card-close.md)
- [{#T}](./events/index.md)