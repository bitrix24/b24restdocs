# Move Block Down landing.landing.downblock

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.landing.downblock` moves a block one position down in the page draft.

If the page is already published, visitors will see the change after the "Publish Changes" command in the interface or after calling the method [landing.landing.publication](../methods/landing-landing-publication.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of the landing. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **lid*** 
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the method [landing.landing.getList](../methods/landing-landing-get-list.md), as well as from the results of the methods [landing.landing.add](../methods/landing-landing-add.md), [landing.landing.addByTemplate](../methods/landing-landing-add-by-template.md), and [landing.landing.copy](../methods/landing-landing-copy.md) ||
|| **block*** 
[`integer`](../../../data-types.md) | Block identifier in the editable version of the page.

The block identifier can be obtained using the method [landing.block.getList](../../block/methods/landing-block-get-list.md) with the parameter `params.edit_mode = 1` ||
|| **preventHistory**
[`boolean`](../../../data-types.md) | If `true`, the method does not add the action to the page change history. Default is `false` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "block": 6428
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.downblock.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "block": 6428,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.downblock.json"
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
        method: 'landing.landing.downblock',
        params: {
          lid: 351,
          block: 6428,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Block moved down:', result)
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
      async function moveBlockDown() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.landing.downblock',
            params: {
              lid: 351,
              block: 6428,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Block moved down:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', moveBlockDown)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.landing.downblock',
                [
                    'lid' => 351,
                    'block' => 6428,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error moving block down: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.downblock',
        {
            lid: 351,
            block: 6428
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
        'landing.landing.downblock',
        [
            'lid' => 351,
            'block' => 6428,
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

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773940778,
        "finish": 1773940779.073966,
        "duration": 1.0739660263061523,
        "processing": 0,
        "date_start": "2026-03-19T20:19:38+01:00",
        "date_finish": "2026-03-19T20:19:39+01:00",
        "operating_reset_at": 1773941379,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the move. Returns `true` on successful execution ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "BLOCK_WRONG_SORT",
    "error_description": "Out of sort range or block not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid` or `block` is missing ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found or not accessible to the current user ||
|| `ACCESS_DENIED` | User does not have permission to call landing methods or does not have permission to edit page `lid` ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found on page `lid`, already marked as deleted, or not accessible in the editable version of the page ||
|| `BLOCK_WRONG_SORT` | Block is already at the bottom of the page, so it cannot be moved down further ||
|| `TYPE_ERROR` | Incorrect type passed in parameter `lid`, `block`, or `preventHistory` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add-block.md)
- [{#T}](./landing-landing-copy-block.md)
- [{#T}](./landing-landing-delete-block.md)
- [{#T}](./landing-landing-hide-block.md)
- [{#T}](./landing-landing-mark-deleted-block.md)
- [{#T}](./landing-landing-mark-undeleted-block.md)
- [{#T}](./landing-landing-move-block.md)
- [{#T}](./landing-landing-show-block.md)
- [{#T}](./landing-landing-up-block.md)
- [{#T}](../methods/landing-landing-publication.md)