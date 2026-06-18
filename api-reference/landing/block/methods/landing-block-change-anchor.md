# Change the Anchor of the Block landing.block.changeAnchor

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.block.changeAnchor` modifies or removes a custom anchor of a block in the draft of a page.

If the page is already published, changes will be visible to visitors after publishing the updates through the interface or using the method [landing.landing.publication](../../page/methods/landing-landing-publication.md).

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of landings. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **lid***
[`integer`](../../../data-types.md) | Identifier of the page.

The page identifier can be obtained using the method [landing.landing.getList](../../page/methods/landing-landing-get-list.md) ||
|| **block***
[`integer`](../../../data-types.md) | Identifier of the block in the draft of the page.

The block identifier can be obtained using the method [landing.block.getlist](./landing-block-get-list.md) with the parameter `params.edit_mode = true` ||
|| **data***
[`string`](../../../data-types.md) | New anchor of the block without the `#` symbol.

Pass the value as a string. The anchor must start with a letter `A-Z` or `a-z`, contain at least two characters, and may include only letters `A-Z` and `a-z`, digits `0-9`, and symbols `-`, `_`, `.`, `:`.

A single-character anchor is not allowed. For example, `a` will trigger an error. Examples of valid values: `about-us`, `Section_A`, `faq.v2`, `Tab:1` ||
|| **preventHistory**
[`boolean`](../../../data-types.md) | Do not add the change to the page history.

Possible values:
`true` — do not save the change in history,
`false` — save the change in history.

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
        "data": "about-us",
        "preventHistory": true
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.changeAnchor.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 311,
        "block": 6058,
        "data": "about-us",
        "preventHistory": true,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.changeAnchor.json"
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
        method: 'landing.block.changeAnchor',
        params: {
          lid: 311,
          block: 6058,
          data: 'about-us',
          preventHistory: true,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Anchor changed:', result)
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
      async function changeBlockAnchor() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.block.changeAnchor',
            params: {
              lid: 311,
              block: 6058,
              data: 'about-us',
              preventHistory: true,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Anchor changed:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', changeBlockAnchor)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.block.changeAnchor',
                [
                    'lid' => 311,
                    'block' => 6058,
                    'data' => 'about-us',
                    'preventHistory' => true,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error changing block anchor: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.changeAnchor',
        {
            lid: 311,
            block: 6058,
            data: 'about-us',
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
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.block.changeAnchor',
        [
            'lid' => 311,
            'block' => 6058,
            'data' => 'about-us',
            'preventHistory' => true,
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
        "start": 1774525298,
        "finish": 1774525298.491831,
        "duration": 0.49183106422424316,
        "processing": 0,
        "date_start": "2026-03-26T14:41:38+01:00",
        "date_finish": "2026-03-26T14:41:38+01:00",
        "operating_reset_at": 1774525898,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | The result of changing the anchor. Upon successful execution, the method returns `true` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "BAD_ANCHOR",
    "error_description": "The anchor must start with a character from a-z and may only contain characters from \"a-z\", \"0-9\", \"-\", \"_\", \".\", \":\""
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid`, `block`, or `data` is missing ||
|| `LANDING_NOT_EXIST` | The page with identifier `lid` was not found or is not accessible to the current user ||
|| `BLOCK_NOT_FOUND` | The block with identifier `block` was not found in the draft of the page ||
|| `ACCESS_DENIED` | Insufficient rights to edit the site ||
|| `BAD_ANCHOR` | An invalid anchor was provided: it does not start with a letter, contains forbidden characters, or consists of a single character ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-update-nodes.md)
- [{#T}](./landing-block-change-node-name.md)
- [{#T}](./landing-block-get-list.md)