# Update Payment Fields crm.item.payment.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires access permission to modify the payment order

This method updates a limited set of payment fields (see [`sale.payment.update`](../../../sale/payment/sale-payment-update.md) for extended functionality with payment fields).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_payment.id`](../../../sale/data-types.md#sale_order_payment) | Payment identifier ||
|| **fields***
[`object`](../../../../api-reference/data-types.md) | Field values for updating the payment  ||
|#

### Fields Parameter

#|
|| **Name**
`type` | **Description** ||
|| **paid**
[`string`](../../../../api-reference/data-types.md) | Payment status.

Possible values:
- `Y` – Paid
- `N` – Not paid ||
|| **paySystemId**
[`sale_paysystem.id`](../../../sale/data-types.md#sale_paysystem) | Payment system identifier
 ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1036,"fields":{"paid":"Y","paySystemId":110}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1036,"fields":{"paid":"Y","paySystemId":110},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.item.payment.update', {
    			id: 1036,
    			fields: {
    				paid: "Y",
    				paySystemId: 110,
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
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
                'crm.item.payment.update',
                [
                    'id' => 1036,
                    'fields' => [
                        'paid' => "Y",
                        'paySystemId' => 110,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating payment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.payment.update', {
            id: 1036,
            fields: {
                paid: "Y",
                paySystemId: 110,
            }
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
        'crm.item.payment.update',
        [
            'id' => 1036,
            'fields' => [
                'paid' => 'Y',
                'paySystemId' => 110,
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
   "result":true,
   "time":{
      "start":1716212943.651507,
      "finish":1716212944.578154,
      "duration":0.9266471862792969,
      "processing":0.6742370128631592,
      "date_start":"2024-05-20T16:49:03+02:00",
      "date_finish":"2024-05-20T16:49:04+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../../api-reference/data-types.md) | Result of the operation  ||
|| **time**
[`time`](../../../../api-reference/data-types.md) | Information about the request execution time ||
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
|| `0` | Payment not found or access denied ||
|| `0` | Payment system not found  ||
|| `100` | Parameter id not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-delete.md)
- [{#T}](./crm-item-payment-get.md)
- [{#T}](./crm-item-payment-list.md)
- [{#T}](./crm-item-payment-pay.md)
- [{#T}](./crm-item-payment-unpay.md)
- [{#T}](./crm-item-payment-add.md)