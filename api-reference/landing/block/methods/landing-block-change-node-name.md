# Change HTML Tag of a Block Element landing.block.changeNodeName

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site containing the page

The method `landing.block.changeNodeName` changes the HTML tag of a block element in the draft of a page.

If the page is already published, the changes will be visible to visitors after re-publishing through the interface or using the method [landing.landing.publication](../../page/methods/landing-landing-publication.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) | Page identifier

The page identifier can be obtained using the method [landing.landing.getList](../../page/methods/landing-landing-get-list.md) ||
|| **block***
[`integer`](../../../data-types.md) | Block identifier in the draft of the page

The block identifier can be obtained using the method [landing.block.getlist](./landing-block-get-list.md) with the parameter `params.edit_mode = 1`.

Use the identifier specifically from the draft of the page. If you pass the block identifier from the published version or another version of the page, the changes will not be applied ||
|| **data***
[`object`](../../../data-types.md) | Set of changes for block elements [(detailed description)](#data) ||
|| **preventHistory**
[`boolean`](../../../data-types.md) | If `true` is passed, the method will not add the action to the page change history

By default, it is `false` ||
|#

### Parameter data {#data}

The `data` parameter is passed in the format:

```
{
    selector_1: tag_1,
    selector_2: tag_2,
    ...,
    selector_n: tag_n
}
```

where:
- `selector_n` — selector of the element from the block manifest
- `tag_n` — new HTML tag name

#|
|| **Key**
`type` | **Description** ||
|| **<selector>**
[`string`](../../../data-types.md) | New HTML tag name for the element specified in the key

The key must match the selector of the element from the block manifest

For repeating elements, you can specify the position after the selector using `@`, for example, `.landing-block-node-title@1`. Positions are numbered starting from `0`.

If the position is not specified, the method will change the first found element, which means it will work the same as `@0`.

If you pass a selector that does not exist in the block manifest, or specify a position that does not exist in the block, the method will complete without error but will not change anything ||
|#

### Allowed Tag Values {#tag-values}

The tag value is passed as a string. Leading and trailing spaces are removed, and case is ignored.

#|
|| **Value** | **Description** ||
|| `h1` | First-level heading ||
|| `h2` | Second-level heading ||
|| `h3` | Third-level heading ||
|| `h4` | Fourth-level heading ||
|| `h5` | Fifth-level heading ||
|| `h6` | Sixth-level heading ||
|| `div` | Block container ||
|| `p` | Paragraph ||
|| `a` | Link ||
|| `span` | Inline container ||
|#

For example, if you pass ` H1 `, the method will save the tag as `h1`. If another value is passed, the method will use `div`.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 311,
        "block": 6058,
        "data": {
          ".landing-block-node-title@0": "h1",
          ".landing-block-node-text@2": "p"
        },
        "preventHistory": true
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.changeNodeName.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 311,
        "block": 6058,
        "data": {
          ".landing-block-node-title@0": "h1",
          ".landing-block-node-text@2": "p"
        },
        "preventHistory": true,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.changeNodeName.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.changeNodeName',
    		{
    			lid: 311,
    			block: 6058,
    			data: {
    				'.landing-block-node-title@0': 'h1',
    				'.landing-block-node-text@2': 'p'
    			},
    			preventHistory: true
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
                'landing.block.changeNodeName',
                [
                    'lid' => 311,
                    'block' => 6058,
                    'data' => [
                        '.landing-block-node-title@0' => 'h1',
                        '.landing-block-node-text@2' => 'p',
                    ],
                    'preventHistory' => true,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error changing node name: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.changeNodeName',
        {
            lid: 311,
            block: 6058,
            data: {
                '.landing-block-node-title@0': 'h1',
                '.landing-block-node-text@2': 'p'
            },
            preventHistory: true
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
        'landing.block.changeNodeName',
        [
            'lid' => 311,
            'block' => 6058,
            'data' => [
                '.landing-block-node-title@0' => 'h1',
                '.landing-block-node-text@2' => 'p',
            ],
            'preventHistory' => true,
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
        "start": 1774510990,
        "finish": 1774510990.1045,
        "duration": 0.10450005531311035,
        "processing": 0,
        "date_start": "2026-03-26T10:43:10+01:00",
        "date_finish": "2026-03-26T10:43:10+01:00",
        "operating_reset_at": 1774511590,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the tag change. If the request is successful, the method returns `true` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| `MISSING_PARAMS` | Required parameter `lid`, `block`, or `data` is missing ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found or not accessible to the current user ||
|| `ACCESS_DENIED` | Insufficient permissions to edit the site ||
|| `TYPE_ERROR` | Parameter `data` is passed in an incorrect format or tag value is not a string ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-update-nodes.md)
- [{#T}](./landing-block-change-anchor.md)
- [{#T}](./landing-block-get-list.md)
- [{#T}](./landing-block-get-manifest.md)