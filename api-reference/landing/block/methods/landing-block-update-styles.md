# Change Styles of Block landing.block.updateStyles

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.block.updateStyles` updates the CSS classes and inline styles of block elements in the draft version of the page.

If the page is already published, changes will be visible to visitors after re-publishing through the interface or using the method [landing.landing.publication](../../page/methods/landing-landing-publication.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the method [landing.landing.getList](../../page/methods/landing-landing-get-list.md) ||
|| **block***
[`integer`](../../../data-types.md) | Block identifier in the editable version of the page.

The block identifier can be obtained using the method [landing.block.getlist](./landing-block-get-list.md) with the parameter `params.edit_mode = 1`. If the block identifier from the published version of the page is passed, the method may return an error ||
|| **data***
[`object`](../../../data-types.md) | Object format:

```json
{
    "<selector>": {
        "classList": [],
        "affect": [],
        "style": {}
    }
}
```

where:
- `<selector>` — CSS selector from the block manifest or `#wrapper`,
- `classList` — list of CSS classes for the element after the update,
- `affect` — list of CSS properties to be removed from nested elements,
- `style` — set of inline styles for the element itself.

A detailed description of the selector value is provided [below](#selector-value) ||
|| **preventHistory**
[`boolean`](../../../data-types.md) | If `true` is passed, the method will not add the change to the page history.

By default, it is `false` ||
|#

### Parameter data {#data}

#|
|| **Key**
`type` | **Description** ||
|| **<selector>**
[`string`](../../../data-types.md) \| [`array`](../../../data-types.md) \| [`object`](../../../data-types.md) | CSS selector from the `style` section of the block manifest or the special selector `#wrapper` for the block wrapper.

If the same selector appears multiple times, you can specify the position using `@`, for example, `.landing-block-node-text@1`. Positions are numbered starting from `0`. If the position is not specified, the method applies changes to all found elements with that selector.

The format of the value depends on the data being passed [(detailed description)](#selector-value) ||
|#

The method processes only those selectors that are described in the block manifest. The special selector `#wrapper` allows you to change the block wrapper. A list of available selectors can be obtained using the method [landing.block.getmanifest](./landing-block-get-manifest.md).

### Selector Value {#selector-value}

#|
|| **Name**
`type` | **Description** ||
|| **classList**
[`array`](../../../data-types.md) | List of CSS classes for the element after the update.

The method completely replaces the `class` attribute rather than adding classes to the current list ||
|| **affect**
[`array`](../../../data-types.md) | List of CSS properties to be removed from the inline styles of all nested elements of the found node. For example, `["text-align", "color"]` ||
|| **style**
[`object`](../../../data-types.md) | Set of inline styles for the found element in the format `{ "css-property": "value" }`. The method merges the passed values with the current `style` attribute.

If the field is not passed and the element does not have a `background-image`, the current inline style remains unchanged. If `background-image` is already set, the method retains only this property and removes other inline styles ||
|#

Instead of an object, you can pass a string with a single class or an array of classes directly. This format is equivalent to the `classList` field.

{% note warning %}

If system classes of the block, such as `landing-block-node-text`, are not included in `classList`, the element may stop being editable through the interface. The method completely replaces the list of classes.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 313,
        "block": 6134,
        "data": {
          ".landing-block-node-text": {
            "classList": [
              "landing-block-node-text",
              "g-color-white",
              "text-right"
            ],
            "affect": [
              "text-align",
              "color"
            ],
            "style": {
              "font-weight": "700"
            }
          }
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.updateStyles.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 313,
        "block": 6134,
        "data": {
          ".landing-block-node-text": {
            "classList": [
              "landing-block-node-text",
              "g-color-white",
              "text-right"
            ],
            "affect": [
              "text-align",
              "color"
            ],
            "style": {
              "font-weight": "700"
            }
          }
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.updateStyles.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.block.updateStyles',
            {
                lid: 313,
                block: 6134,
                data: {
                    '.landing-block-node-text': {
                        classList: [
                            'landing-block-node-text',
                            'g-color-white',
                            'text-right'
                        ],
                        affect: [
                            'text-align',
                            'color'
                        ],
                        style: {
                            'font-weight': '700'
                        }
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
                'landing.block.updateStyles',
                [
                    'lid' => 313,
                    'block' => 6134,
                    'data' => [
                        '.landing-block-node-text' => [
                            'classList' => [
                                'landing-block-node-text',
                                'g-color-white',
                                'text-right',
                            ],
                            'affect' => [
                                'text-align',
                                'color',
                            ],
                            'style' => [
                                'font-weight' => '700',
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
        echo 'Error updating block styles: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.updateStyles',
        {
            lid: 313,
            block: 6134,
            data: {
                '.landing-block-node-text': {
                    classList: [
                        'landing-block-node-text',
                        'g-color-white',
                        'text-right'
                    ],
                    affect: [
                        'text-align',
                        'color'
                    ],
                    style: {
                        'font-weight': '700'
                    }
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
        'landing.block.updateStyles',
        [
            'lid' => 313,
            'block' => 6134,
            'data' => [
                '.landing-block-node-text' => [
                    'classList' => [
                        'landing-block-node-text',
                        'g-color-white',
                        'text-right',
                    ],
                    'affect' => [
                        'text-align',
                        'color',
                    ],
                    'style' => [
                        'font-weight' => '700',
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
        "start": 1774520356,
        "finish": 1774520356.142134,
        "duration": 0.1421339511871338,
        "processing": 0,
        "date_start": "2026-03-26T13:19:16+01:00",
        "date_finish": "2026-03-26T13:19:16+01:00",
        "operating_reset_at": 1774520956,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the style update. If successful, the method returns `true` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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
|| `MISSING_PARAMS` | Required parameter `lid`, `block`, or `data` is missing ||
|| `ACCESS_DENIED` | Insufficient permissions to edit the site ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found or unavailable to the current user ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found on page `lid` or unavailable in the editable version of the page ||
|| `TYPE_ERROR` | Incorrect type of one of the method parameters, for example, `data` in an unsuitable format ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-update-nodes.md)
- [{#T}](./landing-block-update-attrs.md)
- [{#T}](./landing-block-get-list.md)
- [{#T}](./landing-block-get-manifest.md)
- [{#T}](../../page/methods/landing-landing-publication.md)