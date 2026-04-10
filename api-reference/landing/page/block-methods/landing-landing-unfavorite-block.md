# Remove from the saved list of blocks `landing.landing.unFavoriteBlock`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for the site

The method `landing.landing.unFavoriteBlock` removes the saved copy of a block. The original block on the page remains unchanged. When a saved block is deleted, any associated files, such as a user-uploaded preview for that block, are also removed.

You can only delete a saved block that the current user created using the method [landing.landing.favoriteBlock](./landing-landing-favorite-block.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **blockId***
[`integer`](../../../data-types.md) | Identifier of the saved copy of the block.

The identifier can be obtained from the result of the method [landing.landing.favoriteBlock](./landing-landing-favorite-block.md).

If you pass the identifier of a regular block on the page or a non-existent identifier, the method will return an error ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "blockId": 28619
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.unFavoriteBlock.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "blockId": 28619,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.unFavoriteBlock.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.landing.unFavoriteBlock',
            {
                blockId: 28619
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
                'landing.landing.unFavoriteBlock',
                [
                    'blockId' => 28619,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error removing block from the block list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.unFavoriteBlock',
        {
            blockId: 28619
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
        'landing.landing.unFavoriteBlock',
        [
            'blockId' => 28619,
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

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773978259,
        "finish": 1773978259.693505,
        "duration": 0.693505048751831,
        "processing": 0,
        "date_start": "2026-03-20T06:44:19+02:00",
        "date_finish": "2026-03-20T06:44:19+02:00",
        "operating_reset_at": 1773978859,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of removing the saved block. Returns `true` on successful execution ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Operation is forbidden"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `blockId` is missing ||
|| `TYPE_ERROR` | Incorrect type passed in parameter `blockId` ||
|| `BLOCK_NOT_FOUND` | Block with identifier `blockId` does not exist or is not a block from the "My blocks" list ||
|| `ACCESS_DENIED` | Insufficient rights to delete the saved block ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add-block.md)
- [{#T}](./landing-landing-copy-block.md)
- [{#T}](./landing-landing-delete-block.md)
- [{#T}](./landing-landing-down-block.md)
- [{#T}](./landing-landing-favorite-block.md)
- [{#T}](./landing-landing-hide-block.md)
- [{#T}](./landing-landing-mark-deleted-block.md)
- [{#T}](./landing-landing-mark-undeleted-block.md)
- [{#T}](./landing-landing-move-block.md)
- [{#T}](./landing-landing-show-block.md)
- [{#T}](./landing-landing-up-block.md)