# Call Status Change Event CallCard::CallStateChanged

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `CallCard::CallStateChanged` is triggered when the status of the current call changes.

{% note info "" %}

The event operates within the context of the application in the `CALL_CARD` placement.

{% endnote %}

## What the handler receives

Data is passed to the callback `BX24.placement.bindEvent` {.b24-info}

```js
callback(
    "idle",
    {
        "failedCode": "486"
    }
);
```

## Event handler parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **callState***
[`string`](../../../data-types.md) | The current state of the call.

Possible values:

- `idle` — no connection
- `connecting` — connection is being established
- `connected` — connection established ||
|| **additionalParams**
[`object`](../../../data-types.md) | Additional data [(detailed description)](#additional_params) ||
|#

### Parameter additionalParams{#additional_params}

#|
|| **Parameter**
`type` | **Description** ||
|| **failedCode**
[`string`](../../../data-types.md) | Call termination code. Passed only on unsuccessful termination when `callState = idle` ||
|#

## Subscription parameters for the event

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT***
[`string`](../../../data-types.md) | The name of the interface event.

For this event — `CallCard::CallStateChanged` ||
|| **HANDLER***
[`string`](../../../data-types.md) | URL of the event handler for calling `placement.bindEvent` ||
|#

## Code examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"CallCard::CallStateChanged","HANDLER":"**your_handler_url_here**"}' \
    "https://**put_your_bitrix24_address**/rest/placement.bindEvent?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.bindEvent('CallCard::CallStateChanged', function (callState, additionalParams) {
        console.log(callState, additionalParams);
    });
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'placement.bindEvent',
                [
                    'PLACEMENT' => 'CallCard::CallStateChanged',
                    'HANDLER' => '**your_handler_url_here**'
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
        'placement.bindEvent',
        {
            PLACEMENT: 'CallCard::CallStateChanged',
            HANDLER: '**your_handler_url_here**'
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
        'placement.bindEvent',
        [
            'PLACEMENT' => 'CallCard::CallStateChanged',
            'HANDLER' => '**your_handler_url_here**'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue your exploration

- [{#T}](./get-status.md)
- [{#T}](./disable-auto-close.md)
- [{#T}](./enable-auto-close.md)
- [{#T}](./call-card-entity-changed.md)
- [{#T}](./call-card-before-close.md)