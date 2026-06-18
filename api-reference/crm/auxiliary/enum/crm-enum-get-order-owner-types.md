# Get Object Types for Order Binding crm.enum.getorderownertypes

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.enum.getorderownertypes` returns a list of object types to which an order can be bound. Use the `id` of the object type as the value for the `ownerTypeId` parameter in the methods [crm.orderentity.*](../../universal/order-entity/crm-order-entity-add.md).

{% note info " " %}

Currently, an [order binding](../../universal/order-entity/crm-order-entity-add.md) can only be done to a [deal](../../deals/index.md).

{% endnote %}

## Method Parameters

No parameters.

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.enum.getorderownertypes
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/crm.enum.getorderownertypes
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each OrderOwnerType returned in result[]
    type OrderOwnerType = {
      attribute: string
      code: string
      id: number
      name: string
    }

    try {
      const response = await $b24.actions.v2.call.make<OrderOwnerType[]>({
        method: 'crm.enum.getorderownertypes',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Order owner types:', result.map(t => `[${t.id}] ${t.code} (${t.attribute})`))
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
      async function getOrderOwnerTypes() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.enum.getorderownertypes',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Order owner types:', result.map(t => `[${t.id}] ${t.code} (${t.attribute})`))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getOrderOwnerTypes)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.enum.getorderownertypes',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching order owner types: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.enum.getorderownertypes",
        {},
        function(result) {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.enum.getorderownertypes',
        []
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
"result": [
    {
     "attribute": "DYN",
     "code": "DEAL",
     "id": 2,
     "name": "Deal"
    }
],
"time": {
    "start": 1750152924.723585,
    "finish": 1750152924.781251,
    "duration": 0.05766606330871582,
    "processing": 0.020370960235595703,
    "date_start": "2025-06-17T12:35:24+02:00",
    "date_finish": "2025-06-17T12:35:24+02:00",
    "operating_reset_at": 1750153524,
    "operating": 0
}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md) | Array of object types [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result Array {#result}

#|
|| **Name**
`type` | **Description** ||
|| **attribute**
[`string`](../../../data-types.md) | Object type attribute ||
|| **code**
[`string`](../../../data-types.md) | Object type code ||
|| **id**
[`integer`](../../../data-types.md) | Object type identifier ||
|| **name**
[`string`](../../../data-types.md) | Object type name ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../../universal/order-entity/crm-order-entity-add.md)