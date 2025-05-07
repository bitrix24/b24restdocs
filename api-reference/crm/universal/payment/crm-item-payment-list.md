# Get a list of payments crm.item.payment.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires read access permission for the CRM object from which payments are selected.

This method retrieves a list of payments for a specific CRM entity.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityId***
[`integer`](../../../../api-reference/data-types.md) | Identifier of the CRM entity ||
|| **entityTypeId***
[`integer`](../../../../api-reference/data-types.md) | Identifier of the [`CRM entity type`](../../data-types.md#object_type) ||
|| **filter**
[`object`](../../../../api-reference/data-types.md) | Additional filter for cases when you need to retrieve not all payments of the entity, but based on a more specific filter. The format description is provided in the **filter** parameter of the [`sale.payment.list`](../../../sale/payment/sale-payment-list.md) method ||
|| **order**
[`object`](../../../../api-reference/data-types.md) | The format description is provided in the **order** parameter of the [`sale.payment.list`](../../../sale/payment/sale-payment-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityId":13123,"entityTypeId":2,"filter":{"@id":[1036,1037]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.list
    ```

- cURL (OAuth) 

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityId":13123,"entityTypeId":2,"filter":{"@id":[1036,1037]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.list
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.item.payment.list', {
            entityId: 13123,
            entityTypeId: 2,
            filter: {
                "@id": [1036, 1037]
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.payment.list',
        [
            'entityId' => 13123,
            'entityTypeId' => 2,
            'filter' => [
                '@id' => [1036, 1037]
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
      {
         "id":1037,
         "accountNumber":"3653\/2",
         "paid": "N",
         "datePaid":null,
         "empPaidId":null,
         "paySystemId":6,
         "sum":0,
         "currency":"USD",
         "paySystemName":"Cash"
      }
   ],
   "time":{
      "start":1716205783.285524,
      "finish":1716205783.702053,
      "duration":0.41652917861938477,
      "processing":0.15817594528198242,
      "date_start":"2024-05-20T14:49:43+02:00",
      "date_finish":"2024-05-20T14:49:43+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_order_payment_crm_simple[]`](crm-item-payment-get.md#sale_order_payment_crm_simple) | Array of objects containing brief information about the selected payments ||
|| **time**
[`time`](../../../../api-reference/data-types.md) | Information about the execution time of the request ||
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

- [{#T}](./crm-item-payment-update.md)
- [{#T}](./crm-item-payment-delete.md)
- [{#T}](./crm-item-payment-get.md)
- [{#T}](./crm-item-payment-pay.md)
- [{#T}](./crm-item-payment-unpay.md)
- [{#T}](./crm-item-payment-add.md)
- [{#T}](../../../../tutorials/crm/how-to-edit-crm-objects/how-to-add-paid-date-to-deal.md)