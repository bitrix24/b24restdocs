# Get the information block type ID lists.get.iblock.type.id

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Read" access permission for lists

The method `lists.get.iblock.type.id` returns the information block type ID.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IBLOCK_ID*** 
[`integer`](../../data-types.md) | Information block ID.

The ID can be obtained using the [lists.get](../lists/lists-get.md) method ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block.

The code can be obtained using the [lists.get](../lists/lists-get.md) method

{% note info "" %}

At least one of the parameters must be specified: `IBLOCK_ID` or `IBLOCK_CODE`

{% endnote %} ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_ID":87}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.get.iblock.type.id
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_ID":87,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.get.iblock.type.id
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<string | null>({
        method: 'lists.get.iblock.type.id',
        params: {
          IBLOCK_ID: 87,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Iblock type id:', result)
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
      async function getIblockTypeId() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'lists.get.iblock.type.id',
            params: {
              IBLOCK_ID: 87,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Iblock type id:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getIblockTypeId)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'lists.get.iblock.type.id',
                [
                    'IBLOCK_ID' => 87
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
     'lists.get.iblock.type.id',
     {
        IBLOCK_ID: 87
     },
        function(res) {
            if (res.error()) {
                console.error(res.error());
            } else {
                console.log(res.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.get.iblock.type.id',
        [
            'IBLOCK_ID' => 87
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": "lists",
    "time": {
        "start": 1764775444,
        "finish": 1764775444.66342,
        "duration": 0.6634199619293213,
        "processing": 0,
        "date_start": "2025-12-03T18:24:04+01:00",
        "date_finish": "2025-12-03T18:24:04+01:00",
        "operating_reset_at": 1764776044,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../data-types.md) | Information block type ID.

Returns `null` if the information block is not found or the parameters are incorrect ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-add.md)
- [{#T}](./lists-update.md)
- [{#T}](./lists-get.md)
- [{#T}](./lists-delete.md)
