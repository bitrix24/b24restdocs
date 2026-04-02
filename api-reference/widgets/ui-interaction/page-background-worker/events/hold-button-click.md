# On Clicking the Hold Button BackgroundCallCard::holdButtonClick

> Scope: [`telephony`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `BackgroundCallCard::holdButtonClick` is triggered when the operator clicks the hold button.

{% note info "" %}

The event operates within the application in the placement `PAGE_BACKGROUND_WORKER`.

{% endnote %}

## What the Handler Receives

Data is passed to the callback `BX24.placement.bindEvent` {.b24-info}

```js
callback(true);
```

## Event Handler Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **eventData*** 
[`boolean`](../../../../data-types.md) | The current hold status after the button is clicked.

Possible values:

- `true` — on hold
- `false` — hold released ||
|#

## Event Subscription Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT*** 
[`string`](../../../../data-types.md) | The name of the interface event.

For this event — `BackgroundCallCard::holdButtonClick` ||
|| **HANDLER*** 
[`string`](../../../../data-types.md) | The URL of the event handler for calling `placement.bindEvent` ||
|#

## Code Examples

{% include [Note on Examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"BackgroundCallCard::holdButtonClick","HANDLER":"**your_handler_url_here**"}' \
    "https://**put_your_bitrix24_address.com**/rest/placement.bindEvent?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.bindEvent('BackgroundCallCard::holdButtonClick', function (eventData) {
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
                    'PLACEMENT' => 'BackgroundCallCard::holdButtonClick',
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
            PLACEMENT: 'BackgroundCallCard::holdButtonClick',
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
            'PLACEMENT' => 'BackgroundCallCard::holdButtonClick',
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