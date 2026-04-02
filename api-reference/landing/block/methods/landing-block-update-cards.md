# Update Cards of Block landing.block.updateCards

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for the site

The method `landing.block.updateCards` updates the set of cards in the block and the nodes within those cards in the draft of the page.

This method works with cards described in the `cards` key of the block manifest. If the page is already published, changes will be visible to visitors after publication through the interface or via the method [landing.landing.publication](../../page/methods/landing-landing-publication.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) | Identifier of the page.

The page identifier can be obtained using the method [landing.landing.getlist](../../page/methods/landing-landing-get-list.md) ||
|| **block***
[`integer`](../../../data-types.md) | Identifier of the block in the draft of the page.

The block identifier can be obtained using the method [landing.block.getlist](./landing-block-get-list.md) with the parameter `params.edit_mode = 1` ||
|| **data***
[`object`](../../../data-types.md) | Set of changes for the block cards [(detailed description)](#data) ||
|#

### Parameter data {#data}

#|
|| **Key**
`type` | **Description** ||
|| **<card selector from manifest.cards>**
[`object`](../../../data-types.md) | Card selector from the [cards section of the block manifest](../manifest.md#key-cards).

The value describes the final set of cards for this selector and changes to their nodes [(detailed description)](#card-data).

To find available card selectors and presets for a specific block, retrieve the manifest using the method [landing.block.getmanifest](./landing-block-get-manifest.md) ||
|#

### Object <card selector from manifest.cards> {#card-data}

#|
|| **Name**
`type` | **Description** ||
|| **source**
[`array`](../../../data-types.md) | Defines the final order and number of cards [(detailed description)](#source) ||
|| **values**
[`array`](../../../data-types.md) | Updates nodes within cards after applying `source` [(detailed description)](#values) ||
|#

### Element of the source array {#source}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Type of the card source.

Possible values:
`card` - take the current card by index,
`preset` - create a card from a preset in `manifest.cards.<selector>.presets`.

Default - `card` ||
|| **value**
[`integer`](../../../data-types.md) \| [`string`](../../../data-types.md) | Value of the card source.

For `card`, this is the index of the current card by selector, starting from `0`.

For `preset`, this is the code of the preset from the block manifest ||
|#

If the `source` element does not contain `type`, the method uses the value `card`. If `value` is not provided, the method uses index `0`.

To create a new card from a preset, specify `type` with the value `preset` and pass the preset code from the block manifest in `value`. For example:

```json
"source": [
  {
    "type": "card",
    "value": 0
  },
  {
    "type": "preset",
    "value": "preset_code"
  }
]
```

### Element of the values array {#values}

Each element of the `values` array must be an object containing one or more node changes.

#|
|| **Key**
`type` | **Description** ||
|| **<node selector>@<position>**
[`string`](../../../data-types.md) \| [`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | Node selector within the card with position indicated by `@`.

The position is numbered from `0` and indicates which card to change the node in after applying `source`.

The format of the value is the same as the `data` parameter in the method [landing.block.updatenodes](./landing-block-update-nodes.md).

One object in the `values` array can contain multiple keys for different nodes and positions ||
|#

If an element of the `values` array is not an object, the method will skip it without error. For example, in one element of the array, you can change multiple cards at once:

```json
"values": [
  {
    ".landing-block-node-title@0": "First Card",
    ".landing-block-node-title@1": "Second Card"
  }
]
```
For detailed formats of values for different types of nodes, refer to the article [landing.block.updatenodes](./landing-block-update-nodes.md) and the overview [Node Types](../node-types.md).

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
          ".landing-block-card": {
            "source": [
              {
                "type": "card",
                "value": 0
              },
              {
                "type": "preset",
                "value": "preset_code"
              }
            ],
            "values": [
              {
                ".landing-block-node-title@0": "First Card",
                ".landing-block-node-title@1": "Card from Preset"
              }
            ]
          }
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.updateCards.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 311,
        "block": 6058,
        "data": {
          ".landing-block-card": {
            "source": [
              {
                "type": "card",
                "value": 0
              },
              {
                "type": "preset",
                "value": "preset_code"
              }
            ],
            "values": [
              {
                ".landing-block-node-title@0": "First Card",
                ".landing-block-node-title@1": "Card from Preset"
              }
            ]
          }
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.updateCards.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.updateCards',
    		{
    			lid: 311,
    			block: 6058,
    			data: {
    				'.landing-block-card': {
    					source: [
    						{
    							type: 'card',
    							value: 0
    						},
    						{
    							type: 'preset',
    							value: 'preset_code'
    						}
    					],
    					values: [
    						{
    							'.landing-block-node-title@0': 'First Card',
    							'.landing-block-node-title@1': 'Card from Preset'
    						}
    					]
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
                'landing.block.updateCards',
                [
                    'lid' => 311,
                    'block' => 6058,
                    'data' => [
                        '.landing-block-card' => [
                            'source' => [
                                [
                                    'type' => 'card',
                                    'value' => 0,
                                ],
                                [
                                    'type' => 'preset',
                                    'value' => 'preset_code',
                                ],
                            ],
                            'values' => [
                                [
                                    '.landing-block-node-title@0' => 'First Card',
                                    '.landing-block-node-title@1' => 'Card from Preset',
                                ],
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
        echo 'Error updating block cards: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.updateCards',
        {
            lid: 311,
            block: 6058,
            data: {
                '.landing-block-card': {
                    source: [
                        {
                            type: 'card',
                            value: 0
                        },
                        {
                            type: 'preset',
                            value: 'preset_code'
                        }
                    ],
                    values: [
                        {
                            '.landing-block-node-title@0': 'First Card',
                            '.landing-block-node-title@1': 'Card from Preset'
                        }
                    ]
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
        'landing.block.updateCards',
        [
            'lid' => 311,
            'block' => 6058,
            'data' => [
                '.landing-block-card' => [
                    'source' => [
                        [
                            'type' => 'card',
                            'value' => 0,
                        ],
                        [
                            'type' => 'preset',
                            'value' => 'preset_code',
                        ],
                    ],
                    'values' => [
                        [
                            '.landing-block-node-title@0' => 'First Card',
                            '.landing-block-node-title@1' => 'Card from Preset',
                        ],
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
        "start": 1774525085,
        "finish": 1774525085.858169,
        "duration": 0.8581690788269043,
        "processing": 0,
        "date_start": "2026-03-26T14:38:05+01:00",
        "date_finish": "2026-03-26T14:38:05+01:00",
        "operating_reset_at": 1774525685,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the card update. Returns `true` on successful execution ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Insufficient call parameters, missing: data"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid`, `block`, or `data` is missing ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found or not accessible to the current user ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found on page `lid` or not accessible in the draft of the page ||
|| `ACCESS_DENIED` | User does not have permission to edit the site ||
|| `INCORRECT_AFFECTED` | The server could not confirm that the changes were applied correctly. The error depends on server settings and is not related to the request parameters ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-clone-card.md)
- [{#T}](./landing-block-remove-card.md)
- [{#T}](./landing-block-update-nodes.md)
- [{#T}](./landing-block-add-card.md)
- [{#T}](./landing-block-get-manifest.md)