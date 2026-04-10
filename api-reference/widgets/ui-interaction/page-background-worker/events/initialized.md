# After Creating the Call Card BackgroundCallCard::initialized

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `BackgroundCallCard::initialized` occurs after the call card is created and the initial data is transmitted.

{% note info "" %}

The event operates within the application context in the placement `PAGE_BACKGROUND_WORKER`.

{% endnote %}

## What the Handler Receives

Data is passed to the callback `BX24.placement.bindEvent` {.b24-info}

```js
callback({
    "CALL_ID": "E45D40253D1C2D2F.1774588815.822533",
    "PHONE_NUMBER": "+19001234567",
    "LINE_NUMBER": "reg151083",
    "LINE_NAME": "",
    "CRM_ENTITY_TYPE": "CONTACT",
    "CRM_ENTITY_ID": 123,
    "CRM_ACTIVITY_ID": 456,
    "CRM_BINDINGS": [{"ENTITY_TYPE": "DEAL", "ENTITY_ID": 789}],
    "CALL_DIRECTION": "outgoing",
    "CALL_STATE": "idle",
    "CALL_LIST_MODE": false
});
```

## Event Handler Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **CALL_ID**
[`string`](../../../../data-types.md) | Call identifier ||
|| **PHONE_NUMBER**
[`string`](../../../../data-types.md) | Client's phone number ||
|| **LINE_NUMBER**
[`string`](../../../../data-types.md) | Line number ||
|| **LINE_NAME**
[`string`](../../../../data-types.md) | Line name ||
|| **CRM_ENTITY_TYPE**
[`string`](../../../../data-types.md) | Type of the current CRM object ||
|| **CRM_ENTITY_ID**
[`integer`](../../../../data-types.md) | Identifier of the current CRM object ||
|| **CRM_ACTIVITY_ID**
[`integer`](../../../../data-types.md) | Identifier of the CRM activity ||
|| **CRM_BINDINGS**
[`object[]`](../../../../data-types.md) | Call bindings to CRM objects [(detailed description)](#crm_bindings) ||
|| **CALL_DIRECTION**
[`string`](../../../../data-types.md) | Call direction ||
|| **CALL_STATE**
[`string`](../../../../data-types.md) | Call state ||
|| **CALL_LIST_MODE**
[`boolean`](../../../../data-types.md) | Indicator of the dialing mode ||
|#

### Parameter CRM_BINDINGS{#crm_bindings}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE**
[`string`](../../../../data-types.md) | Type of the CRM object ||
|| **ENTITY_ID**
[`integer`](../../../../data-types.md) | Identifier of the CRM object ||
|#

## Event Subscription Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT*** 
[`string`](../../../../data-types.md) | Name of the interface event.

For this event — `BackgroundCallCard::initialized` ||
|| **HANDLER*** 
[`string`](../../../../data-types.md) | URL of the event handler for calling `placement.bindEvent` ||
|#

## Code Examples

{% include [Note on Examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"BackgroundCallCard::initialized","HANDLER":"**your_handler_url_here**"}' \
    "https://**put_your_bitrix24_address**/rest/placement.bindEvent?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.bindEvent('BackgroundCallCard::initialized', function (eventData) {
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
                    'PLACEMENT' => 'BackgroundCallCard::initialized',
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
            PLACEMENT: 'BackgroundCallCard::initialized',
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
            'PLACEMENT' => 'BackgroundCallCard::initialized',
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