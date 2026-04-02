# When pressing one of the numeric buttons on the phone, BackgroundCallCard::dialpadButtonClick

> Scope: [`telephony`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `BackgroundCallCard::dialpadButtonClick` is triggered when a key on the numeric keypad is pressed during a call.

{% note info "" %}

The event operates within the context of the application in the placement `PAGE_BACKGROUND_WORKER`.

{% endnote %}

## What the handler receives

Data is passed to the callback `BX24.placement.bindEvent` {.b24-info}

```js
callback("8");
```

## Event handler parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **eventData***
[`string`](../../../../data-types.md) | The pressed key: `0-9`, `*` or `#` ||
|#

## Subscription parameters for the event

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT***
[`string`](../../../../data-types.md) | The name of the interface event.

For this event — `BackgroundCallCard::dialpadButtonClick` ||
|| **HANDLER***
[`string`](../../../../data-types.md) | The URL of the event handler for calling `placement.bindEvent` ||
|#

## Code examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"BackgroundCallCard::dialpadButtonClick","HANDLER":"**your_handler_url_here**"}' \
    "https://**put_your_bitrix24_address**/rest/placement.bindEvent?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.bindEvent('BackgroundCallCard::dialpadButtonClick', function (eventData) {
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
                    'PLACEMENT' => 'BackgroundCallCard::dialpadButtonClick',
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
            PLACEMENT: 'BackgroundCallCard::dialpadButtonClick',
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
            'PLACEMENT' => 'BackgroundCallCard::dialpadButtonClick',
            'HANDLER' => '**your_handler_url_here**'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue your exploration

- [{#T}](./index.md)
- [{#T}](../card.md)