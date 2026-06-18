# Get Property Value with sale.propertyvalue.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves the property value by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **id*** 
[`sale_order_property_value.id`](../data-types.md) | Identifier of the order property value ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":13176}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyvalue.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":13176,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyvalue.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type GetPropertyValueResult = {
      propertyValue: {
        code: string
        id: number
        name: string
        orderPropsId: number
        value: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<GetPropertyValueResult>({
        method: 'sale.propertyvalue.get',
        params: {
          id: 13176,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.propertyValue.id, result.propertyValue.code, result.propertyValue.value)
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
      async function getPropertyValue() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.propertyvalue.get',
            params: {
              id: 13176,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.propertyValue.id, result.propertyValue.code, result.propertyValue.value)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getPropertyValue)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.propertyvalue.get',
                [
                    'id' => 13176,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting sale property value: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.propertyvalue.get", {
            "id": 13176
        },
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
        'sale.propertyvalue.get',
        [
            'id' => 13176
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "propertyValue": {
            "code": "FIO",
            "id": 13176,
            "name": "First Last",
            "orderPropsId": 20,
            "value": "John Smith"
        }
    },
    "time": {
        "start": 1712056406.00744,
        "finish": 1712056406.370423,
        "duration": 0.36298298835754395,
        "processing": 0.0960841178894043,
        "date_start": "2024-04-02T14:13:26+02:00",
        "date_finish": "2024-04-02T14:13:26+02:00"
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **propertyValue**
[`sale_order_property_value`](../data-types.md) | Information about the property value ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 201040400001,
    "error_description": "property value does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `201040400001` | Property value not found ||
|| `200040300010` | Insufficient permissions to read the property value ||
|| `100` | Parameter `id` not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-value-modify.md)
- [{#T}](./sale-property-value-list.md)
- [{#T}](./sale-property-value-delete.md)
- [{#T}](./sale-property-value-get-fields.md)
