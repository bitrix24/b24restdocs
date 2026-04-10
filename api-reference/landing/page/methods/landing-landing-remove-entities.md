# Remove Blocks and Clear Image File Bindings on Page landing.landing.removeEntities

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.landing.removeEntities` removes specified blocks and their associated images from the page. It can also clear file bindings for individual images without deleting the blocks themselves.

## Method Parameters

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the method [landing.landing.getList](./landing-landing-get-list.md), as well as from the results of the methods [landing.landing.add](./landing-landing-add.md), [landing.landing.addByTemplate](./landing-landing-add-by-template.md), and [landing.landing.copy](./landing-landing-copy.md) ||
|| **data***
[`object`](../../../data-types.md) | A set of objects to be removed [(detailed description)](#data).

If no blocks or images are specified for removal, the page will remain unchanged. However, the method will still return `true` if the page exists ||
|#

### Parameter data {#data}

#|
|| **Name**
`type` | **Description** ||
|| **blocks**
[`integer[]`](../../../data-types.md) | Identifiers of the page blocks to be completely removed.

For each block, the method also deletes all associated images.

If the list includes blocks that are not present on the page or blocks that the user cannot delete, the method will skip them. For an existing accessible page, it may return `true`, even if some or all blocks from the list were not removed ||
|| **images**
[`object[]`](../../../data-types.md) | Pairs of block and image identifiers for removing file bindings. The content of the block remains unchanged — use this when the image has already been removed from the block and you need to clear the remaining service record. [(detailed description)](#images).

The method will not return a separate error in three cases: if the block is not found, if it is already specified in the `blocks` parameter, or if the image is not associated with this block ||
|#

### Parameter images {#images}

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **block***
[`integer`](../../../data-types.md) | Identifier of the block associated with the image file binding ||
|| **image***
[`integer`](../../../data-types.md) | Internal identifier of the image file binding (`FILE_ID`) associated with the block `block`.

For existing images, `FILE_ID` can be obtained using the method [landing.block.getcontent](../../block/methods/landing-block-get-content.md). In the response, you need to find the HTML block in the `content` field and check the value of the `data-fileid` attribute for the desired image
||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

This example demonstrates a mixed scenario: blocks from `blocks` are completely removed, while `images` elements clear file bindings for images in other blocks.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 648,
        "data": {
          "blocks": [12167, 123],
          "images": [
            {
              "block": 12269,
              "image": 6866
            },
            {
              "block": 12268,
              "image": 6861
            }
          ]
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.removeEntities.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 648,
        "data": {
          "blocks": [12167, 123],
          "images": [
            {
              "block": 12269,
              "image": 6866
            },
            {
              "block": 12268,
              "image": 6861
            }
          ]
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.removeEntities.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.removeEntities',
    		{
    			lid: 648,
    			data: {
    				blocks: [12167, 123],
    				images: [
    					{
    						block: 12269,
    						image: 6866
    					},
    					{
    						block: 12268,
    						image: 6861
    					}
    				]
    			}
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
                'landing.landing.removeEntities',
                [
                    'lid' => 648,
                    'data' => [
                        'blocks' => [12167, 123],
                        'images' => [
                            [
                                'block' => 12269,
                                'image' => 6866,
                            ],
                            [
                                'block' => 12268,
                                'image' => 6861,
                            ],
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error removing blocks and images: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.removeEntities',
        {
            lid: 648,
            data: {
                blocks: [12167, 123],
                images: [
                    {
                        block: 12269,
                        image: 6866
                    },
                    {
                        block: 12268,
                        image: 6861
                    }
                ]
            }
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
        'landing.landing.removeEntities',
        [
            'lid' => 648,
            'data' => [
                'blocks' => [12167, 123],
                'images' => [
                    [
                        'block' => 12269,
                        'image' => 6866,
                    ],
                    [
                        'block' => 12268,
                        'image' => 6861,
                    ],
                ],
            ],
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
        "start": 1773796328,
        "finish": 1773796328.413521,
        "duration": 0.41352105140686035,
        "processing": 0,
        "date_start": "2026-03-18T04:12:08+01:00",
        "date_finish": "2026-03-18T04:12:08+01:00",
        "operating_reset_at": 1773796928,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the removal, returns `true` on success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "LANDING_NOT_EXIST",
    "error_description": "Landing not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameters are not provided: the request is missing `lid`, `data`, or both parameters ||
|| `LANDING_NOT_EXIST` | The page is not found, deleted, or inaccessible to the current user ||
|| `TYPE_ERROR` | Data type error in the method call parameters ||
|| `SYSTEM_ERROR` | Internal error during method execution ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-delete.md)
- [{#T}](./landing-landing-mark-delete.md)
- [{#T}](./landing-landing-mark-undelete.md)
- [{#T}](./landing-landing-move.md)
- [{#T}](./landing-landing-publication.md)
- [{#T}](./landing-landing-update.md)