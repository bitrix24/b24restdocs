# Get Payment Information crm.item.payment.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: access permission to modify the payment order is required

This method retrieves brief information about the payment.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_payment.id`](../../../sale/data-types.md#sale_order_payment) | Payment identifier ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1036}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1036,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.item.payment.get', {
    			id: 1036,
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
                'crm.item.payment.get',
                [
                    'id' => 1036,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting payment item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.payment.get', {
            id: 1036,
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
        'crm.item.payment.get',
        [
            'id' => 1036
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
   "result":{
      "id":1036,
      "accountNumber":"3653\/1",
      "paid": "Y",
      "datePaid":"2024-05-20T12:32:02+02:00",
      "empPaidId":1,
      "paySystemId":6,
      "sum":0,
      "currency":"USD",
      "paySystemName":"Cash"
   },
   "time":{
      "start":1716203536.414886,
      "finish":1716203536.798211,
      "duration":0.38332509994506836,
      "processing":0.052394866943359375,
      "date_start":"2024-05-20T14:12:16+02:00",
      "date_finish":"2024-05-20T14:12:16+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_order_payment_crm_simple`](#sale_order_payment_crm_simple) | Object containing brief information about the payment  ||
|| **time**
[`time`](../../../../api-reference/data-types.md) | Information about the request execution time ||
|#

### Key result. Object of type sale_order_payment_crm_simple {#sale_order_payment_crm_simple}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`sale_order_payment.id`](../../../sale/data-types.md#sale_order_payment) | Payment identifier ||
|| **accountNumber**
[`string`](../../../../api-reference/data-types.md) | System payment number ||
|| **paid**
[`string`](../../../../api-reference/data-types.md) | Payment status

Possible values:
- `Y` – Paid
- `N` – Not paid ||
|| **datePaid**
[`datetime`](../../../../api-reference/data-types.md) | Payment date ||
|| **empPaidId**
[`user.id`](../../../../api-reference/data-types.md)| User who made the payment ||
|| **paySystemId**
[`sale_paysystem.id`](../../../sale/data-types.md#sale_paysystem) | Payment system identifier ||
|| **sum**
[`double`](../../../../api-reference/data-types.md) | Payment amount ||
|| **currency**
[`string`](../../../../api-reference/data-types.md) | Payment currency ||
|| **paySystemName**
[`string`](../../../../api-reference/data-types.md) | Name of the payment system ||
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
|| `100` | Parameter id not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-update.md)
- [{#T}](./crm-item-payment-delete.md)
- [{#T}](./crm-item-payment-list.md)
- [{#T}](./crm-item-payment-pay.md)
- [{#T}](./crm-item-payment-unpay.md)
- [{#T}](./crm-item-payment-add.md)