# Delete Field from Universal List lists.field.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Full access" permission for the required list

The method `lists.field.delete` removes a field from the list.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IBLOCK_TYPE_ID*** 
[`string`](../../data-types.md) | Identifier of the information block type. Possible values: 
- `lists` — list information block type 
- `bitrix_processes` — processes information block type 
- `lists_socnet` — group lists information block type 

The identifier can be obtained using the method [lists.get.iblock.type.id](../lists/lists-get-iblock-type-id.md) ||
|| **IBLOCK_ID*** 
[`integer`](../../data-types.md) | Identifier of the information block.

The identifier can be obtained using the method [lists.get](../lists/lists-get.md) ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block.

The code can be obtained using the method [lists.get](../lists/lists-get.md)

{% note info "" %}

At least one of the parameters must be specified: `IBLOCK_ID` or `IBLOCK_CODE`

{% endnote %} ||
|| **FIELD_ID*** 
[`string`](../../data-types.md) | Identifier of the field. For a custom field, it appears as `PROPERTY_PropertyId`. For a system field, it is its symbolic code.

The identifier can be obtained using the method [lists.field.get](./lists-field-get.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"123","FIELD_ID":"PROPERTY_1151"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.field.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"123","FIELD_ID":"PROPERTY_1151","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.field.delete
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
        method: 'lists.field.delete',
        params: {
          IBLOCK_TYPE_ID: 'lists',
          IBLOCK_ID: '123',
          FIELD_ID: 'PROPERTY_1151',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field deleted successfully:', result)
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
      async function deleteListField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'lists.field.delete',
            params: {
              IBLOCK_TYPE_ID: 'lists',
              IBLOCK_ID: '123',
              FIELD_ID: 'PROPERTY_1151',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Field deleted successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteListField)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'lists.field.delete',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => '123',
                    'FIELD_ID' => 'PROPERTY_1151'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'lists.field.delete', 
        {
            'IBLOCK_TYPE_ID': 'lists',      
            'IBLOCK_ID': '123',             
            'FIELD_ID': 'PROPERTY_1151'     
        }, 
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.field.delete',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => '123',
            'FIELD_ID' => 'PROPERTY_1151'
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
    "result": true,
    "time": {
        "start": 1765377563,
        "finish": 1765377563.864951,
        "duration": 0.8649508953094482,
        "processing": 0,
        "date_start": "2025-12-10T13:39:23+01:00",
        "date_finish": "2025-12-10T13:39:23+01:00",
        "operating_reset_at": 1765378163,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the field was successfully deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_REQUIRED_PARAMETERS_MISSING",
    "error_description":"Required parameter `X` is missing"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter not provided ||
|| `ERROR_IBLOCK_NOT_FOUND` | Iblock not found | Information block not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to delete the field ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./lists-field-add.md)
- [{#T}](./lists-field-update.md)
- [{#T}](./lists-field-get.md)
- [{#T}](./lists-field-type-get.md)
