# Get Payment by Id sale.payment.get

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves the values of all payment fields by `Id`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_payment.id`](../data-types.md) | Payment identifier ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.payment.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.payment.get
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.payment.get",
        {
            "id": 6
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.payment.get',
        [
            'id' => 6
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
        "payment": {
            "accountNumber": "161\/1",
            "comments": "",
            "companyId": null,
            "currency": "USD",
            "dateBill": "2022-10-14T16:46:27+03:00",
            "dateMarked": null,
            "datePaid": null,
            "datePayBefore": null,
            "dateResponsibleId": "2022-10-14T16:46:27+03:00",
            "empMarkedId": null,
            "empPaidId": null,
            "empResponsibleId": 1,
            "empReturnId": null,
            "externalPayment": "N",
            "id": 6,
            "id1c": "",
            "isReturn": "N",
            "marked": "N",
            "orderId": 5,
            "paid": "N",
            "payReturnComment": "",
            "payReturnDate": null,
            "payReturnNum": "",
            "paySystemId": 6,
            "paySystemIsCash": "Y",
            "paySystemName": "Cash",
            "paySystemXmlId": "bx_64134ba550ffa",
            "payVoucherDate": null,
            "payVoucherNum": "",
            "priceCod": "0",
            "psCurrency": "",
            "psInvoiceId": null,
            "psResponseDate": null,
            "psStatus": "",
            "psStatusCode": "",
            "psStatusDescription": "",
            "psStatusMessage": "",
            "psSum": null,
            "reasonMarked": "",
            "responsibleId": 1,
            "sum": 2352,
            "updated1c": "N",
            "version1c": "",
            "xmlId": "bx_6349845343355"
        }
    },
    "time": {
        "start": 1713446368.239796,
        "finish": 1713446369.113212,
        "duration": 0.8734161853790283,
        "processing": 0.4978961944580078,
        "date_start": "2024-04-18T16:19:28+03:00",
        "date_finish": "2024-04-18T16:19:29+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **payment**
[`sale_order_payment`](../data-types.md) | Payment information ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200640400001,
    "error_description":"payment does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200640400001` | Payment not found ||
|| `200040300010` | Insufficient permissions to read payment ||
|| `100` | Parameter `id` not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-payment-add.md)
- [{#T}](./sale-payment-update.md)
- [{#T}](./sale-payment-list.md)
- [{#T}](./sale-payment-delete.md)
- [{#T}](./sale-payment-get-fields.md)