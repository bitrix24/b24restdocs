# Delete User Block landing.repo.unregister

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: user with View access permission in the Sites section

The method `landing.repo.unregister` deletes a user block by its code.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code**^*^
[`string`](../../data-types.md) | External code of the block (`XML_ID`).

The code can be obtained, for example, from the result of the method [landing.repo.getList](./landing-repo-get-list.md) in the `XML_ID` field ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of deleting a block, where:
- `code` — code of the block to be deleted

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "code": "myblockx"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.repo.unregister.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "code": "myblockx",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.repo.unregister.json"
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
        method: 'landing.repo.unregister',
        params: {
          code: 'myblockx',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Block unregistered:', result)
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
      async function unregisterBlock() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.repo.unregister',
            params: {
              code: 'myblockx',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Block unregistered:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', unregisterBlock)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.repo.unregister',
                [
                    'code' => 'myblockx',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error unregistering block: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.repo.unregister',
        {
            code: 'myblockx'
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
        'landing.repo.unregister',
        [
            'code' => 'myblockx',
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
        "start": 1774951693,
        "finish": 1774951693.523505,
        "duration": 0.5235049724578857,
        "processing": 0,
        "date_start": "2026-03-31T13:08:13+02:00",
        "date_finish": "2026-03-31T13:08:13+02:00",
        "operating_reset_at": 1774952293,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the deletion:

- `true` — block found and deleted
- `false` — `code` is not a string, empty, or block not found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "The value of an argument 'code' has an invalid type",
    "argument": "code"
}
```

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Insufficient permissions."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `MISSING_PARAMS` | Not enough call parameters, missing: code | Method call without `code` ||
|| `ERROR_ARGUMENT` | The value of an argument 'code' has an invalid type | Parameter `code` passed in an incorrect type ||
|| `ACCESS_DENIED` | Insufficient permissions | User did not pass general access checks ||
|| `insufficient_scope` | Token lacks sufficient scope | Token does not contain `landing` scope ||
|| `-` | Error deleting block | Failed to delete record from the repository ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-repo-register.md)
- [{#T}](./landing-repo-get-list.md)
- [{#T}](./landing-repo-check-content.md)
- [{#T}](./index.md)