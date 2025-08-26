# Update Order sale.order.update

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.order.update` updates the fields of an order.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order.id`](../data-types.md) | Order identifier ||
|| **fields***
[`object`](../data-types.md) | Field values for updating the order ||
|#

## Fields Parameter

#|
|| **Name**
`type` | **Description** ||
|| **price**
[`double`](../../data-types.md) | Price ||
|| **discountValue**
[`double`](../../data-types.md) | Discount value ||
|| **statusId**
[`sale_status.id`](../data-types.md) | Order status identifier ||
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
[`integer`](../../data-types.md) | Subscription renewal identifier ||
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

Affiliate identifier ||
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
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":300,"fields":{"price":100,"discountValue":10,"statusId":"N","empStatusId":1,"dateInsert":"2024-03-01T14:00:00","marked":"Y","empMarkedId":1,"reasonMarked":"","userDescription":"","additionalInfo":"","comments":"","companyId":1,"responsibleId":1,"recurringId":1,"lockedBy":1,"recountFlag":"N","affiliateId":1,"updated1c":"N","orderTopic":"","xmlId":"","id1c":"","version1c":"","externalOrder":"N","canceled":"Y","empCanceledId":1,"reasonCanceled":""}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.order.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":300,"fields":{"price":100,"discountValue":10,"statusId":"N","empStatusId":1,"dateInsert":"2024-03-01T14:00:00","marked":"Y","empMarkedId":1,"reasonMarked":"","userDescription":"","additionalInfo":"","comments":"","companyId":1,"responsibleId":1,"recurringId":1,"lockedBy":1,"recountFlag":"N","affiliateId":1,"updated1c":"N","orderTopic":"","xmlId":"","id1c":"","version1c":"","externalOrder":"N","canceled":"Y","empCanceledId":1,"reasonCanceled":""},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.order.update

    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.order.update',
    		{
    			id: 300,
    			fields: {
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
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch(error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.order.update',
                [
                    'id' => 300,
                    'fields' => [
                        'price'           => 100,
                        'discountValue'   => 10,
                        'statusId'        => 'N',
                        'empStatusId'     => 1,
                        'dateInsert'      => '2024-03-01T14:00:00',
                        'marked'          => 'Y',
                        'empMarkedId'     => 1,
                        'reasonMarked'    => '',
                        'userDescription' => '',
                        'additionalInfo'  => '',
                        'comments'        => '',
                        'companyId'       => 1,
                        'responsibleId'   => 1,
                        'recurringId'     => 1,
                        'lockedBy'        => 1,
                        'recountFlag'     => 'N',
                        'affiliateId'     => 1,
                        'updated1c'       => 'N',
                        'orderTopic'      => '',
                        'xmlId'           => '',
                        'id1c'            => '',
                        'version1c'       => '',
                        'externalOrder'   => 'N',
                        'canceled'        => 'Y',
                        'empCanceledId'   => 1,
                        'reasonCanceled'  => '',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating sale order: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.order.update',
        {
            id: 300,
            fields: {
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
        'sale.order.update',
        [
            'id' => 300,
            'fields' => [
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
            "accountNumber": "456",
            "additionalInfo": "",
            "affiliateId": 1,
            "canceled": "Y",
            "clients": [],
            "comments": "",
            "companyId": 1,
            "currency": "USD",
            "dateCanceled": "2024-04-12T14:06:05+02:00",
            "dateInsert": "2024-03-01T13:00:00+02:00",
            "dateLock": null,
            "dateMarked": "2024-04-15T10:21:14+02:00",
            "dateStatus": "2024-04-12T14:06:04+02:00",
            "dateUpdate": "2024-04-15T10:21:15+02:00",
            "deducted": "N",
            "discountValue": 10,
            "empCanceledId": 1,
            "empMarkedId": 1,
            "empStatusId": 1,
            "externalOrder": "N",
            "id": 300,
            "id1c": "",
            "lid": "s1",
            "lockedBy": "1",
            "marked": "N",
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
            "taxValue": null,
            "updated1c": "N",
            "userDescription": "",
            "userId": 1,
            "version": 3,
            "version1c": "",
            "xmlId": ""
        }
    },
    "time": {
        "start": 1713169274.29568,
        "finish": 1713169275.698528,
        "duration": 1.4028480052947998,
        "processing": 0.9852678775787354,
        "date_start": "2024-04-15T11:21:14+02:00",
        "date_finish": "2024-04-15T11:21:15+02:00"
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
[`sale_order`](../data-types.md) | Object containing information about the updated order ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200540400001,
    "error_description":"order does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200540400001` | The order being updated was not found ||
|| `200040300020` | Insufficient permissions to update the order ||
|| `100` | The `id` parameter is missing ||
|| `100` | The `fields` parameter is missing or empty ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-order-add.md)
- [{#T}](./sale-order-get.md)
- [{#T}](./sale-order-list.md)
- [{#T}](./sale-order-delete.md)
- [{#T}](./sale-order-get-fields.md)