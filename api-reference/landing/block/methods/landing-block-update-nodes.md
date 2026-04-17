# Update Nodes of the Block landing.block.updatenodes

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.block.updatenodes` updates the nodes of a block in the draft of a page. If the page is already published, changes will be visible to visitors after re-publishing through the interface or using the method [landing.landing.publication](../../page/methods/landing-landing-publication.md).

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of the landing. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **lid*** 
[`integer`](../../../data-types.md) | Identifier of the page.

The page identifier can be obtained using the method [landing.landing.getList](../../page/methods/landing-landing-get-list.md) ||
|| **block*** 
[`integer`](../../../data-types.md) | Identifier of the block in the version of the page for editing.

The block identifier can be obtained using the method [landing.block.getlist](./landing-block-get-list.md) with the parameter `params.edit_mode = 1`. If you pass the block identifier from the published version of the page, the method may return an error ||
|| **data*** 
[`object`](../../../data-types.md) | Set of changes for the block nodes and component parameters available for editing [(detailed description)](#data) ||
|| **additional**
[`object`](../../../data-types.md) | Additional save parameters [(detailed description)](#additional) ||
|| **preventHistory**
[`boolean`](../../../data-types.md) | If set to `true`, the method will not add the action to the page change history. Default is `false` ||
|#

### Parameter data {#data}

#|
|| **Key**
`type` | **Description** ||
|| **<selector>**
[`string`](../../../data-types.md) \| [`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | The key must match the selector from the block manifest.

For repeating elements, you can specify the position using `@`, for example, `.landing-block-node-text@1`. Positions are numbered starting from `0`.

If the position is not specified, the behavior depends on the value format:

- for node values, the method changes the first found element,
- for values of the form `{attrs: {...}}` with a regular selector, the method updates all found elements,
- for component selectors with `:`, for example, `bitrix:catalog.section`, the position is not used.

The value format depends on the node type [(detailed description)](#value-formats) ||
|#

### Value Formats in data {#value-formats}

#|
|| **Node Type** | **Example Value** ||
|| `text` | `'New block text'` or `{text: 'New block text'}` ||
|| `img` | `{src: 'https://example.com/a.jpg', alt: 'Banner', src2x: 'https://example.com/a@2x.jpg'}` ||
|| `link` | `{text: 'Read more', href: 'https://example.com', target: '_blank', query: 'utm_source=test'}` ||
|| `icon` | `['fa', 'fa-telegram']` ||
|| `embed` | `{src: '//youtube.com/embed/123', source: 'https://youtube.com/watch?v=123'}` ||
|#

The formats for other node types are described in the article [Node Types](../node-types.md).

Additional features of the formats:

- `link` supports fields `text`, `href`, `query`, `target`, `skipContent`, and `attrs`
- in `link.attrs`, only the attributes `data-embed` and `data-url` are saved
- if `link.text` is not provided, the method will update `href`, `target`, and allowed attributes, but will not change the link text
- if a link object is provided without `target`, the current value of `target` will be cleared
- if `link.attrs` is not provided or an empty object is passed, the current `data-embed` and `data-url` will be removed
- `img` additionally supports fields `src2x`, `id`, `id2x`, `isLazy`, `lazyOrigSrc`, `lazyOrigSrc2x`, `lazyOrigSrcset`, `lazyOrigStyle`
- for `img`, it is recommended to pass the full image object. If `src` is not provided, the current image address will be cleared. If `src2x` is not provided, the value for the retina version will be cleared
- for `img`, addresses `src` and `src2x` starting with `http://` will be replaced with `https://`
- for `img`, lazy loading is enabled only with the value `isLazy = 'Y'`
- `embed` additionally supports fields `preview` and `ratio`
- for `embed`, it is recommended to pass the full object. If `preview` is not provided, the current preview will be removed
- for `embed`, the `ratio` field is applied only with a new `src`

If a value of the form `{attrs: {...}}` is passed, the method behaves differently depending on the selector:

- for a regular selector, it updates the node attributes,
- for a component selector with `:`, for example, `bitrix:catalog.section`, it updates only those component parameters that are available for editing in the block manifest.

Selectors not present in the block manifest are ignored by the method. The same applies to component parameters that are not among those available for editing.

### Parameter additional {#additional}

#|
|| **Name**
`type` | **Description** ||
|| **appendMenu**
[`boolean`](../../../data-types.md) | Adds new items to the current menu instead of completely replacing it. Works only for blocks that have a `menu` section described in the manifest.

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
        "lid": 311,
        "block": 6058,
        "data": {
          ".landing-block-node-text": "New block text",
          ".landing-block-node-img": {
            "src": "https://cdn.bitrix24.site/bitrix/images/landing/business/1920x1280/img12.jpg",
            "alt": "New banner"
          },
          ".landing-block-node-link": {
            "text": "Read more",
            "href": "https://www.bitrix24.com",
            "target": "_blank"
          },
          ".landing-block-node-icon": [
            "fa",
            "fa-telegram"
          ],
          ".landing-block-node-embed": {
            "src": "//www.youtube.com/embed/q4d8g9Dn3ww?autoplay=1&controls=0&loop=1&mute=1&rel=0",
            "source": "https://www.youtube.com/watch?v=q4d8g9Dn3ww"
          }
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.updatenodes.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 311,
        "block": 6058,
        "data": {
          ".landing-block-node-text": "New block text",
          ".landing-block-node-img": {
            "src": "https://cdn.bitrix24.site/bitrix/images/landing/business/1920x1280/img12.jpg",
            "alt": "New banner"
          },
          ".landing-block-node-link": {
            "text": "Read more",
            "href": "https://www.bitrix24.com",
            "target": "_blank"
          },
          ".landing-block-node-icon": [
            "fa",
            "fa-telegram"
          ],
          ".landing-block-node-embed": {
            "src": "//www.youtube.com/embed/q4d8g9Dn3ww?autoplay=1&controls=0&loop=1&mute=1&rel=0",
            "source": "https://www.youtube.com/watch?v=q4d8g9Dn3ww"
          }
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.updatenodes.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.updatenodes',
    		{
    			lid: 311,
    			block: 6058,
    			data: {
    				'.landing-block-node-text': 'New block text',
    				'.landing-block-node-img': {
    					src: 'https://cdn.bitrix24.site/bitrix/images/landing/business/1920x1280/img12.jpg',
    					alt: 'New banner'
    				},
    				'.landing-block-node-link': {
    					text: 'Read more',
    					href: 'https://www.bitrix24.com',
    					target: '_blank'
    				},
    				'.landing-block-node-icon': ['fa', 'fa-telegram'],
    				'.landing-block-node-embed': {
    					src: '//www.youtube.com/embed/q4d8g9Dn3ww?autoplay=1&controls=0&loop=1&mute=1&rel=0',
    					source: 'https://www.youtube.com/watch?v=q4d8g9Dn3ww'
    				}
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
                'landing.block.updatenodes',
                [
                    'lid' => 311,
                    'block' => 6058,
                    'data' => [
                        '.landing-block-node-text' => 'New block text',
                        '.landing-block-node-img' => [
                            'src' => 'https://cdn.bitrix24.site/bitrix/images/landing/business/1920x1280/img12.jpg',
                            'alt' => 'New banner',
                        ],
                        '.landing-block-node-link' => [
                            'text' => 'Read more',
                            'href' => 'https://www.bitrix24.com',
                            'target' => '_blank',
                        ],
                        '.landing-block-node-icon' => ['fa', 'fa-telegram'],
                        '.landing-block-node-embed' => [
                            'src' => '//www.youtube.com/embed/q4d8g9Dn3ww?autoplay=1&controls=0&loop=1&mute=1&rel=0',
                            'source' => 'https://www.youtube.com/watch?v=q4d8g9Dn3ww',
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
        echo 'Error updating block nodes: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.updatenodes',
        {
            lid: 311,
            block: 6058,
            data: {
                '.landing-block-node-text': 'New block text',
                '.landing-block-node-img': {
                    src: 'https://cdn.bitrix24.site/bitrix/images/landing/business/1920x1280/img12.jpg',
                    alt: 'New banner'
                },
                '.landing-block-node-link': {
                    text: 'Read more',
                    href: 'https://www.bitrix24.com',
                    target: '_blank'
                },
                '.landing-block-node-icon': ['fa', 'fa-telegram'],
                '.landing-block-node-embed': {
                    src: '//www.youtube.com/embed/q4d8g9Dn3ww?autoplay=1&controls=0&loop=1&mute=1&rel=0',
                    source: 'https://www.youtube.com/watch?v=q4d8g9Dn3ww'
                }
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
        'landing.block.updatenodes',
        [
            'lid' => 311,
            'block' => 6058,
            'data' => [
                '.landing-block-node-text' => 'New block text',
                '.landing-block-node-img' => [
                    'src' => 'https://cdn.bitrix24.site/bitrix/images/landing/business/1920x1280/img12.jpg',
                    'alt' => 'New banner',
                ],
                '.landing-block-node-link' => [
                    'text' => 'Read more',
                    'href' => 'https://www.bitrix24.com',
                    'target' => '_blank',
                ],
                '.landing-block-node-icon' => ['fa', 'fa-telegram'],
                '.landing-block-node-embed' => [
                    'src' => '//www.youtube.com/embed/q4d8g9Dn3ww?autoplay=1&controls=0&loop=1&mute=1&rel=0',
                    'source' => 'https://www.youtube.com/watch?v=q4d8g9Dn3ww',
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

### Updating Component Parameters

Some blocks contain a component, such as a catalog. In this case, the method can only change those component parameters that are available for editing in the block manifest.

To understand which parameters can be changed, first obtain the block manifest using the method [landing.block.getmanifest](./landing-block-get-manifest.md) and check the `attrs` section for the desired component selector.

After that, pass the component selector and an object with the key `attrs` in `data`:

```js
BX24.callMethod(
    'landing.block.updatenodes',
    {
        lid: 5597,
        block: 44131,
        data: {
            'bitrix:catalog.section': {
                attrs: {
                    MESS_BTN_BUY: 'Buy'
                }
            }
        }
    }
);
```

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1774442460,
        "finish": 1774442460.28751,
        "duration": 0.2875099182128906,
        "processing": 0,
        "date_start": "2026-03-25T11:01:00+01:00",
        "date_finish": "2026-03-25T11:01:00+01:00",
        "operating_reset_at": 1774443060,
        "operating": 0.09410285949707031
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the block update. Upon successful execution, the method returns `true` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NODES_NOT_FOUND",
    "error_description": "No nodes found for modification"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid`, `block`, or `data` is missing ||
|| `TYPE_ERROR` | Incorrect type passed in parameter `lid`, `block`, `data`, `additional`, or `preventHistory` ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found or not accessible to the current user ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found on page `lid` or not accessible in the version of the page for editing ||
|| `ACCESS_DENIED` | Insufficient permissions to edit the site ||
|| `NODES_NOT_FOUND` | No changes passed for the block in the `data` parameter ||
|| `INCORRECT_AFFECTED` | The server could not confirm the saving of the modified HTML after updating the nodes with additional verification enabled ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-update-attrs.md)
- [{#T}](./landing-block-update-styles.md)
- [{#T}](./landing-block-get-manifest.md)
- [{#T}](./landing-block-get-list.md)
- [{#T}](./landing-block-update-content.md)