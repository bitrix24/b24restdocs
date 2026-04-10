# In the Call Card, When Changing the Current Dialed Client BackgroundCallCard::entityChanged

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `BackgroundCallCard::entityChanged` is triggered when the current client in the call card is changed.

{% note info "" %}

The event operates within the context of the application in the placement `PAGE_BACKGROUND_WORKER`.

{% endnote %}

## What the Handler Receives

Data is passed to the callback `BX24.placement.bindEvent` {.b24-info}

```js
callback({
    "PHONE_NUMBER": "+19001234567",
    "CRM_ENTITY_TYPE": "CONTACT",
    "CRM_ENTITY_ID": 123
});
```

## Event Handler Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **PHONE_NUMBER***
[`string`](../../../../data-types.md) | The client's phone number ||
|| **CRM_ENTITY_TYPE***
[`string`](../../../../data-types.md) | The type of the CRM entity associated with the call ||
|| **CRM_ENTITY_ID***
[`integer`](../../../../data-types.md) | The identifier of the CRM entity associated with the call ||
|#

## Event Subscription Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT***
[`string`](../../../../data-types.md) | The name of the interface event.

For this event — `BackgroundCallCard::entityChanged` ||
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
    -d '{"PLACEMENT":"BackgroundCallCard::entityChanged","HANDLER":"**your_handler_url_here**"}' \
    "https://**put_your_bitrix24_address**/rest/placement.bindEvent?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.bindEvent('BackgroundCallCard::entityChanged', function (eventData) {
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
                    'PLACEMENT' => 'BackgroundCallCard::entityChanged',
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
            PLACEMENT: 'BackgroundCallCard::entityChanged',
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
            'PLACEMENT' => 'BackgroundCallCard::entityChanged',
            'HANDLER' => '**your_handler_url_here**'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Your Learning

- [{#T}](./index.md)
- [{#T}](../card.md)