# Remove Card from Block landing.block.removecard

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.block.removecard` removes a card from a block in the draft of a page.

This method only works with cards described in the `cards` key of the block manifest. If the page is already published, the change will be visible to visitors after publication through the interface or by using the [landing.landing.publication](../../page/methods/landing-landing-publication.md) method.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of landings. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **lid*** 
[`integer`](../../../data-types.md) | Identifier of the page.

The page identifier can be obtained using the [landing.landing.getlist](../../page/methods/landing-landing-get-list.md) method ||
|| **block*** 
[`integer`](../../../data-types.md) | Identifier of the block in the editable version of the page.

The block identifier can be obtained using the [landing.block.getlist](./landing-block-get-list.md) method with the parameter `params.edit_mode = 1`. If you pass the block identifier from the published version of the page, the method may return an error ||
|| **selector*** 
[`string`](../../../data-types.md) | Selector of the card from the [cards section of the block manifest](../manifest.md#key-cards)

The method searches for cards using this selector and removes the one whose index is specified after `@<index>`. The index is counted only among the found cards. The numbering starts from `0`: 
- `.landing-block-card@0` removes the first found card, 
- `.landing-block-card@2` removes the third found card.

If the index is not specified and only `.landing-block-card` is passed, the method will return an error. If a blank or non-numeric value is specified after `@`, the method will interpret it as `0` and attempt to remove the first card.

The method will also return an error if the selector is not present in the manifest, if there are no cards in the block with that selector, or if the index is out of bounds of the list ||
|| **preventHistory**
[`boolean`](../../../data-types.md) | Do not add the action to the page change history.

Possible values:
`true` - do not save the action in the change history,
`false` - save the action in the change history.

Default is `false` ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "block": 6428,
        "selector": ".landing-block-card@0"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.removecard.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "block": 6428,
        "selector": ".landing-block-card@0",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.removecard.json"
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
        method: 'landing.block.removecard',
        params: {
          lid: 351,
          block: 6428,
          selector: '.landing-block-card@0',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Card removed:', result)
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
      async function removeBlockCard() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.block.removecard',
            params: {
              lid: 351,
              block: 6428,
              selector: '.landing-block-card@0',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Card removed:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', removeBlockCard)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.block.removecard',
                [
                    'lid' => 351,
                    'block' => 6428,
                    'selector' => '.landing-block-card@0',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error removing block card: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.removecard',
        {
            lid: 351,
            block: 6428,
            selector: '.landing-block-card@0'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.block.removecard',
        [
            'lid' => 351,
            'block' => 6428,
            'selector' => '.landing-block-card@0',
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo '<pre>';
        print_r($result['result']);
        echo '</pre>';
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1774505673,
        "finish": 1774505673.578092,
        "duration": 0.578092098236084,
        "processing": 0,
        "date_start": "2026-03-26T09:14:33+01:00",
        "date_finish": "2026-03-26T09:14:33+01:00",
        "operating_reset_at": 1774506273,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the card removal. Returns `true` upon successful execution ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CARD_NOT_FOUND",
    "error_description": "Card not found in the block"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid`, `block`, or `selector` is missing ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found or not accessible to the current user ||
|| `ACCESS_DENIED` | User does not have permission to edit the page and block ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found on page `lid` or not accessible in the editable version of the page ||
|| `CARD_NOT_FOUND` | No card found in the block with the selector `selector`. This error is returned if the position is not specified, the selector is not in `manifest.cards`, no cards are found for it, or the index is out of bounds of the found cards ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-clone-card.md)
- [{#T}](./landing-block-add-card.md)
- [{#T}](./landing-block-update-cards.md)
- [{#T}](../../page/methods/landing-landing-publication.md)