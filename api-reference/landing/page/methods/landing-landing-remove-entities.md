# Remove Blocks and Clear Image File Bindings on Page landing.landing.removeEntities

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.landing.removeEntities` removes specified blocks from the page together with their images. The method also clears file bindings: internal records that remain after an image has been removed from a block. The method does not change the content of such a block.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal landing parameter: the type of sites the method works with. It is not related to the REST scope `landing`.

For Knowledge Base pages, pass `KNOWLEDGE`; for group knowledge base pages, pass `GROUP`. The value is case-insensitive. Without `scope`, the method cannot find a Knowledge Base page and returns the `LANDING_NOT_EXIST` error. For sites of the `PAGE`, `STORE`, `SMN`, and `VIBE` types, the parameter is not needed [(detailed description)](../../types.md) ||
|| **lid*** 
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the method [landing.landing.getList](./landing-landing-get-list.md), as well as from the results of the methods [landing.landing.add](./landing-landing-add.md), [landing.landing.addByTemplate](./landing-landing-add-by-template.md), and [landing.landing.copy](./landing-landing-copy.md) ||
|| **data*** 
[`object`](../../../data-types.md) | Blocks and images to delete [(detailed description)](#data).

If `data` contains neither blocks nor images, the method changes nothing. For an existing page, it still returns `true` ||
|#

### Parameter data {#data}

#| 
|| **Name**
`type` | **Description** ||
|| **blocks**
[`integer[]`](../../../data-types.md) | Identifiers of the blocks to delete. Together with a block, the method deletes all images associated with it.

The method skips blocks that are not on the page or that the user cannot delete. If the page exists and is accessible, the method returns `true` even if some or all blocks from the list remain on the page ||
|| **images**
[`object[]`](../../../data-types.md) | "Block — image" pairs whose file binding needs to be cleared [(detailed description)](#images). The method does not change the content of the block.

The method does not return an error if the block is not found, is already specified in `blocks`, or the image is not associated with it ||
|#

### Parameter images {#images}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **block*** 
[`integer`](../../../data-types.md) | Identifier of the block associated with the image file binding ||
|| **image*** 
[`integer`](../../../data-types.md) | Identifier of the image file `FILE_ID` associated with the block `block`. This is the same `id` that the [landing.block.uploadfile](../../block/methods/landing-block-upload-file.md) method returns.

For existing images, `FILE_ID` can be obtained using the method [landing.block.getcontent](../../block/methods/landing-block-get-content.md). In the response, you need to find the HTML of the block in the `content` field and check the value of the `data-fileid` attribute for the required image
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

The example removes the blocks from `blocks` entirely and clears the image bindings in other blocks from `images`.

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
      "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/landing.landing.removeEntities.json"
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
      "https://**put_your_bitrix24_address**/rest/landing.landing.removeEntities.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'landing.landing.removeEntities',
        params: {
          lid: 648,
          data: {
            blocks: [12167, 123],
            images: [
              {
                block: 12269,
                image: 6866,
              },
              {
                block: 12268,
                image: 6861,
              },
            ],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('removeEntities result:', result)
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
      async function removeEntities() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.landing.removeEntities',
            params: {
              lid: 648,
              data: {
                blocks: [12167, 123],
                images: [
                  {
                    block: 12269,
                    image: 6866,
                  },
                  {
                    block: 12268,
                    image: 6861,
                  },
                ],
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
          console.info('removeEntities result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', removeEntities)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.landing.landing.remove_entities(
            lid=648,
            data={
                "blocks": [
                    12167,
                    123,
                ],
                "images": [
                    {
                        "block": 12269,
                        "image": 6866,
                    },
                    {
                        "block": 12268,
                        "image": 6861,
                    },
                ],
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "landing.landing.removeEntities", b24.Params{
    	"lid": 648,
    	"data": b24.Params{
    		"blocks": []int{12167, 123},
    		"images": []b24.Params{
    			{
    				"block": 12269,
    				"image": 6866,
    			},
    			{
    				"block": 12268,
    				"image": 6861,
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("landing.landing.removeEntities: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
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
[`boolean`](../../../data-types.md) | `true` if the page is found and accessible. The response does not show which blocks the method skipped. Bitrix24 deletes files later, and only the files that are not used anywhere else ||
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
|| `MISSING_PARAMS` | The required parameter `lid` or `data` is not provided ||
|| `LANDING_NOT_EXIST` | Page not found: there is no page with this `lid`, the page is in the trash, the current user cannot access it, or `scope` is not passed for a Knowledge Base page ||
|| `ACCESS_DENIED` | Access denied: `data.images` contains images, and the user does not have the "edit" access permission for the site. Without this permission, the method does not delete the blocks from `data.blocks`, but does not return an error ||
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