# Get the list of orders sale.order.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.order.list` retrieves a list of orders.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to be selected (see fields of the [sale_order](../data-types.md#sale_order) object).

If not provided or an empty array is passed, all available order fields will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected orders in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.
 
Possible values for `field` correspond to the fields of the [sale_order](../data-types.md#sale_order) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:

- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@`— NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The `%` character in the filter value should not be passed. The search looks for a substring in any position of the string.
- `=%` — LIKE, substring search. The `%` character should be passed in the value. Examples:
    - "mol%" — searching for values starting with "mol"
    - "%mol" — searching for values ending with "mol"
    - "%mol%" — searching for values where "mol" can be in any position.

- `%=` — LIKE (see description above)

- `!%` — NOT LIKE, substring search. The `%` character in the filter value should not be passed. The search goes from both sides.

- `!=%` — NOT LIKE, substring search. The `%` character should be passed in the value. Examples:
    - "mol%" — searching for values not starting with "mol"
    - "%mol" — searching for values not ending with "mol"
    - "%mol%" — searching for values where the substring "mol" is not present in any position.

- `!%=` — NOT LIKE (see description above)

- `=` — equal, exact match (used by default)
- `!=` - not equal
- `!` — not equal ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected orders in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.
 
Possible values for `field` correspond to the fields of the [sale_order](../data-types.md#sale_order).
 
Possible values for `order`:

- asc — ascending order
- desc — descending order
 ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.
 
The page size of results is always static: 50 records.
 
To select the second page of results, the value `50` must be passed. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the `start` parameter value:
 
`start = (N-1) * 50`, where `N` is the desired page number
 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","lid","dateInsert","dateUpdate","personTypeId","personTypeXmlId","statusId","dateStatus","empStatusId","marked","dateMarked","empMarkedId","reasonMarked","price","discountValue","taxValue","userDescription","additionalInfo","comments","companyId","responsibleId","recurringId","lockedBy","dateLock","recountFlag","affiliateId","updated1c","orderTopic","xmlId","statusXmlId","id1c","version","version1c","externalOrder","canceled","dateCanceled","empCanceledId","reasonCanceled","userId","currency","accountNumber","payed","deducted"],"filter":{"<id":10,"@personTypeId":[3,4],"payed":"N"},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.order.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","lid","dateInsert","dateUpdate","personTypeId","personTypeXmlId","statusId","dateStatus","empStatusId","marked","dateMarked","empMarkedId","reasonMarked","price","discountValue","taxValue","userDescription","additionalInfo","comments","companyId","responsibleId","recurringId","lockedBy","dateLock","recountFlag","affiliateId","updated1c","orderTopic","xmlId","statusXmlId","id1c","version","version1c","externalOrder","canceled","dateCanceled","empCanceledId","reasonCanceled","userId","currency","accountNumber","payed","deducted"],"filter":{"<id":10,"@personTypeId":[3,4],"payed":"N"},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.order.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SaleOrderListResult = {
      orders: {
        id: number
        accountNumber: string
        additionalInfo: string
        affiliateId: number | null
        canceled: string
        comments: string
        companyId: number | null
        currency: string
        dateCanceled: ISODate | null
        dateInsert: ISODate
        dateLock: ISODate | null
        dateMarked: ISODate | null
        dateStatus: ISODate
        dateUpdate: ISODate
        deducted: string
        discountValue: number
        empCanceledId: number | null
        empMarkedId: number | null
        empStatusId: number
        externalOrder: string
        id1c: string
        lid: string
        lockedBy: string
        marked: string
        orderTopic: string
        payed: string
        personTypeId: number
        personTypeXmlId: string
        price: number
        reasonCanceled: string
        reasonMarked: string
        recountFlag: string
        recurringId: string
        responsibleId: number
        statusId: string
        statusXmlId: string
        taxValue: number
        updated1c: string
        userDescription: string
        userId: number
        version: number
        version1c: string
        xmlId: string
      }[]
    }

    try {
      // sale.order.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<SaleOrderListResult>({
        method: 'sale.order.list',
        params: {
          select: [
            'id',
            'lid',
            'dateInsert',
            'dateUpdate',
            'personTypeId',
            'personTypeXmlId',
            'statusId',
            'dateStatus',
            'empStatusId',
            'marked',
            'dateMarked',
            'empMarkedId',
            'reasonMarked',
            'price',
            'discountValue',
            'taxValue',
            'userDescription',
            'additionalInfo',
            'comments',
            'companyId',
            'responsibleId',
            'recurringId',
            'lockedBy',
            'dateLock',
            'recountFlag',
            'affiliateId',
            'updated1c',
            'orderTopic',
            'xmlId',
            'statusXmlId',
            'id1c',
            'version',
            'version1c',
            'externalOrder',
            'canceled',
            'dateCanceled',
            'empCanceledId',
            'reasonCanceled',
            'userId',
            'currency',
            'accountNumber',
            'payed',
            'deducted',
          ],
          filter: {
            '<id': 10,
            '@personTypeId': [3, 4],
            payed: 'N',
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
        console.info(`Fetched ${result.orders.length} orders, first order id: ${result.orders[0]?.id}`)
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
      async function fetchOrderList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.order.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.order.list',
            params: {
              select: [
                'id',
                'lid',
                'dateInsert',
                'dateUpdate',
                'personTypeId',
                'personTypeXmlId',
                'statusId',
                'dateStatus',
                'empStatusId',
                'marked',
                'dateMarked',
                'empMarkedId',
                'reasonMarked',
                'price',
                'discountValue',
                'taxValue',
                'userDescription',
                'additionalInfo',
                'comments',
                'companyId',
                'responsibleId',
                'recurringId',
                'lockedBy',
                'dateLock',
                'recountFlag',
                'affiliateId',
                'updated1c',
                'orderTopic',
                'xmlId',
                'statusXmlId',
                'id1c',
                'version',
                'version1c',
                'externalOrder',
                'canceled',
                'dateCanceled',
                'empCanceledId',
                'reasonCanceled',
                'userId',
                'currency',
                'accountNumber',
                'payed',
                'deducted',
              ],
              filter: {
                '<id': 10,
                '@personTypeId': [3, 4],
                payed: 'N',
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
          console.info(`Fetched ${result.orders.length} orders, first order id: ${result.orders[0]?.id}`)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchOrderList)
    </script>
    ```

- PHP


    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.order.list',
                [
                    'select' => [
                        'id',
                        'lid',
                        'dateInsert',
                        'dateUpdate',
                        'personTypeId',
                        'personTypeXmlId',
                        'statusId',
                        'dateStatus',
                        'empStatusId',
                        'marked',
                        'dateMarked',
                        'empMarkedId',
                        'reasonMarked',
                        'price',
                        'discountValue',
                        'taxValue',
                        'userDescription',
                        'additionalInfo',
                        'comments',
                        'companyId',
                        'responsibleId',
                        'recurringId',
                        'lockedBy',
                        'dateLock',
                        'recountFlag',
                        'affiliateId',
                        'updated1c',
                        'orderTopic',
                        'xmlId',
                        'statusXmlId',
                        'id1c',
                        'version',
                        'version1c',
                        'externalOrder',
                        'canceled',
                        'dateCanceled',
                        'empCanceledId',
                        'reasonCanceled',
                        'userId',
                        'currency',
                        'accountNumber',
                        'payed',
                        'deducted',
                    ],
                    'filter' => [
                        '<id'          => 10,
                        '@personTypeId' => [3, 4],
                        'payed'        => 'N',
                    ],
                    'order' => [
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
        echo 'Error fetching order list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.order.list", {
            "select": [
                "id",
                "lid",
                "dateInsert",
                "dateUpdate",
                "personTypeId",
                "personTypeXmlId",
                "statusId",
                "dateStatus",
                "empStatusId",
                "marked",
                "dateMarked",
                "empMarkedId",
                "reasonMarked",
                "price",
                "discountValue",
                "taxValue",
                "userDescription",
                "additionalInfo",
                "comments",
                "companyId",
                "responsibleId",
                "recurringId",
                "lockedBy",
                "dateLock",
                "recountFlag",
                "affiliateId",
                "updated1c",
                "orderTopic",
                "xmlId",
                "statusXmlId",
                "id1c",
                "version",
                "version1c",
                "externalOrder",
                "canceled",
                "dateCanceled",
                "empCanceledId",
                "reasonCanceled",
                "userId",
                "currency",
                "accountNumber",
                "payed",
                "deducted",
            ],
            "filter": {
                "<id": 10,
                "@personTypeId": [3, 4],
                "payed": "N",
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
        'sale.order.list',
        [
            'select' => [
                "id",
                "lid",
                "dateInsert",
                "dateUpdate",
                "personTypeId",
                "personTypeXmlId",
                "statusId",
                "dateStatus",
                "empStatusId",
                "marked",
                "dateMarked",
                "empMarkedId",
                "reasonMarked",
                "price",
                "discountValue",
                "taxValue",
                "userDescription",
                "additionalInfo",
                "comments",
                "companyId",
                "responsibleId",
                "recurringId",
                "lockedBy",
                "dateLock",
                "recountFlag",
                "affiliateId",
                "updated1c",
                "orderTopic",
                "xmlId",
                "statusXmlId",
                "id1c",
                "version",
                "version1c",
                "externalOrder",
                "canceled",
                "dateCanceled",
                "empCanceledId",
                "reasonCanceled",
                "userId",
                "currency",
                "accountNumber",
                "payed",
                "deducted",
            ],
            'filter' => [
                "<id" => 10,
                "@personTypeId" => [3, 4],
                "payed" => "N",
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

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "orders": [
            {
                "accountNumber": "165",
                "additionalInfo": "",
                "affiliateId": null,
                "canceled": "N",
                "comments": "",
                "companyId": null,
                "currency": "USD",
                "dateCanceled": null,
                "dateInsert": "2022-10-14T17:19:11+02:00",
                "dateLock": null,
                "dateMarked": null,
                "dateStatus": "2022-10-14T17:19:03+02:00",
                "dateUpdate": "2022-10-14T17:19:11+02:00",
                "deducted": "N",
                "discountValue": 0,
                "empCanceledId": null,
                "empMarkedId": null,
                "empStatusId": 1,
                "externalOrder": "N",
                "id": 9,
                "id1c": "",
                "lid": "s1",
                "lockedBy": "",
                "marked": "N",
                "orderTopic": "",
                "payed": "N",
                "personTypeId": 4,
                "personTypeXmlId": "",
                "price": 1176,
                "reasonCanceled": "",
                "reasonMarked": "",
                "recountFlag": "Y",
                "recurringId": "",
                "responsibleId": 1,
                "statusId": "N",
                "statusXmlId": "",
                "taxValue": 196,
                "updated1c": "N",
                "userDescription": "",
                "userId": 2,
                "version": 0,
                "version1c": "",
                "xmlId": "bx_63498bf7c8d31"
            },
        ]
    },
    "total": 1,
    "time": {
        "start": 1712847891.436862,
        "finish": 1712847892.028163,
        "duration": 0.5913009643554688,
        "processing": 0.1332709789276123,
        "date_start": "2024-04-11T18:04:51+02:00",
        "date_finish": "2024-04-11T18:04:52+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **orders**
[`sale_order[]`](../data-types.md) | An array of objects containing information about the selected orders ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

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
|| `200040300010` | Insufficient permissions to read orders ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-order-add.md)
- [{#T}](./sale-order-update.md)
- [{#T}](./sale-order-get.md)
- [{#T}](./sale-order-delete.md)
- [{#T}](./sale-order-get-fields.md)

