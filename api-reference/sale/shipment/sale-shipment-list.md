# Get the list of shipments sale.shipment.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of shipments.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the [sale_order_shipment](../data-types.md) object).
 ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected payment bindings to shipments in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.
 
Possible values for `field` correspond to the fields of the [sale_order_shipment](../data-types.md) object.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:

- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The % symbol in the filter value should not be passed. The search looks for the substring in any position of the string.
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol in the filter value should not be passed. The search goes from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol should be passed in the value. Examples: 
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol should be passed in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected payment bindings to shipments in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.
 
Possible values for `field` correspond to the fields of the [sale_order_shipment](../data-types.md) object.
 
Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.
 
The page size of results is always static: 50 records.
 
To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the `start` parameter value:
 
`start = (N-1) * 50`, where `N` — the desired page number
 ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","accountNumber","allowDelivery","basePriceDelivery","canceled","comments","companyId","currency","customPriceDelivery","dateAllowDelivery","dateCanceled","dateDeducted","dateInsert","dateMarked","dateResponsibleId","deducted","deliveryDocDate","deliveryDocNum","deliveryId","deliveryName","deliveryXmlId","discountPrice","empAllowDeliveryId","empCanceledId","empDeductedId","empMarkedId","empResponsibleId","externalDelivery","id1c","marked","orderId","priceDelivery","reasonMarked","reasonUndoDeducted","responsibleId","statusId","statusXmlId","system","trackingDescription","trackingLastCheck","trackingNumber","trackingStatus","updated1c","version1c","xmlId"],"filter":{"@orderId":[2069,2070],">=id":2464},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipment.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","accountNumber","allowDelivery","basePriceDelivery","canceled","comments","companyId","currency","customPriceDelivery","dateAllowDelivery","dateCanceled","dateDeducted","dateInsert","dateMarked","dateResponsibleId","deducted","deliveryDocDate","deliveryDocNum","deliveryId","deliveryName","deliveryXmlId","discountPrice","empAllowDeliveryId","empCanceledId","empDeductedId","empMarkedId","empResponsibleId","externalDelivery","id1c","marked","orderId","priceDelivery","reasonMarked","reasonUndoDeducted","responsibleId","statusId","statusXmlId","system","trackingDescription","trackingLastCheck","trackingNumber","trackingStatus","updated1c","version1c","xmlId"],"filter":{"@orderId":[2069,2070],">=id":2464},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipment.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ShipmentListResult = {
      shipments: {
        id: number
        accountNumber: string
        allowDelivery: string
        basePriceDelivery: number
        canceled: string
        comments: string
        companyId: number
        currency: string
        customPriceDelivery: string
        dateAllowDelivery: ISODate | null
        dateCanceled: ISODate | null
        dateDeducted: ISODate | null
        dateInsert: ISODate
        dateMarked: ISODate | null
        dateResponsibleId: ISODate
        deducted: string
        deliveryDocDate: ISODate | null
        deliveryDocNum: string
        deliveryId: number
        deliveryName: string
        deliveryXmlId: string
        discountPrice: number | null
        empAllowDeliveryId: number | null
        empCanceledId: number | null
        empDeductedId: number | null
        empMarkedId: number | null
        empResponsibleId: number | null
        externalDelivery: string
        id1c: string
        marked: string
        orderId: number
        priceDelivery: number
        reasonMarked: string
        reasonUndoDeducted: string
        responsibleId: number
        statusId: string
        statusXmlId: string
        system: string
        trackingDescription: string
        trackingLastCheck: string
        trackingNumber: string
        trackingStatus: string
        updated1c: string
        version1c: string
        xmlId: string
      }[]
    }

    try {
      // sale.shipment.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ShipmentListResult>({
        method: 'sale.shipment.list',
        params: {
          select: [
            'id',
            'accountNumber',
            'allowDelivery',
            'basePriceDelivery',
            'canceled',
            'comments',
            'companyId',
            'currency',
            'customPriceDelivery',
            'dateAllowDelivery',
            'dateCanceled',
            'dateDeducted',
            'dateInsert',
            'dateMarked',
            'dateResponsibleId',
            'deducted',
            'deliveryDocDate',
            'deliveryDocNum',
            'deliveryId',
            'deliveryName',
            'deliveryXmlId',
            'discountPrice',
            'empAllowDeliveryId',
            'empCanceledId',
            'empDeductedId',
            'empMarkedId',
            'empResponsibleId',
            'externalDelivery',
            'id1c',
            'marked',
            'orderId',
            'priceDelivery',
            'reasonMarked',
            'reasonUndoDeducted',
            'responsibleId',
            'statusId',
            'statusXmlId',
            'system',
            'trackingDescription',
            'trackingLastCheck',
            'trackingNumber',
            'trackingStatus',
            'updated1c',
            'version1c',
            'xmlId',
          ],
          filter: {
            '>=id': 2464,
            '@orderId': [2069, 2070],
          },
          order: {
            id: 'desc',
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
        console.info('Shipments fetched:', result.shipments.length, result.shipments)
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
      async function fetchShipmentList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.shipment.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.shipment.list',
            params: {
              select: [
                'id',
                'accountNumber',
                'allowDelivery',
                'basePriceDelivery',
                'canceled',
                'comments',
                'companyId',
                'currency',
                'customPriceDelivery',
                'dateAllowDelivery',
                'dateCanceled',
                'dateDeducted',
                'dateInsert',
                'dateMarked',
                'dateResponsibleId',
                'deducted',
                'deliveryDocDate',
                'deliveryDocNum',
                'deliveryId',
                'deliveryName',
                'deliveryXmlId',
                'discountPrice',
                'empAllowDeliveryId',
                'empCanceledId',
                'empDeductedId',
                'empMarkedId',
                'empResponsibleId',
                'externalDelivery',
                'id1c',
                'marked',
                'orderId',
                'priceDelivery',
                'reasonMarked',
                'reasonUndoDeducted',
                'responsibleId',
                'statusId',
                'statusXmlId',
                'system',
                'trackingDescription',
                'trackingLastCheck',
                'trackingNumber',
                'trackingStatus',
                'updated1c',
                'version1c',
                'xmlId',
              ],
              filter: {
                '>=id': 2464,
                '@orderId': [2069, 2070],
              },
              order: {
                id: 'desc',
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
          console.info('Shipments fetched:', result.shipments.length, result.shipments)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchShipmentList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.shipment.list',
                [
                    'select' => [
                        'id',
                        'accountNumber',
                        'allowDelivery',
                        'basePriceDelivery',
                        'canceled',
                        'comments',
                        'companyId',
                        'currency',
                        'customPriceDelivery',
                        'dateAllowDelivery',
                        'dateCanceled',
                        'dateDeducted',
                        'dateInsert',
                        'dateMarked',
                        'dateResponsibleId',
                        'deducted',
                        'deliveryDocDate',
                        'deliveryDocNum',
                        'deliveryId',
                        'deliveryName',
                        'deliveryXmlId',
                        'discountPrice',
                        'empAllowDeliveryId',
                        'empCanceledId',
                        'empDeductedId',
                        'empMarkedId',
                        'empResponsibleId',
                        'externalDelivery',
                        'id1c',
                        'marked',
                        'orderId',
                        'priceDelivery',
                        'reasonMarked',
                        'reasonUndoDeducted',
                        'responsibleId',
                        'statusId',
                        'statusXmlId',
                        'system',
                        'trackingDescription',
                        'trackingLastCheck',
                        'trackingNumber',
                        'trackingStatus',
                        'updated1c',
                        'version1c',
                        'xmlId',
                    ],
                    'filter' => [
                        '>=id'     => 2464,
                        '@orderId' => [2069, 2070],
                    ],
                    'order'  => [
                        'id' => 'desc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching shipment list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.shipment.list", {
            "select": [
                "id",
                "accountNumber",
                "allowDelivery",
                "basePriceDelivery",
                "canceled",
                "comments",
                "companyId",
                "currency",
                "customPriceDelivery",
                "dateAllowDelivery",
                "dateCanceled",
                "dateDeducted",
                "dateInsert",
                "dateMarked",
                "dateResponsibleId",
                "deducted",
                "deliveryDocDate",
                "deliveryDocNum",
                "deliveryId",
                "deliveryName",
                "deliveryXmlId",
                "discountPrice",
                "empAllowDeliveryId",
                "empCanceledId",
                "empDeductedId",
                "empMarkedId",
                "empResponsibleId",
                "externalDelivery",
                "id1c",
                "marked",
                "orderId",
                "priceDelivery",
                "reasonMarked",
                "reasonUndoDeducted",
                "responsibleId",
                "statusId",
                "statusXmlId",
                "system",
                "trackingDescription",
                "trackingLastCheck",
                "trackingNumber",
                "trackingStatus",
                "updated1c",
                "version1c",
                "xmlId",
            ],
            "filter": {
                ">=id": 2464,
                "@orderId": [2069, 2070],
            },
            "order": {
                "id": "desc",
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

    $result = CRest::call(
        'sale.shipment.list',
        [
            'select' => [
                "id",
                "accountNumber",
                "allowDelivery",
                "basePriceDelivery",
                "canceled",
                "comments",
                "companyId",
                "currency",
                "customPriceDelivery",
                "dateAllowDelivery",
                "dateCanceled",
                "dateDeducted",
                "dateInsert",
                "dateMarked",
                "dateResponsibleId",
                "deducted",
                "deliveryDocDate",
                "deliveryDocNum",
                "deliveryId",
                "deliveryName",
                "deliveryXmlId",
                "discountPrice",
                "empAllowDeliveryId",
                "empCanceledId",
                "empDeductedId",
                "empMarkedId",
                "empResponsibleId",
                "externalDelivery",
                "id1c",
                "marked",
                "orderId",
                "priceDelivery",
                "reasonMarked",
                "reasonUndoDeducted",
                "responsibleId",
                "statusId",
                "statusXmlId",
                "system",
                "trackingDescription",
                "trackingLastCheck",
                "trackingNumber",
                "trackingStatus",
                "updated1c",
                "version1c",
                "xmlId",
            ],
            'filter' => [
                ">=id" => 2464,
                "@orderId" => [2069, 2070],
            ],
            'order' => [
                "id" => "desc",
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP Status: **200**

```json
{
   "result":{
      "shipments":[
         {
            "accountNumber":"2070\/2",
            "allowDelivery":"N",
            "basePriceDelivery":0,
            "canceled":"N",
            "comments":"",
            "companyId":0,
            "currency":"USD",
            "customPriceDelivery":"N",
            "dateAllowDelivery":null,
            "dateCanceled":null,
            "dateDeducted":null,
            "dateInsert":"2024-04-11T16:59:20+02:00",
            "dateMarked":null,
            "dateResponsibleId":"2024-04-11T16:59:20+02:00",
            "deducted":"N",
            "deliveryDocDate":null,
            "deliveryDocNum":"",
            "deliveryId":1,
            "deliveryName":"No delivery",
            "deliveryXmlId":"",
            "discountPrice":null,
            "empAllowDeliveryId":null,
            "empCanceledId":null,
            "empDeductedId":null,
            "empMarkedId":null,
            "empResponsibleId":null,
            "externalDelivery":"N",
            "id":2467,
            "id1c":"",
            "marked":"N",
            "orderId":2070,
            "priceDelivery":0,
            "reasonMarked":"",
            "reasonUndoDeducted":"",
            "responsibleId":0,
            "statusId":"DN",
            "statusXmlId":"FFdddd",
            "system":"N",
            "trackingDescription":"",
            "trackingLastCheck":"",
            "trackingNumber":"",
            "trackingStatus":"",
            "updated1c":"N",
            "version1c":"",
            "xmlId":"bx_6617fac818ee2"
         },
         {
            "accountNumber":"2069\/2",
            "allowDelivery":"N",
            "basePriceDelivery":600,
            "canceled":"N",
            "comments":"",
            "companyId":0,
            "currency":"USD",
            "customPriceDelivery":"N",
            "dateAllowDelivery":null,
            "dateCanceled":null,
            "dateDeducted":null,
            "dateInsert":"2024-04-11T15:05:48+02:00",
            "dateMarked":null,
            "dateResponsibleId":"2024-04-11T15:05:48+02:00",
            "deducted":"N",
            "deliveryDocDate":null,
            "deliveryDocNum":"",
            "deliveryId":2,
            "deliveryName":"Courier delivery",
            "deliveryXmlId":"",
            "discountPrice":null,
            "empAllowDeliveryId":null,
            "empCanceledId":null,
            "empDeductedId":null,
            "empMarkedId":null,
            "empResponsibleId":null,
            "externalDelivery":"N",
            "id":2465,
            "id1c":"",
            "marked":"N",
            "orderId":2069,
            "priceDelivery":600,
            "reasonMarked":"",
            "reasonUndoDeducted":"",
            "responsibleId":0,
            "statusId":"DN",
            "statusXmlId":"FFdddd",
            "system":"N",
            "trackingDescription":"",
            "trackingLastCheck":"",
            "trackingNumber":"",
            "trackingStatus":"",
            "updated1c":"N",
            "version1c":"",
            "xmlId":"bx_6617e02cae2a1"
         }
      ]
   },
   "total":2,
   "time":{
      "start":1712847615.641893,
      "finish":1712847615.857499,
      "duration":0.2156059741973877,
      "processing":0.02844095230102539,
      "date_start":"2024-04-11T18:00:15+02:00",
      "date_finish":"2024-04-11T18:00:15+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **shipments**
[`sale_order_shipment[]`](../data-types.md) | An array of objects with information about the selected shipments ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
   "error":0,
   "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read shipments ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-add.md)
- [{#T}](./sale-shipment-update.md)
- [{#T}](./sale-shipment-get.md)
- [{#T}](./sale-shipment-delete.md)
- [{#T}](./sale-shipment-get-fields.md)

