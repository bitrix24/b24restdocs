# Get the list of deliveries for a CRM entity crm.item.delivery.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires read access permission for the crm entity from which deliveries are selected.

This method retrieves the list of deliveries for a specific crm entity.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityId***
[`integer`](../../../data-types.md) | Identifier of the crm entity ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the [`crm entity type`](../../data-types.md#crm-entity-type)  ||
|| **filter**
[`object`](../../../data-types.md) | Additional filter for cases when you need to get not all deliveries of the crm entity, but based on a more specific filter. 
The format of the `filter` parameter corresponds to what is described in the [`sale.shipment.list`](../../../sale/shipment/sale-shipment-list.md) method ||
|| **order**
[`object`](../../../data-types.md) | The format of the `order` parameter corresponds to what is described in the [`sale.shipment.list`](../../../sale/shipment/sale-shipment-list.md) method ||

|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityId":13127,"entityTypeId":2,"filter":{"@id":[4077,4078]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.delivery.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityId":13127,"entityTypeId":2,"filter":{"@id":[4077,4078]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.delivery.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each DeliveryItem returned in result[]
    type DeliveryItem = {
      id: number
      accountNumber: string
      deducted: string
      dateDeducted: ISODate | null
      deliveryId: number
      priceDelivery: number
      currency: string
      deliveryName: string
    }

    try {
      // crm.item.delivery.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<DeliveryItem[]>({
        method: 'crm.item.delivery.list',
        params: {
          entityId: 13127,
          entityTypeId: 2,
          filter: {
            '@id': [4077, 4078],
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Deliveries fetched:', result.length, result)
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
      async function listDeliveries() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.item.delivery.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.delivery.list',
            params: {
              entityId: 13127,
              entityTypeId: 2,
              filter: {
                '@id': [4077, 4078],
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Deliveries fetched:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listDeliveries)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.delivery.list',
                [
                    'entityId'     => 13127,
                    'entityTypeId' => 2,
                    'filter'       => [
                        '@id' => [4077, 4078]
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching delivery list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.delivery.list', {
            entityId: 13127,
            entityTypeId: 2,
            filter: {
                "@id": [4077, 4078]
            },
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
        'crm.item.delivery.list',
        [
            'entityId' => 13127,
            'entityTypeId' => 2,
            'filter' => [
                "@id" => [4077, 4078]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP status: **200**

```json
{
   "result":[
      {
         "id":4077,
         "accountNumber":"3657\/2",
         "deducted":"N",
         "dateDeducted":null,
         "deliveryId":228,
         "priceDelivery":79.99,
         "currency":"USD",
         "deliveryName":"Uber Taxi (Cargo)"
      },
      {
         "id":4078,
         "accountNumber":"3657\/3",
         "deducted":"N",
         "dateDeducted":null,
         "deliveryId":228,
         "priceDelivery":79.99,
         "currency":"USD",
         "deliveryName":"Uber Taxi (Cargo)"
      }
   ],
   "time":{
      "start":1716369036.246855,
      "finish":1716369036.734466,
      "duration":0.4876110553741455,
      "processing":0.18442106246948242,
      "date_start":"2024-05-22T12:10:36+02:00",
      "date_finish":"2024-05-22T12:10:36+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_order_shipment_crm_simple`](./crm-item-delivery-get.md#sale_order_shipment_crm_simple) | Array of objects containing brief information about the selected deliveries ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":0,
   "error_description":"Insufficient permissions"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | Access denied ||
|| `100` | Required fields not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-delivery-get.md)

