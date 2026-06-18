# On Clicking the "Next" Button: BackgroundCallCard::nextButtonClick

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `BackgroundCallCard::nextButtonClick` is triggered when moving to the next client in the dialing mode.

{% note info "" %}

The event operates within the context of the application in the placement `PAGE_BACKGROUND_WORKER`.

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

For this event — `BackgroundCallCard::nextButtonClick` ||
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
    -d '{"PLACEMENT":"BackgroundCallCard::nextButtonClick","HANDLER":"**your_handler_url_here**"}' \
    "https://**put_your_bitrix24_address**/rest/placement.bindEvent?auth=**put_access_token_here**"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'placement.bindEvent',
        params: {
          PLACEMENT: 'BackgroundCallCard::nextButtonClick',
          HANDLER: '**your_handler_url_here**',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Event bound successfully:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function bindNextButtonClick() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.bindEvent',
            params: {
              PLACEMENT: 'BackgroundCallCard::nextButtonClick',
              HANDLER: '**your_handler_url_here**',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Event bound successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', bindNextButtonClick)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'placement.bindEvent',
                [
                    'PLACEMENT' => 'BackgroundCallCard::nextButtonClick',
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
            PLACEMENT: 'BackgroundCallCard::nextButtonClick',
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
            'PLACEMENT' => 'BackgroundCallCard::nextButtonClick',
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
