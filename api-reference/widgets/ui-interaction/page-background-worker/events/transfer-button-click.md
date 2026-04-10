# When selecting an operator to whom the current operator wants to transfer the call BackgroundCallCard::transferButtonClick

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `BackgroundCallCard::transferButtonClick` is triggered when a recipient for the call transfer is selected.

{% note info "" %}

The event operates within the context of the application in the placement `PAGE_BACKGROUND_WORKER`.

{% endnote %}

## What the handler receives

Data is passed to the callback `BX24.placement.bindEvent` {.b24-info}

```js
callback({
    "phoneNumber": "+19001234567",
    "target": "12"
});
```

## Event handler parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **phoneNumber***
[`string`](../../../../data-types.md) | The number of the current call ||
|| **target***
[`string`](../../../../data-types.md) | The identifier of the target user for the call transfer ||
|#

## Subscription parameters for the event

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT***
[`string`](../../../../data-types.md) | The name of the interface event.

For this event — `BackgroundCallCard::transferButtonClick` ||
|| **HANDLER***
[`string`](../../../../data-types.md) | The URL of the event handler to call `placement.bindEvent` ||
|#

## Code examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"BackgroundCallCard::transferButtonClick","HANDLER":"**your_handler_url_here**"}' \
    "https://**put_your_bitrix24_address.com**/rest/placement.bindEvent?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.bindEvent('BackgroundCallCard::transferButtonClick', function (eventData) {
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
                    'PLACEMENT' => 'BackgroundCallCard::transferButtonClick',
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
            PLACEMENT: 'BackgroundCallCard::transferButtonClick',
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
            'PLACEMENT' => 'BackgroundCallCard::transferButtonClick',
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