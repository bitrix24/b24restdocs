# Add Card to Block landing.block.addcard

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.block.addcard` adds a card to a block in the draft of a page.

This method works with cards described in the `cards` key of the block manifest. If the page is already published, changes will be visible to visitors after re-publishing through the interface or using the method [landing.landing.publication](../../page/methods/landing-landing-publication.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the method [landing.landing.getlist](../../page/methods/landing-landing-get-list.md) ||
|| **block***
[`integer`](../../../data-types.md) | Block identifier in the editable version of the page.

The block identifier can be obtained using the method [landing.block.getlist](./landing-block-get-list.md) with the parameter `params.edit_mode = 1` ||
|| **selector***
[`string`](../../../data-types.md) | Selector of the card from the [key `cards` of the block manifest](../manifest.md#key-cards)

After the selector, you can specify an index using `@<index>`. For example, `@0` will add a new card after the first found one, while `@2` will add it after the third.

If an index is specified, the method inserts the new card after the selected one. If this card is the last one, the new card will appear at the end of the container. If no index is specified, the method takes the first card matching this selector and inserts the new card at the beginning of the container.

After adding a card, the indices change. If you are adding multiple cards in succession, specify a new index in `selector` for each subsequent call.

If the selector is not present in the manifest, if there are no cards in the block for this selector, or if a non-existent index is specified, the method will return an error.

For more details on card selectors and the structure of the manifest, refer to the article [Block Manifest](../manifest.md#key-cards) ||
|| **content***
[`string`](../../../data-types.md) | HTML of the new card.

If you need to use the existing markup of the block as a basis, obtain the HTML using the method [landing.block.getcontent](./landing-block-get-content.md) and use the card structure from the current block.

If an empty string is passed or the content becomes empty after filtering, the method will behave like [landing.block.clonecard](./landing-block-clone-card.md) and create a copy of the card specified in `selector` ||
|| **preventHistory**
[`boolean`](../../../data-types.md) | Do not add the action to the page change history.

Possible values:
`true` - do not save the action in history,
`false` - save the action in history.

Default is `false` ||
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
        "selector": ".landing-block-node-menu-list-item@0",
        "content": "<li class=\"landing-block-node-menu-list-item nav-item\"><a href=\"#services\" class=\"landing-block-node-menu-list-item-link nav-link\">Services</a></li>"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.addcard.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "block": 6428,
        "selector": ".landing-block-node-menu-list-item@0",
        "content": "<li class=\"landing-block-node-menu-list-item nav-item\"><a href=\"#services\" class=\"landing-block-node-menu-list-item-link nav-link\">Services</a></li>",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.addcard.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.block.addcard',
            {
                lid: 351,
                block: 6428,
                selector: '.landing-block-node-menu-list-item@0',
                content: '<li class="landing-block-node-menu-list-item nav-item"><a href="#services" class="landing-block-node-menu-list-item-link nav-link">Services</a></li>'
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
                'landing.block.addcard',
                [
                    'lid' => 351,
                    'block' => 6428,
                    'selector' => '.landing-block-node-menu-list-item@0',
                    'content' => '<li class="landing-block-node-menu-list-item nav-item"><a href="#services" class="landing-block-node-menu-list-item-link nav-link">Services</a></li>',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding block card: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.addcard',
        {
            lid: 351,
            block: 6428,
            selector: '.landing-block-node-menu-list-item@0',
            content: '<li class="landing-block-node-menu-list-item nav-item"><a href="#services" class="landing-block-node-menu-list-item-link nav-link">Services</a></li>'
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
        'landing.block.addcard',
        [
            'lid' => 351,
            'block' => 6428,
            'selector' => '.landing-block-node-menu-list-item@0',
            'content' => '<li class="landing-block-node-menu-list-item nav-item"><a href="#services" class="landing-block-node-menu-list-item-link nav-link">Services</a></li>',
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
        "start": 1774524862,
        "finish": 1774524862.61318,
        "duration": 0.6131799221038818,
        "processing": 0,
        "date_start": "2026-03-26T14:34:22+01:00",
        "date_finish": "2026-03-26T14:34:22+01:00",
        "operating_reset_at": 1774525462,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of adding the card. Returns `true` on successful execution; the identifier of the new card is not returned in the response ||
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
|| `MISSING_PARAMS` | Required parameter `lid`, `block`, `selector`, or `content` is missing ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found or not accessible to the current user ||
|| `ACCESS_DENIED` | User does not have permission to edit the site ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found on page `lid` or not accessible in the draft of the page ||
|| `CARD_NOT_FOUND` | No card found in the block for the selector `selector` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-clone-card.md)
- [{#T}](./landing-block-remove-card.md)
- [{#T}](./landing-block-update-cards.md)
- [{#T}](./landing-block-get-content.md)
- [{#T}](../../page/methods/landing-landing-publication.md)