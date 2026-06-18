# Remove the deletion mark from the block landing.landing.markundeletedblock

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.landing.markundeletedblock` removes the deletion mark from the block.

If the page is already published, the changes will be visible to visitors after the "Publish changes" command in the interface or after calling the method [landing.landing.publication](../methods/landing-landing-publication.md).

The method only removes the deletion mark. If the block was hidden before deletion, it will remain hidden after restoration. To show such a block again, use [landing.landing.showblock](./landing-landing-show-block.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of landings. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **lid*** 
[`integer`](../../../data-types.md) | Identifier of the page.

The page identifier can be obtained using the method [landing.landing.getList](../methods/landing-landing-get-list.md), as well as from the results of the methods [landing.landing.add](../methods/landing-landing-add.md), [landing.landing.addByTemplate](../methods/landing-landing-add-by-template.md), and [landing.landing.copy](../methods/landing-landing-copy.md) ||
|| **block*** 
[`integer`](../../../data-types.md) | Identifier of the block.

To restore a deleted block, request it using the method [landing.block.getList](../../block/methods/landing-block-get-list.md) with the parameters `params.edit_mode = 1` and `params.deleted = 1`.

If you pass the identifier of a block from another page or a non-existent identifier, the method will return an error ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 627,
        "block": 11923
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.markundeletedblock.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 627,
        "block": 11923,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.markundeletedblock.json"
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
        method: 'landing.landing.markundeletedblock',
        params: {
          lid: 627,
          block: 11923,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Mark undeleted block result:', result)
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
      async function markUndeletedBlock() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.landing.markundeletedblock',
            params: {
              lid: 627,
              block: 11923,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Mark undeleted block result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', markUndeletedBlock)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.landing.markundeletedblock',
                [
                    'lid' => 627,
                    'block' => 11923,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error restoring block: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.markundeletedblock',
        {
            lid: 627,
            block: 11923
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
        'landing.landing.markundeletedblock',
        [
            'lid' => 627,
            'block' => 11923,
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
        "start": 1773972342,
        "finish": 1773972343.01137,
        "duration": 1.0113699436187744,
        "processing": 1,
        "date_start": "2026-03-20T05:05:42+01:00",
        "date_finish": "2026-03-20T05:05:43+01:00",
        "operating_reset_at": 1773972942,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | The result of removing the deletion mark. Returns `true` on successful execution ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "BLOCK_NOT_FOUND",
    "error_description": "Block not found in the landing"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid` or `block` is missing ||
|| `LANDING_NOT_EXIST` | The page with identifier `lid` was not found or is not accessible to the current user ||
|| `ACCESS_DENIED` | Insufficient permissions to call the method ||
|| `BLOCK_NOT_FOUND` | The block with identifier `block` does not exist or does not belong to the page `lid` ||
|| `TYPE_ERROR` | An incorrect type was passed in the parameter `lid`, `block`, or `preventHistory` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add-block.md)
- [{#T}](./landing-landing-copy-block.md)
- [{#T}](./landing-landing-delete-block.md)
- [{#T}](./landing-landing-down-block.md)
- [{#T}](./landing-landing-favorite-block.md)
- [{#T}](./landing-landing-hide-block.md)
- [{#T}](./landing-landing-mark-deleted-block.md)
- [{#T}](./landing-landing-move-block.md)
- [{#T}](./landing-landing-show-block.md)
- [{#T}](./landing-landing-unfavorite-block.md)
- [{#T}](./landing-landing-up-block.md)
- [{#T}](../methods/landing-landing-publication.md)