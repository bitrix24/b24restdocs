# Hide block on page `landing.landing.hideblock`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for the site

The method `landing.landing.hideblock` hides a block on the page. The hidden block remains in the draft of the page and can be shown again using the method [landing.landing.showblock](./landing-landing-show-block.md).

If the page is already published, the change will be visible to visitors after the "Publish changes" command in the interface or after calling the method [landing.landing.publication](../methods/landing-landing-publication.md).

## Method parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the method [landing.landing.getList](../methods/landing-landing-get-list.md), as well as from the results of the methods [landing.landing.add](../methods/landing-landing-add.md), [landing.landing.addByTemplate](../methods/landing-landing-add-by-template.md), and [landing.landing.copy](../methods/landing-landing-copy.md) ||
|| **block***
[`integer`](../../../data-types.md) | Block identifier.

The block identifier can be obtained using the method [landing.block.getList](../../block/methods/landing-block-get-list.md) with the parameter `params.edit_mode = 1` ||
|#

## Code examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "block": 6428
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.hideblock.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "block": 6428,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.hideblock.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.landing.hideblock',
            {
                lid: 351,
                block: 6428
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
                'landing.landing.hideblock',
                [
                    'lid' => 351,
                    'block' => 6428,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error hiding block: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.hideblock',
        {
            lid: 351,
            block: 6428
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
        'landing.landing.hideblock',
        [
            'lid' => 351,
            'block' => 6428,
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

## Response handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773969024,
        "finish": 1773969024.227401,
        "duration": 0.2274010181427002,
        "processing": 0,
        "date_start": "2026-03-20T04:10:24+02:00",
        "date_finish": "2026-03-20T04:10:24+02:00",
        "operating_reset_at": 1773969624,
        "operating": 0
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of hiding, returns `true` on success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error handling

HTTP status: **400**

```json
{
    "error": "BLOCK_NOT_FOUND",
    "error_description": "Block not found in the landing"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible error codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required top-level parameter `lid` or `block` is missing ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found or not accessible to the current user ||
|| `ACCESS_DENIED` | Insufficient rights to modify the block ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found on page `lid`, already marked as deleted or not accessible ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue exploring

- [{#T}](./landing-landing-add-block.md)
- [{#T}](./landing-landing-copy-block.md)
- [{#T}](./landing-landing-delete-block.md)
- [{#T}](./landing-landing-down-block.md)
- [{#T}](./landing-landing-mark-deleted-block.md)
- [{#T}](./landing-landing-mark-undeleted-block.md)
- [{#T}](./landing-landing-move-block.md)
- [{#T}](./landing-landing-show-block.md)
- [{#T}](./landing-landing-up-block.md)
- [{#T}](../methods/landing-landing-publication.md)