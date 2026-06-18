# Get Content of Block landing.block.getcontent

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "view" access permission for the site

The method `landing.block.getcontent` returns the ready HTML of the block, its resources, manifest, and service properties of the block.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of landings. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **lid*** 
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the method [landing.landing.getlist](../../page/methods/landing-landing-get-list.md) ||
|| **block*** 
[`integer`](../../../data-types.md) | Block identifier.

The block identifier can be obtained using the method [landing.block.getlist](./landing-block-get-list.md) ||
|| **editMode**
[`boolean`](../../../data-types.md) | Mode for obtaining the version of the block.

Possible values:
`true` — return the draft of the block from the page,
`false` — return the published version of the block.

Default is `false`. When `editMode=true`, the method automatically includes the return of HTML for inactive blocks.

If the page has not yet been published, a call without `editMode` may not find the block ||
|| **params**
[`object`](../../../data-types.md) | Additional parameters [(detailed description)](#params) ||
|#

### Parameter params {#params}

#| 
|| **Name**
`type` | **Description** ||
|| **wrapper_show**
[`boolean`](../../../data-types.md) | Whether to return the external container of the block `<div class="block-wrapper">` in `result.content`.

Possible values:
`true` — return HTML along with the external container of the block used by the Bitrix24 page editor,
`false` — return the HTML of the block without the container. Default is `true` ||
|| **force_unactive**
[`boolean`](../../../data-types.md) | Generate HTML even for inactive blocks.

Possible values:
`true` — return HTML of the inactive block,
`false` — if the block is inactive, the `result.content` field will return an empty string.

Default is `false`. When `editMode=true`, this mode is automatically enabled ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 4858,
        "block": 39556,
        "editMode": true,
        "params": {
          "wrapper_show": false
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.getcontent.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 4858,
        "block": 39556,
        "editMode": true,
        "params": {
          "wrapper_show": false
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.getcontent.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BlockContentResult = {
      id: number
      sections: string
      active: boolean
      access: string
      anchor: string
      php: boolean
      designed: boolean
      repoId: number | null
      content: string
      content_ext: string
      css: string[]
      js: string[]
      assetStrings: string[]
      lang: string[] | Record<string, string>
      manifest: Record<string, unknown>
      dynamicParams: unknown[]
    }

    try {
      const response = await $b24.actions.v2.call.make<BlockContentResult>({
        method: 'landing.block.getcontent',
        params: {
          lid: 4858,
          block: 39556,
          editMode: true,
          params: {
            wrapper_show: false,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.id, result.content, result.active)
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
      async function getBlockContent() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.block.getcontent',
            params: {
              lid: 4858,
              block: 39556,
              editMode: true,
              params: {
                wrapper_show: false,
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
          console.info(result.id, result.content, result.active)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getBlockContent)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.block.getcontent',
                [
                    'lid' => 4858,
                    'block' => 39556,
                    'editMode' => true,
                    'params' => [
                        'wrapper_show' => false,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting block content: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.getcontent',
        {
            lid: 4858,
            block: 39556,
            editMode: true,
            params: {
                wrapper_show: false
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
        'landing.block.getcontent',
        [
            'lid' => 4858,
            'block' => 39556,
            'editMode' => true,
            'params' => [
                'wrapper_show' => false,
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
    "result": {
        "id": 28853,
        "sections": "tiles,news",
        "active": true,
        "access": "X",
        "anchor": "b28853",
        "php": false,
        "designed": false,
        "repoId": null,
        "content": "<div id=\"block28853\" data-id=\"28853\" class=\"block-wrapper block-18-2-two-cols-fix-img-text-button-with-cards\"><section class=\"landing-block g-pt-30 g-pb-30 g-bg-transparent\">...</section></div>",
        "content_ext": "",
        "css": [],
        "js": [
            "/bitrix/js/pull/protobuf/protobuf.js?1592315491274055",
            "/bitrix/js/pull/protobuf/model.min.js?159231549114190",
            "/bitrix/js/main/core/core_promise.min.js?17647596972494",
            "/bitrix/js/rest/client/rest.client.min.js?16015491189240",
            "/bitrix/js/pull/client/pull.client.min.js?174471771449849"
        ],
        "assetStrings": [],
        "lang": [],
        "manifest": {
            "block": {
                "name": "List of Pages with Small Image on the Left",
                "section": [
                    "tiles",
                    "news"
                ]
            },
            "cards": {
                ".landing-block-card": {
                    "name": "Card",
                    "label": [
                        ".landing-block-node-img",
                        ".landing-block-node-title"
                    ]
                }
            }
        },
        "dynamicParams": []
    },
    "time": {
        "start": 1774520845,
        "finish": 1774520845.380018,
        "duration": 0.3800179958343506,
        "processing": 0,
        "date_start": "2026-03-26T13:27:25+01:00",
        "date_finish": "2026-03-26T13:27:25+01:00",
        "operating_reset_at": 1774521445,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Block data [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

### Object result {#result}

#| 
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Block identifier ||
|| **sections**
[`string`](../../../data-types.md) | Codes of the block sections from the manifest, concatenated into a string ||
|| **active**
[`boolean`](../../../data-types.md) | Indicator of the block's activity ||
|| **access**
[`string`](../../../data-types.md) | Access level to the block. Possible values:

`A` — access is closed to all blocks,
`D` — access is denied,
`V` — can edit only the design,
`W` — can edit content and design without deletion,
`X` — full access. ||
|| **anchor**
[`string`](../../../data-types.md) | Local anchor of the block.

The value is used as the HTML attribute `id` for the external wrapper of the block and in links to the block within the page ||
|| **php**
[`boolean`](../../../data-types.md) | Indicator that there is PHP code in the original content of the block ||
|| **designed**
[`boolean`](../../../data-types.md) | Indicator that the block has been modified in the designer ||
|| **repoId**
[`integer`](../../../data-types.md) | Identifier of the REST block from the repository or `null` if the block is not linked to a REST repository ||
|| **content**
[`string`](../../../data-types.md) | Ready HTML of the block for output on the page.

Returns the final HTML after rendering, not the block template.

If `params.wrapper_show=true`, the field returns HTML along with the external container of the block. If `params.wrapper_show=false`, it returns only the HTML of the block.

If the block is inactive and `params.force_unactive=false`, the field returns an empty string ||
|| **content_ext**
[`string`](../../../data-types.md) | Additional HTML code of connected extensions ||
|| **css**
[`array`](../../../data-types.md) | List of CSS resources of the block and dependencies connected during rendering.

If there are no separate CSS connections, an empty array is returned ||
|| **js**
[`array`](../../../data-types.md) | List of JS resources of the block and dependencies connected during rendering.

If `editMode=true` is requested for a REST block with `repoId` not equal to `null`, the block's own JS resources are not returned ||
|| **assetStrings**
[`array`](../../../data-types.md) | Initialization strings of resources to be added to the page ||
|| **lang**
[`array`](../../../data-types.md) \| [`object`](../../../data-types.md) | Language messages of connected extensions in the format of `key: value` object.

If connected extensions have no language messages, an empty array is returned ||
|| **manifest**
[`object`](../../../data-types.md) | The complete manifest of the block. The general format is described in the article [Block Manifest](../manifest.md) ||
|| **dynamicParams**
[`array`](../../../data-types.md) | Data source parameters for the dynamic block.

For a static block, the field returns an empty array ||
|| **requiredUserAction**
[`array`](../../../data-types.md) | Additional action that needs to be performed after loading the block.

The field is returned only in `editMode`, if an additional action is required from the user after loading the block ||
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
|| `MISSING_PARAMS` | Required top-level parameter `lid` or `block` is missing ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found or not accessible to the current user ||
|| `ACCESS_DENIED` | No access to the "Sites and Stores" section ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found in the selected version of the page ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-get-content-from-repository.md)
- [{#T}](./landing-block-update-content.md)
- [{#T}](./landing-block-get-list.md)
- [{#T}](./landing-block-get-by-id.md)
- [{#T}](./landing-block-get-manifest.md)