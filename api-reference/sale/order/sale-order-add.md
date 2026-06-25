# Add Order sale.order.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.order.add` is designed for adding an order.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../data-types.md) | Field values for creating an order ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`string`](../../data-types.md) | Identifier of the site where this payer type will be used. Has a constant value 's1' ||
|| **personTypeId***
[`sale_person_type.id`](../data-types.md) | Identifier of the payer type ||
|| **currency***
[`string`](../../data-types.md) | Currency. The list of currencies can be obtained via the method [crm.currency.list](../../crm/currency/crm-currency-list.md) ||
|| **price**
[`double`](../../data-types.md) | Price ||
|| **discountValue**
[`double`](../../data-types.md) | Discount value ||
|| **statusId**
[`sale_status.id`](../data-types.md) | Identifier of the order status ||
|| **empStatusId**
[`user.id`](../../data-types.md) | Identifier of the user who changed the order status ||
|| **dateInsert**
[`datetime`](../../data-types.md) | Order creation date ||
|| **marked**
[`string`](../../data-types.md) | Marking flag. Indicates whether the shipment is marked as problematic. The value `Y` is set automatically if an error occurred during saving.

- `Y` — yes
- `N` — no

Defaults to `N` ||
|| **empMarkedId**
[`user.id`](../../data-types.md) | Identifier of the user who set the marking ||
|| **reasonMarked**
[`string`](../../data-types.md) | Reason for marking the order ||
|| **userDescription**
[`string`](../../data-types.md) | Customer's comment on the order ||
|| **additionalInfo**
[`string`](../../data-types.md) | Deprecated.

Additional information ||
|| **comments**
[`string`](../../data-types.md) | Manager's comment on the order ||
|| **responsibleId**
[`user.id`](../../data-types.md) | Identifier of the user responsible for the order ||
|| **recurringId**
[`integer`](../../data-types.md) | Identifier for subscription renewal ||
|| **lockedBy**
[`user.id`](../../data-types.md) | Relevant only for on-premise.

Identifier of the user who locked the order. The order is locked in the admin panel when the user opens the detail form of the order ||
|| **recountFlag**
[`string`](../../data-types.md) | Deprecated.

Recount flag.

- `Y` — yes
- `N` — no

Defaults to `Y` ||
|| **affiliateId**
[`integer`](../../data-types.md) | Relevant only for on-premise.

Identifier of the affiliate ||
|| **updated1c**
[`string`](../../data-types.md) | Updated via QuickBooks and other similar platforms.

- `Y` — yes
- `N` — no

Defaults to `N` ||
|| **orderTopic**
[`string`](../../data-types.md) | Deprecated.

Order topic ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier ||
|| **id1c**
[`string`](../../data-types.md) | Identifier in QuickBooks and other similar platforms ||
|| **version1c**
[`string`](../../data-types.md) | Version in QuickBooks and other similar platforms ||
|| **externalOrder**
[`string`](../../data-types.md) | Whether the order is from an external system.

- `Y` — yes
- `N` — no

Defaults to `N` ||
|| **canceled**
[`string`](../../data-types.md) | Whether the order was canceled.

- `Y` — yes
- `N` — no

Defaults to `N` ||
|| **empCanceledId**
[`user.id`](../../data-types.md) | Identifier of the user who canceled the order ||
|| **reasonCanceled**
[`string`](../../data-types.md) | Reason for cancellation ||
|| **userId**
[`user.id`](../../data-types.md) | Identifier of the client ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"lid":"s1","personTypeId":1,"currency":"USD","price":100,"discountValue":10,"statusId":"N","empStatusId":1,"dateInsert":"2024-03-01T14:00:00","marked":"Y","empMarkedId":1,"reasonMarked":"","userDescription":"","additionalInfo":"","comments":"","companyId":1,"responsibleId":1,"recurringId":1,"lockedBy":1,"recountFlag":"N","affiliateId":1,"updated1c":"N","orderTopic":"","xmlId":"","id1c":"","version1c":"","externalOrder":"N","canceled":"Y","empCanceledId":1,"reasonCanceled":"","userId":1}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.order.add
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"lid":"s1","personTypeId":1,"currency":"USD","price":100,"discountValue":10,"statusId":"N","empStatusId":1,"dateInsert":"2024-03-01T14:00:00","marked":"Y","empMarkedId":1,"reasonMarked":"","userDescription":"","additionalInfo":"","comments":"","companyId":1,"responsibleId":1,"recurringId":1,"lockedBy":1,"recountFlag":"N","affiliateId":1,"updated1c":"N","orderTopic":"","xmlId":"","id1c":"","version1c":"","externalOrder":"N","canceled":"Y","empCanceledId":1,"reasonCanceled":"","userId":1},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.order.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type OrderResult = {
      order: {
        accountNumber: string
        additionalInfo: string
        affiliateId: number
        canceled: string
        clients: unknown[]
        comments: string
        companyId: number
        currency: string
        dateCanceled: ISODate | null
        dateInsert: ISODate | null
        dateMarked: ISODate | null
        dateStatus: ISODate | null
        dateUpdate: ISODate | null
        deducted: string
        discountValue: number
        empCanceledId: number
        empMarkedId: number
        empStatusId: number
        externalOrder: string
        id: number
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
        requisiteLink: unknown[]
        responsibleId: number
        statusId: string
        statusXmlId: string
        updated1c: string
        userDescription: string
        userId: number
        version1c: string
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<OrderResult>({
        method: 'sale.order.add',
        params: {
          fields: {
            lid: 's1',
            personTypeId: 1,
            currency: 'USD',
            price: 100,
            discountValue: 10,
            statusId: 'N',
            empStatusId: 1,
            dateInsert: '2024-03-01T14:00:00',
            marked: 'Y',
            empMarkedId: 1,
            reasonMarked: '',
            userDescription: '',
            additionalInfo: '',
            comments: '',
            companyId: 1,
            responsibleId: 1,
            recurringId: 1,
            lockedBy: 1,
            recountFlag: 'N',
            affiliateId: 1,
            updated1c: 'N',
            orderTopic: '',
            xmlId: '',
            id1c: '',
            version1c: '',
            externalOrder: 'N',
            canceled: 'Y',
            empCanceledId: 1,
            reasonCanceled: '',
            userId: 1,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.order.id, result.order.accountNumber)
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
      async function addOrder() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.order.add',
            params: {
              fields: {
                lid: 's1',
                personTypeId: 1,
                currency: 'USD',
                price: 100,
                discountValue: 10,
                statusId: 'N',
                empStatusId: 1,
                dateInsert: '2024-03-01T14:00:00',
                marked: 'Y',
                empMarkedId: 1,
                reasonMarked: '',
                userDescription: '',
                additionalInfo: '',
                comments: '',
                companyId: 1,
                responsibleId: 1,
                recurringId: 1,
                lockedBy: 1,
                recountFlag: 'N',
                affiliateId: 1,
                updated1c: 'N',
                orderTopic: '',
                xmlId: '',
                id1c: '',
                version1c: '',
                externalOrder: 'N',
                canceled: 'Y',
                empCanceledId: 1,
                reasonCanceled: '',
                userId: 1,
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
          console.info(result.order.id, result.order.accountNumber)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addOrder)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.order.add',
                [
                    'fields' => [
                        'lid'            => 's1',
                        'personTypeId'   => 1,
                        'currency'       => 'USD',
                        'price'          => 100,
                        'discountValue'  => 10,
                        'statusId'       => 'N',
                        'empStatusId'    => 1,
                        'dateInsert'     => '2024-03-01T14:00:00',
                        'marked'         => 'Y',
                        'empMarkedId'    => 1,
                        'reasonMarked'   => '',
                        'userDescription' => '',
                        'additionalInfo' => '',
                        'comments'       => '',
                        'companyId'      => 1,
                        'responsibleId'  => 1,
                        'recurringId'    => 1,
                        'lockedBy'       => 1,
                        'recountFlag'    => 'N',
                        'affiliateId'    => 1,
                        'updated1c'      => 'N',
                        'orderTopic'     => '',
                        'xmlId'          => '',
                        'id1c'           => '',
                        'version1c'      => '',
                        'externalOrder'  => 'N',
                        'canceled'       => 'Y',
                        'empCanceledId'  => 1,
                        'reasonCanceled' => '',
                        'userId'         => 1,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding order: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.order.add',
        {
            fields: {
                lid: 's1',
                personTypeId: 1,
                currency: 'USD',
                price: 100,
                discountValue: 10,
                statusId: 'N',
                empStatusId: 1,
                dateInsert: '2024-03-01T14:00:00',
                marked: 'Y',
                empMarkedId: 1,
                reasonMarked: '',
                userDescription: '',
                additionalInfo: '',
                comments: '',
                companyId: 1,
                responsibleId: 1,
                recurringId: 1,
                lockedBy: 1,
                recountFlag: 'N',
                affiliateId: 1,
                updated1c: 'N',
                orderTopic: '',
                xmlId: '',
                id1c: '',
                version1c: '',
                externalOrder: 'N',
                canceled: 'Y',
                empCanceledId: 1,
                reasonCanceled: '',
                userId: 1,
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.order.add',
        [
            'fields' => [
                'lid' => 's1',
                'personTypeId' => 1,
                'currency' => 'USD',
                'price' => 100,
                'discountValue' => 10,
                'statusId' => 'N',
                'empStatusId' => 1,
                'dateInsert' => '2024-03-01T14:00:00',
                'marked' => 'Y',
                'empMarkedId' => 1,
                'reasonMarked' => '',
                'userDescription' => '',
                'additionalInfo' => '',
                'comments' => '',
                'companyId' => 1,
                'responsibleId' => 1,
                'recurringId' => 1,
                'lockedBy' => 1,
                'recountFlag' => 'N',
                'affiliateId' => 1,
                'updated1c' => 'N',
                'orderTopic' => '',
                'xmlId' => '',
                'id1c' => '',
                'version1c' => '',
                'externalOrder' => 'N',
                'canceled' => 'Y',
                'empCanceledId' => 1,
                'reasonCanceled' => '',
                'userId' => 1,
            ]
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
        "order": {
            "accountNumber": "455",
            "additionalInfo": "",
            "affiliateId": 1,
            "canceled": "Y",
            "clients": [],
            "comments": "",
            "companyId": 1,
            "currency": "USD",
            "dateCanceled": "2024-04-12T13:50:21+02:00",
            "dateInsert": "2024-03-01T13:00:00+02:00",
            "dateMarked": "2024-04-12T13:50:21+02:00",
            "dateStatus": "2024-04-12T13:50:21+02:00",
            "dateUpdate": "2024-04-12T13:50:21+02:00",
            "deducted": "N",
            "discountValue": 10,
            "empCanceledId": 1,
            "empMarkedId": 1,
            "empStatusId": 1,
            "externalOrder": "N",
            "id": 299,
            "id1c": "",
            "lid": "s1",
            "lockedBy": "1",
            "marked": "Y",
            "orderTopic": "",
            "payed": "N",
            "personTypeId": 1,
            "personTypeXmlId": "",
            "price": 100,
            "reasonCanceled": "",
            "reasonMarked": "",
            "recountFlag": "N",
            "recurringId": "1",
            "requisiteLink": [],
            "responsibleId": 1,
            "statusId": "N",
            "statusXmlId": "",
            "updated1c": "N",
            "userDescription": "",
            "userId": 1,
            "version1c": "",
            "xmlId": ""
        }
    },
    "time": {
        "start": 1712922620.724857,
        "finish": 1712922623.393783,
        "duration": 2.6689260005950928,
        "processing": 2.210068941116333,
        "date_start": "2024-04-12T14:50:20+02:00",
        "date_finish": "2024-04-12T14:50:23+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **order**
[`sale_order`](../data-types.md) | Object containing information about the added order ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200040300020,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to add an order ||
|| `100` | Parameter `fields` is not specified or is empty ||
|| `0` | Required fields are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-order-update.md)
- [{#T}](./sale-order-get.md)
- [{#T}](./sale-order-list.md)
- [{#T}](./sale-order-delete.md)
- [{#T}](./sale-order-get-fields.md)
