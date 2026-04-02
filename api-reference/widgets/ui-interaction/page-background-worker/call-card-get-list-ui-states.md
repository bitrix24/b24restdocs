# Get a List of Available Call Card UI States CallCardGetListUiStates

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `CallCardGetListUiStates` returns a list of available states for the call card interface.

{% note info "" %}

The method operates within the context of the application in the placement `PAGE_BACKGROUND_WORKER`.

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT***
[`string`](../../../data-types.md) | The name of the interface command.

For this method — `CallCardGetListUiStates` ||
|| **PARAMS***
[`object`](../../../data-types.md) | The parameters object for the command.

For this method, an empty object is passed: `{}` ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% note info "" %}

It is recommended to call the method after the event [BackgroundCallCard::initialized](./events/initialized.md)

{% endnote %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"PLACEMENT":"CallCardGetListUiStates","PARAMS":{}}' \
      "https://**put_your_bitrix24_address**/rest/placement.call?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.call('CallCardGetListUiStates', {}, function(result) {
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
                    'PLACEMENT' => 'CallCardGetListUiStates',
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
            PLACEMENT: 'CallCardGetListUiStates',
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
            'PLACEMENT' => 'CallCardGetListUiStates',
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
[
    "incoming",
    "transferIncoming",
    "outgoing",
    "connectingIncoming",
    "connectingOutgoing",
    "connected",
    "transferring",
    "transferFailed",
    "transferConnected",
    "error",
    "moneyError",
    "redial"
]
```

### Returned Data

The root element of the response is an array of strings representing the available states of the call card interface.

Possible values:

- `incoming` — incoming call
- `transferIncoming` — incoming call transfer
- `outgoing` — outgoing call
- `connectingIncoming` — connecting incoming call
- `connectingOutgoing` — connecting outgoing call
- `connected` — connection established
- `transferring` — transferring the call
- `transferFailed` — call transfer error
- `transferConnected` — call transfer successfully connected
- `error` — call error
- `moneyError` — error due to insufficient funds
- `redial` — redial

## Error Handling

### REST Call Error

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Application context required"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Application context required | Method called outside the context of the application in the placement `PAGE_BACKGROUND_WORKER` ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./call-card-set-ui-state.md)
- [{#T}](./call-card-set-mute.md)
- [{#T}](./call-card-set-hold.md)
- [{#T}](./call-card-set-card-title.md)
- [{#T}](./call-card-set-status-text.md)
- [{#T}](./call-card-close.md)
- [{#T}](./events/index.md)