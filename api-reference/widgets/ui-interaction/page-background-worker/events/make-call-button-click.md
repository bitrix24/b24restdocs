# When the "Call" or "Callback" Button is Clicked: BackgroundCallCard::makeCallButtonClick

> Scope: [`telephony`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `BackgroundCallCard::makeCallButtonClick` is triggered when a call is initiated from the call card.

{% note info "" %}

The event operates within the application context in the `PAGE_BACKGROUND_WORKER` placement.

{% endnote %}

## What the Handler Receives

No data is passed to the event handler.

## Event Subscription Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **PLACEMENT*** 
[`string`](../../../../data-types.md) | The name of the interface event.

For this event — `BackgroundCallCard::makeCallButtonClick` ||
|| **HANDLER*** 
[`string`](../../../../data-types.md) | The URL of the event handler for the call `placement.bindEvent` ||
|#

## Code Examples

{% include [Note on Examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"BackgroundCallCard::makeCallButtonClick","HANDLER":"**your_handler_url_here**"}' \
    "https://**put_your_bitrix24_address.com**/rest/placement.bindEvent?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.bindEvent('BackgroundCallCard::makeCallButtonClick', function (eventData) {
        console.log(eventData);
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
                    'PLACEMENT' => 'BackgroundCallCard::makeCallButtonClick',
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
            PLACEMENT: 'BackgroundCallCard::makeCallButtonClick',
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
            'PLACEMENT' => 'BackgroundCallCard::makeCallButtonClick',
            'HANDLER' => '**your_handler_url_here**'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../card.md)