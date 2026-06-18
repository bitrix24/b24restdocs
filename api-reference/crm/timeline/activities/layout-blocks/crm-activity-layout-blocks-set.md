# Set a set of additional content blocks in the activity crm.activity.layout.blocks.set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.activity.layout.blocks.set` sets a set of additional content blocks for the activity.

Setting a new set of additional content blocks in the activity will overwrite the previously added set within the same application.

Setting a set of additional content blocks cannot be applied to:
- [configurable activity](../configurable/index.md),
- a activity whose type is deprecated.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../../data-types.md) | Identifier of the CRM object type to which the activity is linked ||
|| **entityId***
[`integer`](../../../../data-types.md) | Identifier of the CRM object to which the activity is linked ||
|| **activityId***
[`integer`](../../../../data-types.md) | Identifier of the activity ||
|| **layout***
[`RestAppLayoutDto`](../configurable/structure/rest-app-layout-dto.md) | Object describing the set of additional content blocks ||
|#

## Code Examples

For the activity with `id = 8`, linked to the activity with `id = 4`, we will set the following set of additional content blocks:

1. Text
2. Long multiline text
3. Link
4. Block with a title

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"activityId":8,"layout":{"blocks":{"block_1":{"type":"text","properties":{"value":"Hello!\nWe are starting.","multiline":true,"bold":true,"color":"base_90"}},"block_2":{"type":"largeText","properties":{"value":"Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to the result.\nGoodbye."}},"block_3":{"type":"link","properties":{"text":"Open deal","bold":true,"action":{"type":"redirect","uri":"/crm/deal/details/123/"}}},"block_4":{"type":"withTitle","properties":{"title":"Title","block":{"type":"text","properties":{"value":"Some value"}}}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.layout.blocks.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"activityId":8,"layout":{"blocks":{"block_1":{"type":"text","properties":{"value":"Hello!\nWe are starting.","multiline":true,"bold":true,"color":"base_90"}},"block_2":{"type":"largeText","properties":{"value":"Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to the result.\nGoodbye."}},"block_3":{"type":"link","properties":{"text":"Open deal","bold":true,"action":{"type":"redirect","uri":"/crm/deal/details/123/"}}},"block_4":{"type":"withTitle","properties":{"title":"Title","block":{"type":"text","properties":{"value":"Some value"}}}}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.layout.blocks.set
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SetLayoutBlocksResult = {
      success: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<SetLayoutBlocksResult>({
        method: 'crm.activity.layout.blocks.set',
        params: {
          entityTypeId: 2, // Deal
          entityId: 4,     // Deal ID
          activityId: 8,   // Activity ID linked to the deal
          layout: {
            blocks: {
              block_1: {
                type: 'text',
                properties: {
                  value: 'Hello!\nWe are starting.',
                  multiline: true,
                  bold: true,
                  color: 'base_90',
                },
              },
              block_2: {
                type: 'largeText',
                properties: {
                  value: 'Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to the result.\nGoodbye.',
                },
              },
              block_3: {
                type: 'link',
                properties: {
                  text: 'Open deal',
                  bold: true,
                  action: {
                    type: 'redirect',
                    uri: '/crm/deal/details/123/',
                  },
                },
              },
              block_4: {
                type: 'withTitle',
                properties: {
                  title: 'Header',
                  block: {
                    type: 'text',
                    properties: {
                      value: 'Some value',
                    },
                  },
                },
              },
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Layout blocks set successfully:', result.success)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function setActivityLayoutBlocks() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.layout.blocks.set',
            params: {
              entityTypeId: 2, // Deal
              entityId: 4,     // Deal ID
              activityId: 8,   // Activity ID linked to the deal
              layout: {
                blocks: {
                  block_1: {
                    type: 'text',
                    properties: {
                      value: 'Hello!\nWe are starting.',
                      multiline: true,
                      bold: true,
                      color: 'base_90',
                    },
                  },
                  block_2: {
                    type: 'largeText',
                    properties: {
                      value: 'Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to the result.\nGoodbye.',
                    },
                  },
                  block_3: {
                    type: 'link',
                    properties: {
                      text: 'Open deal',
                      bold: true,
                      action: {
                        type: 'redirect',
                        uri: '/crm/deal/details/123/',
                      },
                    },
                  },
                  block_4: {
                    type: 'withTitle',
                    properties: {
                      title: 'Header',
                      block: {
                        type: 'text',
                        properties: {
                          value: 'Some value',
                        },
                      },
                    },
                  },
                },
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Layout blocks set successfully:', result.success)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setActivityLayoutBlocks)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.layout.blocks.set',
                [
                    'entityTypeId' => 2, // Deal
                    'entityId'     => 4, // Deal ID
                    'activityId'   => 8, // ID of the deal linked to this deal
                    'layout'       => [
                        'blocks' => [
                            'block_1' => [
                                'type'       => "text",
                                'properties' => [
                                    'value'     => "Hello!\nWe are starting.",
                                    'multiline' => true,
                                    'bold'      => true,
                                    'color'     => "base_90",
                                ],
                            ],
                            'block_2' => [
                                'type'       => "largeText",
                                'properties' => [
                                    'value' => "Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to the result.\nGoodbye.",
                                ],
                            ],
                            'block_3' => [
                                'type'       => "link",
                                'properties' => [
                                    'text'     => "Open deal",
                                    'bold'     => true,
                                    'action'   => [
                                        'type' => "redirect",
                                        'uri'  => "/crm/deal/details/123/",
                                    ],
                                ],
                            ],
                            'block_4' => [
                                'type'       => "withTitle",
                                'properties' => [
                                    'title'  => "Title",
                                    'block'  => [
                                        'type'       => "text",
                                        'properties' => [
                                            'value' => "Some value",
                                        ],
                                    ],
                                ],
                            ],
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting activity layout blocks: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const layout = {
        blocks: {
            'block_1': {
                type: "text",
                properties: {
                    value: "Hello!\nWe are starting.",
                    multiline: true,
                    bold: true,
                    color: "base_90"
                }
            },
            'block_2': {
                type: "largeText",
                properties: {
                    value: "Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to the result.\nGoodbye."
                }
            },
            'block_3': {
                type: "link",
                properties: {
                    text: "Open deal",
                    bold: true,
                    action: {
                        type: "redirect",
                        uri: "/crm/deal/details/123/"
                    }
                }
            },
            'block_4': {
                type: "withTitle",
                properties: {
                    title: "Title",
                    block: {
                        type: "text",
                        properties: {
                            value: "Some value"
                        }
                    }
                }
            }
        }
    };
    BX24.callMethod(
        'crm.activity.layout.blocks.set',
        {
            entityTypeId: 2, // Deal
            entityId: 4,     // Deal ID
            activityId: 8,   // ID of the activity linked to this deal
            layout: layout,  // Object describing the set of additional content blocks
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');
    $result = CRest::call(
        'crm.activity.layout.blocks.set',
        [
            'entityTypeId' => 2,
            'entityId' => 4,
            'activityId' => 8,
            'layout' => [
                'blocks' => [
                    'block_1' => [
                        'type' => "text",
                        'properties' => [
                            'value' => "Hello!\nWe are starting.",
                            'multiline' => true,
                            'bold' => true,
                            'color' => "base_90"
                        ]
                    ],
                    'block_2' => [
                        'type' => "largeText",
                        'properties' => [
                            'value' => "Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to the result.\nGoodbye."
                        ]
                    ],
                    'block_3' => [
                        'type' => "link",
                        'properties' => [
                            'text' => "Open deal",
                            'bold' => true,
                            'action' => [
                                'type' => "redirect",
                                'uri' => "/crm/deal/details/123/"
                            ]
                        ]
                    ],
                    'block_4' => [
                        'type' => "withTitle",
                        'properties' => [
                            'title' => "Title",
                            'block' => [
                                'type' => "text",
                                'properties' => [
                                    'value' => "Some value"
                                ]
                            ]
                        ]
                    ]
                ]
            ]
        ]
    );
    echo '';
    print_r($result);
    echo '';
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.activity.layout.blocks.set(
            entity_type_id=2,
            entity_id=101,
            activity_id=999,
            layout={
                "blocks": {
                    "summary": {
                        "type": "text",
                        "properties": {
                            "value": "Proposal is ready",
                            "multiline": True,
                            "bold": True,
                            "color": "base_90",
                        },
                    },
                    "dealLink": {
                        "type": "link",
                        "properties": {
                            "text": "Open deal",
                            "bold": True,
                            "action": {"type": "redirect", "uri": "/crm/deal/details/101/"},
                        },
                    },
                },
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```
{% endlist %}

## Appearance

If the activity contains more than one set of additional content blocks, they will be displayed in the order they were added.

In the HTML layout, it is explicitly highlighted with data attributes which application added the set of additional content blocks:
- `data-app-name`: name of the application,
- `data-rest-client-id`: identifier of the application.

## Response Handling

HTTP status: **200**

Returns `{ success: true }` in case of successful writing of the set of additional content blocks, otherwise `null`.

```json
{
    "success": true
}
```

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_WRONG_CONTEXT",
    "error_description": "The method call is only possible in the context of a REST application"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ERROR_WRONG_CONTEXT` | The method can only be called in the context of a REST application ||
|| `OWNER_NOT_FOUND` | The element to which the activity is linked was not found ||
|| `NOT_FOUND` | The activity was not found ||
|| `ACCESS_DENIED` | Access denied ||
|| `UNSUITABLE_ACTIVITY_TYPE_ERROR` | The type of this activity is not suitable for adding a set of additional content blocks ||
|| `FIELD_IS_REQUIRED` | The `blocks` field in `RestAppLayoutDto` must be filled. ||
|#

The method also returns errors related to the incorrect structure of the set of content blocks. Details can be found in the error message.

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-activity-layout-blocks-get.md)
- [{#T}](./crm-activity-layout-blocks-delete.md)