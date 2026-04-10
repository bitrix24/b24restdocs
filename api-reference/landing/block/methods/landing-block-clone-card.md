# Clone Card Method: landing.block.clonecard

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.block.clonecard` creates a copy of a card within a block in the draft of a page. It operates on cards described in the `cards` key of the block manifest.

If the page is already published, the changes will be visible to visitors after publication through the interface or by using the [landing.landing.publication](../../page/methods/landing-landing-publication.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the [landing.landing.getlist](../../page/methods/landing-landing-get-list.md) method. ||
|| **block***
[`integer`](../../../data-types.md) | Block identifier in the draft of the page.

The block identifier can be obtained using the [landing.block.getlist](./landing-block-get-list.md) method with the parameter `params.edit_mode = 1`. ||
|| **selector***
[`string`](../../../data-types.md) | Card selector from the [cards key of the block manifest](../manifest.md#key-cards).

After the selector, you can specify the position using `@<index>`. `@0` copies the first found card, `@2` — the third.

When an index is specified, the method copies the selected card and inserts the copy immediately after it. For the last card, the copy is added to the end of the container. Without an index, the method takes the first card matching this selector and inserts the copy at the beginning of the container.

If the selector is not present in the manifest or a matching card is not found, the method will return an error. ||
|| **preventHistory**
[`boolean`](../../../data-types.md) | Do not add the action to the page change history.

Possible values:
`true` - do not save the action in history
`false` - save the action in history

Default is `false`. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

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
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.clonecard.json"
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
      "https://**put.your-domain-here**/rest/landing.block.clonecard.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.block.clonecard',
            {
                lid: 351,
                block: 6428,
                selector: '.landing-block-card@0'
            }
        );

        const result = response.getData().result;
        console.info(result);
    }
    catch (error)
    {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.block.clonecard',
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
        echo 'Error cloning block card: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.clonecard',
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
        'landing.block.clonecard',
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
        "start": 1774477875,
        "finish": 1774477875.611452,
        "duration": 0.6114521026611328,
        "processing": 0,
        "date_start": "2026-03-26T01:31:15+01:00",
        "date_finish": "2026-03-26T01:31:15+01:00",
        "operating_reset_at": 1774478475,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the card cloning.

Returns `true` upon successful execution. The identifier of the new card is not included in the response. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
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
|| `MISSING_PARAMS` | Required parameter `lid`, `block`, or `selector` is missing. ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found or not accessible to the current user. ||
|| `ACCESS_DENIED` | User does not have permission to edit the page. ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found on page `lid` or not accessible in the draft of the page. ||
|| `CARD_NOT_FOUND` | No card found in the block for the selector `selector`. This error is returned if the selector is absent in `manifest.cards` or the card with the specified selector and index is not found. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-remove-card.md)
- [{#T}](./landing-block-add-card.md)
- [{#T}](./landing-block-update-cards.md)
- [{#T}](../../page/methods/landing-landing-publication.md)