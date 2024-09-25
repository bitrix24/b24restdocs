# Add Order sale.order.add

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
[`string`](../../data-types.md) | Currency. The list of currencies can be obtained through the method [crm.currency.list](../../crm/currency/crm-currency-list.md) ||
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
[`string`](../../data-types.md) | Reason why the order was marked ||
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
[`integer`](../../data-types.md) | Identifier of the subscription renewal ||
|| **lockedBy**
[`user.id`](../../data-types.md) | Relevant only for on-premise.

Identifier of the user who locked the order. The order is locked in the admin panel when the user opens the detail form of the order ||
|| **recountFlag**
[`string`](../../data-types.md) | Deprecated.

Recount flag.

- `Y` — yes
- `N` — no

Defaults to Y ||
|| **affiliateId**
[`integer`](../../data-types.md) | Relevant only for on-premise.

Identifier of the affiliate ||
|| **updated1c**
[`string`](../../data-types.md) | Updated via 1C.

- `Y` — yes
- `N` — no

Defaults to `N` ||
|| **orderTopic**
[`string`](../../data-types.md) | Deprecated.

Order topic ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier ||
|| **id1c**
[`string`](../../data-types.md) | Identifier in 1C ||
|| **version1c**
[`string`](../../data-types.md) | Version in 1C ||
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

- JS

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

- PHP

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
[`sale_order`](../data-types.md) | Object with information about the added order ||
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