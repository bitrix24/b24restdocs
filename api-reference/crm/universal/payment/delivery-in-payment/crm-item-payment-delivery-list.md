# Get Delivery Item List by Specific Payment crm.item.payment.delivery.list

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: read access permission for the payment order is required

This method retrieves a list of delivery items for a specific payment.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **paymentId***
[`sale_order_payment.id`](../../../../sale/data-types.md#sale_order_payment) | Payment identifier.
Can be obtained using the method [`sale.payment.list`](../../../../sale/payment/sale-payment-list.md) ||
|| **filter***
[`object`](../../../../data-types.md) | Object for filtering selected delivery items in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field`:
- `id`
- `quantity`

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:

- `=` — equals, exact match (default)
- `%` — LIKE, substring search. The % symbol should not be included in the filter value. The search looks for the substring in any position of the string
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol should not be included in the filter value. The search goes from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol should be included in the value. Examples: 
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol should be included in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)
||
|| **order**
[`object`](../../../../data-types.md) | Object for sorting selected delivery items of the payment in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field`:
- `id`
- `quantity`

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"paymentId":1040,"filter":{"@id":[1201], ">=quantity":1}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.delivery.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"paymentId":1040,"filter":{"@id":[1201], ">=quantity":1},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.delivery.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'crm.item.payment.delivery.list',
        {
          paymentId: 1040,
          filter: {
            ">=quantity": 1,
            "@id": [1201],
          },
        },
        (progress) => { console.log('Progress:', progress) }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('crm.item.payment.delivery.list', {
        paymentId: 1040,
        filter: {
          ">=quantity": 1,
          "@id": [1201],
        },
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.item.payment.delivery.list', {
        paymentId: 1040,
        filter: {
          ">=quantity": 1,
          "@id": [1201],
        },
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.payment.delivery.list',
                [
                    'paymentId' => 1040,
                    'filter'    => [
                        ">=quantity" => 1,
                        "@id"        => [1201],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching payment delivery list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.payment.delivery.list', {
            paymentId: 1040,
            filter: {
                ">=quantity": 1,
                "@id": [1201],
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
        'crm.item.payment.delivery.list',
        [
            'paymentId' => 1040,
            'filter' => [
                '>=quantity' => 1,
                '@id' => [1201],
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
         "id":1201,
         "paymentId":1040,
         "quantity":1,
         "deliveryId":4073
      }
   ],
   "time":{
      "start":1716301848.792584,
      "finish":1716301849.095721,
      "duration":0.30313706398010254,
      "processing":0.05563783645629883,
      "date_start":"2024-05-21T17:30:48+02:00",
      "date_finish":"2024-05-21T17:30:49+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`crm_item_payment_delivery[]` | Array of objects containing information about the selected delivery items for the payment ||
|| **time**
[`time`](../../../../data-types.md) | Information about the execution time of the request ||
|#

### Key result. Object of type crm_item_payment_delivery 

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the delivery item in the payment ||
|| **paymentId**
[`sale_order_payment.id`](../../../../data-types.md#sale_order_payment) | Payment identifier  ||
|| **quantity**
[`double`](../../../../data-types.md) | Quantity ||
|| **deliveryId**
[`sale_order_shipment.id`](../../../../data-types.md#sale_order_shipment)  | Delivery identifier ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":0,
   "error_description":"Payment has not been found"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | Payment not found ||
|| `0` | Access denied ||
|| `100` | Required parameters not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-delivery-add.md)
- [{#T}](./crm-item-payment-delivery-delete.md)
- [{#T}](./crm-item-payment-delivery-set-delivery.md)

