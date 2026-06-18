# Update status sale.status.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the status of an order or delivery.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_status.id`](../data-types.md) | Symbolic identifier of the status ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the status ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../../data-types.md) | Status type:
- `O` — order status
- `D` — delivery status ||
|| **notify**
[`string`](../../data-types.md) | Indicator of whether to send an email notification to the user when the order or delivery transitions to this status.

Possible values:
- `Y` — notify
- `N` — do not notify
 ||
|| **sort**
[`integer`](../../data-types.md) | Sorting ||
|| **color**
[`string`](../../data-types.md) | HEX color code of the status (e.g., `#FF0000`) ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the status.

Can be used for synchronization with the order or delivery status by identifier in an external system
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"MS","fields":{"type":"D","notify":"N","sort":100,"color":"#00FF00","xmlId":"updatedXmlId"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.status.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"MS","fields":{"type":"D","notify":"N","sort":100,"color":"#00FF00","xmlId":"updatedXmlId"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.status.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type StatusUpdateResult = {
      status: {
        color: string
        id: string
        notify: string
        sort: number
        type: string
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<StatusUpdateResult>({
        method: 'sale.status.update',
        params: {
          id: 'MS',
          fields: {
            type: 'D',
            notify: 'N',
            sort: 100,
            color: '#00FF00',
            xmlId: 'updatedXmlId',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.status)
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
      async function updateStatus() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.status.update',
            params: {
              id: 'MS',
              fields: {
                type: 'D',
                notify: 'N',
                sort: 100,
                color: '#00FF00',
                xmlId: 'updatedXmlId',
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
          console.info(result.status)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateStatus)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.status.update',
                [
                    'id' => 'MS',
                    'fields' => [
                        'type'   => 'D',
                        'notify' => 'N',
                        'sort'   => 100,
                        'color'  => '#00FF00',
                        'xmlId'  => 'updatedXmlId',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating sale status: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.status.update', {
            id: 'MS',
            fields: {
                type: 'D',
                notify: 'N',
                sort: 100,
                color: '#00FF00',
                xmlId: 'updatedXmlId',
            }
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

    $result = CRest::call('sale.status.update', [
        'id' => 'MS',
        'fields' => [
            'type' => 'D',
            'notify' => 'N',
            'sort' => 100,
            'color' => '#00FF00',
            'xmlId' => 'updatedXmlId',
        ]
    ]);

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "status": {
            "color": "#00FF00",
            "id": "MS",
            "notify": "N",
            "sort": 100,
            "type": "D",
            "xmlId": "updatedXmlId"
        }
    },
    "time": {
        "start": 1712142575.655577,
        "finish": 1712142575.940234,
        "duration": 0.28465700149536133,
        "processing": 0.02240896224975586,
        "date_start": "2024-04-03T14:09:35+02:00",
        "date_finish": "2024-04-03T14:09:35+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **status**
[`sale_status`](../data-types.md) | Object with information about the updated status ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description": "Required fields: type"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201340400001` | Status with the provided identifier not found ||
|| `200040300020` | Insufficient permissions to update the status ||
|| `201350000003` | Status type value not provided or the provided value is incorrect ||
|| `201350000006` | Error occurs when trying to change the type of certain [system statuses](./index.md):

- statuses `F` and `N` must always have type `O` (order)
- statuses `DF` and `DN` must always have type `D` (delivery)
||
|| `201350000007` | Error occurs when trying to change the order status type if orders are already attached to this status ||
|| `201350000008` | Error occurs when trying to change the delivery status type if deliveries are already attached to this status ||
|| `100` | Parameter `id` not specified ||
|| `100` | Parameter `fields` not specified or empty ||
|| `0` | Required fields of the `fields` structure not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-status-add.md)
- [{#T}](./sale-status-get.md)
- [{#T}](./sale-status-list.md)
- [{#T}](./sale-status-delete.md)
- [{#T}](./sale-status-get-fields.md)
