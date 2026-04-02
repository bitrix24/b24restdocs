# Update Content of the Block landing.block.updatecontent

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.block.updatecontent` completely replaces the HTML content of a block in the draft of the page.

If you only need to change specific nodes, attributes, or styles of the block, use the methods [landing.block.updatenodes](./landing-block-update-nodes.md), [landing.block.updateattrs](./landing-block-update-attrs.md), and [landing.block.updateStyles](./landing-block-update-styles.md).

## Method Parameters

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***  
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the method [landing.landing.getList](../../page/methods/landing-landing-get-list.md) ||
|| **block***  
[`integer`](../../../data-types.md) | Block identifier in the draft of the page.

The block identifier can be obtained using the method [landing.block.getlist](./landing-block-get-list.md) with the parameter `params.edit_mode = 1`.

If you pass the block identifier from the published version of the page, the method may return an error ||
|| **content***  
[`string`](../../../data-types.md) | New HTML content of the block.

The method completely replaces the current content of the block. The current HTML of the block can be obtained using the method [landing.block.getcontent](./landing-block-get-content.md) or the method [landing.block.getlist](./landing-block-get-list.md) with the parameter `params.get_content = true` ||
|| **designed**  
[`boolean`](../../../data-types.md) \| [`integer`](../../../data-types.md) \| [`string`](../../../data-types.md) | Marks the block as manually modified.

To enable this flag, pass `true`, `"true"` or `1`. Any other value will not enable it. Default is `false` ||
|| **preventHistory**  
[`boolean`](../../../data-types.md) | Do not add the change to the page history. Default is `false` ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Accept: application/json" \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 311,
        "block": 6058,
        "content": "<section class=\"g-pt-60 g-pb-60\"><div class=\"container\"><h2 class=\"landing-block-node-title\">Spring Promotion</h2><p class=\"landing-block-node-text\" bxstyle=\"color:#1d1d1d;\">15% discount until the end of the month</p></div></section>",
        "preventHistory": true
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.updatecontent.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Accept: application/json" \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 311,
        "block": 6058,
        "content": "<section class=\"g-pt-60 g-pb-60\"><div class=\"container\"><h2 class=\"landing-block-node-title\">Spring Promotion</h2><p class=\"landing-block-node-text\" bxstyle=\"color:#1d1d1d;\">15% discount until the end of the month</p></div></section>",
        "preventHistory": true,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.updatecontent.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.updatecontent',
    		{
    			lid: 311,
    			block: 6058,
    			content: '<section class="g-pt-60 g-pb-60"><div class="container"><h2 class="landing-block-node-title">Spring Promotion</h2><p class="landing-block-node-text" bxstyle="color:#1d1d1d;">15% discount until the end of the month</p></div></section>',
    			preventHistory: true
    		}
    	);

    	const result = response.getData().result;
    	console.info('Block update result:', result);
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
                'landing.block.updatecontent',
                [
                    'lid' => 311,
                    'block' => 6058,
                    'content' => '<section class="g-pt-60 g-pb-60"><div class="container"><h2 class="landing-block-node-title">Spring Promotion</h2><p class="landing-block-node-text" bxstyle="color:#1d1d1d;">15% discount until the end of the month</p></div></section>',
                    'preventHistory' => true,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Block update result: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating block content: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.updatecontent',
        {
            lid: 311,
            block: 6058,
            content: '<section class="g-pt-60 g-pb-60"><div class="container"><h2 class="landing-block-node-title">Spring Promotion</h2><p class="landing-block-node-text" bxstyle="color:#1d1d1d;">15% discount until the end of the month</p></div></section>',
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
                console.info('Block update result:', result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.block.updatecontent',
        [
            'lid' => 311,
            'block' => 6058,
            'content' => '<section class="g-pt-60 g-pb-60"><div class="container"><h2 class="landing-block-node-title">Spring Promotion</h2><p class="landing-block-node-text" bxstyle="color:#1d1d1d;">15% discount until the end of the month</p></div></section>',
            'preventHistory' => true,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1774524519,
        "finish": 1774524519.331829,
        "duration": 0.3318290710449219,
        "processing": 0,
        "date_start": "2026-03-26T14:28:39+02:00",
        "date_finish": "2026-03-26T14:28:39+02:00",
        "operating_reset_at": 1774525119,
        "operating": 0
    }
}
```

If you pass `designed = true` for a block where custom design cannot be used, the method will return the following response:

HTTP Status: **200**

```json
{
    "result": null,
    "time": {
        "start": 1774524519,
        "finish": 1774524519.102341,
        "duration": 0.1023409366607666,
        "processing": 0,
        "date_start": "2026-03-26T14:28:39+02:00",
        "date_finish": "2026-03-26T14:28:39+02:00",
        "operating_reset_at": 1774525119,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) \| `null` | Result of the block update. The method returns `true` if the call was successful. 

If the method is called with `designed = true` for a block where custom design is not available, it will return `null` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "BLOCK_NOT_FOUND",
    "error_description": "Block not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid`, `block`, or `content` is missing ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found or not accessible to the current user ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found on page `lid` or not available in the draft ||
|| `ACCESS_DENIED` | Insufficient permissions to edit the site ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-get-content.md)
- [{#T}](./landing-block-update-nodes.md)
- [{#T}](./landing-block-update-attrs.md)
- [{#T}](./landing-block-update-styles.md)
- [{#T}](./landing-block-get-manifest.md)
- [{#T}](./landing-block-get-list.md)
- [{#T}](../../page/methods/landing-landing-publication.md)