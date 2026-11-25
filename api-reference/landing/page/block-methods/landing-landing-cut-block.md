# Cut Block on Page landing.landing.cutblock

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site and full access to the block

The method `landing.landing.cutblock` records a "cut block" action in the page history. The block itself remains on the page: its place is cleared only when it is subsequently inserted using the [landing.landing.moveblock](./landing-landing-move-block.md) method. Before insertion, the action can be undone through the page change history.

Calling the method again for the same block does not create an additional history step and returns `true`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of the landing. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **lid***
[`integer`](../../../data-types.md) | Identifier of the page on which the block is located.

The page identifier can be obtained using the method [landing.landing.getList](../methods/landing-landing-get-list.md) ||
|| **block***
[`integer`](../../../data-types.md) | Identifier of the block from the page's edit version.

The identifier can be obtained using the method [landing.block.getList](../../block/methods/landing-block-get-list.md) with the parameter `params.edit_mode = 1` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{"lid":351,"block":6428}' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.cutblock.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{"lid":351,"block":6428,"auth":"**put_access_token_here**"}' \
      "https://**put.your-domain-here**/rest/landing.landing.cutblock.json"
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'landing.landing.cutblock',
        params: { lid: 351, block: 6428 },
        requestId: Text.getUuidRfc4122()
      })
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info(response.getData()!.result)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function cutBlock() {
        try {
          const $b24 = await B24Js.initializeB24Frame()
          const response = await $b24.actions.v2.call.make({
            method: 'landing.landing.cutblock',
            params: { lid: 351, block: 6428 },
            requestId: B24Js.Text.getUuidRfc4122()
          })
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }
          console.info(response.getData().result)
        } catch (error) {
          console.error(error)
        }
      }
      document.addEventListener('DOMContentLoaded', cutBlock)
    </script>
    ```

- Python

    The method does not have a ready-made wrapper in `b24pysdk` yet. Call it via `bitrix_token.call_method`.

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        response = bitrix_token.call_method(
            api_method="landing.landing.cutblock",
            params={
                "lid": 351,
                "block": 6428,
            },
        )
        print(response["result"])
    except BitrixAPIError as error:
        print(error.error, error.error_description)
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'landing.landing.cutblock',
            ['lid' => 351, 'block' => 6428]
        );
        print_r($response->getResponseData()->getResult());
    } catch (Throwable $e) {
        error_log($e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.cutblock',
        { lid: 351, block: 6428 },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.landing.cutblock',
        ['lid' => 351, 'block' => 6428]
    );
    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        var_dump($result['result']);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "landing.landing.cutblock", b24.Params{
        "lid": 351, "block": 6428,
    })
    if err != nil {
        return fmt.Errorf("landing.landing.cutblock: %w", err)
    }
    var value bool
    if err := json.Unmarshal(res.Result, &value); err != nil {
        return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("result:", value)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1795591363,
        "finish": 1795591363.176229,
        "duration": 0.17622900009155273,
        "processing": 0,
        "date_start": "2026-11-25T10:22:43+01:00",
        "date_finish": "2026-11-25T10:22:43+01:00",
        "operating_reset_at": 1795591915,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if the history step was recorded or such a step already exists ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400 Bad Request**

```json
{
    "error": "BLOCK_NOT_FOUND",
    "error_description": "Block was not found in the landing"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid` or `block` is not provided ||
|| `LANDING_NOT_EXIST` | The page with identifier `lid` is not found or is not accessible to the current user ||
|| `ACCESS_DENIED` | The user does not have permission to call landing methods or edit the page, or does not have full access to the block ||
|| `BLOCK_NOT_FOUND` | The block was not found in the edit version of the page `lid` ||
|| `TYPE_ERROR` | An invalid type was passed in the `lid` or `block` parameter ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-copy-block.md)
- [{#T}](./landing-landing-move-block.md)
- [{#T}](./landing-landing-mark-deleted-block.md)
- [{#T}](./landing-landing-mark-undeleted-block.md)
