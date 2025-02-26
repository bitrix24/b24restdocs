# Update Payment sale.payment.update

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.payment.update` is used to update fields of the payment collection item.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_payment.id`](../data-types.md) | Payment identifier ||
|| **fields***
[`object`](../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for creating a payment in the form of a structure:

```js
fields: {
    paySystemId: "value",
    paid: "value",
    datePaid: "value",
    empPaidId: "value",
    psStatus: "value",
    psStatusCode: "value",
    psStatusDescription: "value",
    psStatusMessage: "value",
    psSum: "value",
    psCurrency: "value",
    psResponseDate: "value",
    payVoucherNum: "value",
    payVoucherDate: "value",
    datePayBefore: "value",
    dateBill: "value",
    xmlId: "value",
    sum: "value",
    companyId: "value",
    payReturnNum: "value",
    priceCod: "value",
    payReturnDate: "value",
    empReturnId: "value",
    payReturnComment: "value",
    responsibleId: "value",
    empResponsibleId: "value",
    isReturn: "value",
    comments: "value",
    updated1c: "value",
    id1c: "value",
    version1c: "value",
    externalPayment: "value",
    psInvoiceId: "value",
    marked: "value",
    reasonMarked: "value",
    empMarkedId: "value",
}
```
 ||
|#

## Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **paySystemId***
[`sale_paysystem.id`](../data-types.md) | Payment system identifier ||
|| **paid**
[`string`](../../data-types.md) | Payment status:

- `Y` — yes
- `N` — no

Defaults to `N` ||
|| **datePaid**
[`datetime`](../../data-types.md) | Payment date ||
|| **empPaidId**
[`user.id`](../../data-types.md) | Identifier of the user who made the payment ||
|| **psStatus**
[`string`](../../data-types.md) | Payment system status flag — whether the payment was successfully made. Options:

- `Y` — yes
- `N` — no

Defaults to `null` ||
|| **psStatusCode**
[`string`](../../data-types.md) | Payment system status code ||
|| **psStatusDescription**
[`string`](../../data-types.md) | Description of the payment system's operation result ||
|| **psStatusMessage**
[`string`](../../data-types.md) | Message from the payment system ||
|| **psSum**
[`double`](../../data-types.md) | Payment system amount ||
|| **psCurrency**
[`string`](../../data-types.md) | Payment system currency ||
|| **psResponseDate**
[`datetime`](../../data-types.md) | Payment system response date ||
|| **payVoucherNum**
[`string`](../../data-types.md) | Payment document number ||
|| **payVoucherDate**
[`datetime`](../../data-types.md) | Payment document date ||
|| **datePayBefore**
[`datetime`](../../data-types.md) | Deprecated.

Date by which the invoice must be paid ||
|| **dateBill**
[`datetime`](../../data-types.md) | Invoice date ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier ||
|| **sum**
[`double`](../../data-types.md) | Payment amount ||
|| **companyId**
[`integer`](../../data-types.md) | Identifier of the company that will receive the payment

Currently not used ||
|| **payReturnNum**
[`string`](../../data-types.md) | Return document number ||
|| **priceCod**
[`double`](../../data-types.md) | Relevant only for on-premise version
Cost of payment upon delivery. Used for cash on delivery ||
|| **payReturnDate**
[`date`](../../data-types.md) | Return document date ||
|| **empReturnId**
[`user.id`](../../data-types.md) | Identifier of the user who processed the return ||
|| **payReturnComment**
[`string`](../../data-types.md) | Return comment ||
|| **responsibleId**
[`user.id`](../../data-types.md) | Identifier of the user responsible for the payment ||
|| **empResponsibleId**
[`user.id`](../../data-types.md) | Identifier of the user who assigned the responsible person ||
|| **isReturn**
[`string`](../../data-types.md) | Was the return processed:

- `Y` — yes
- `N` — no

Defaults to N ||
|| **comments**
[`string`](../../data-types.md) | Payment comments ||
|| **updated1c**
[`string`](../../data-types.md) | Payment updated via 1C:

- `Y` — yes
- `N` — no

Defaults to N ||
|| **id1c**
[`string`](../../data-types.md) | Identifier in 1C ||
|| **version1c**
[`string`](../../data-types.md) | Payment document version from 1C ||
|| **externalPayment**
[`string`](../../data-types.md) | Relevant only for on-premise version
Whether it is an external payment or not. Used for import from 1C via XML

- `Y` — yes
- `F` — yes, loaded with the order
- `N` — no

Defaults to `N` ||
|| **psInvoiceId**
[`string`](../../data-types.md) | Payment identifier in the payment system ||
|| **marked**
[`string`](../../data-types.md) | Marking flag. Indicates whether the payment is marked as problematic:

- `Y` — yes
- `N` — no

Defaults to `N` ||
|| **reasonMarked**
[`string`](../../data-types.md) | Reason for marking ||
|| **empMarkedId**
[`user.id`](../../data-types.md) | Identifier of the user who marked the payment ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":144,"fields":{"paySystemId":1,"paid":"Y","datePaid":"2024-04-10T10:00:00","empPaidId":1,"psStatus":"Y","psSum":100,"psCurrency":"USD","psResponseDate":"2024-04-10T10:00:00","sum":100,"companyId":1,"responsibleId":1,"empResponsibleId":1,"isReturn":"N","externalPayment":"N","psInvoiceId":1,"marked":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.payment.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":144,"fields":{"paySystemId":1,"paid":"Y","datePaid":"2024-04-10T10:00:00","empPaidId":1,"psStatus":"Y","psSum":100,"psCurrency":"USD","psResponseDate":"2024-04-10T10:00:00","sum":100,"companyId":1,"responsibleId":1,"empResponsibleId":1,"isReturn":"N","externalPayment":"N","psInvoiceId":1,"marked":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.payment.update
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.payment.update',
        {
            id: 144,
            fields: {
                paySystemId: 1,
                paid: 'Y',
                datePaid: '2024-04-10T10:00:00',
                empPaidId: 1,
                psStatus: 'Y',
                psStatusCode: '',
                psStatusDescription: '',
                psStatusMessage: '',
                psSum: 100,
                psCurrency: 'USD',
                psResponseDate: '2024-04-10T10:00:00',
                payVoucherNum: '',
                payVoucherDate: '2024-04-10T10:00:00',
                datePayBefore: '2024-04-10T10:00:00',
                dateBill: '2024-04-10T10:00:00',
                xmlId: '',
                sum: 100,
                companyId: 1,
                payReturnNum: '',
                priceCod: 100,
                payReturnDate: '2024-04-10T10:00:00',
                empReturnId: 1,
                payReturnComment: '',
                responsibleId: 1,
                empResponsibleId: 1,
                isReturn: 'N',
                comments: '',
                updated1c: 'N',
                id1c: '',
                version1c: '',
                externalPayment: 'N',
                psInvoiceId: 1,
                marked: 'N',
                reasonMarked: '',
                empMarkedId: 1,
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.payment.update',
        [
            'id' => 144,
            'fields' => [
                'paySystemId' => 1,
                'paid' => 'Y',
                'datePaid' => '2024-04-10T10:00:00',
                'empPaidId' => 1,
                'psStatus' => 'Y',
                'psStatusCode' => '',
                'psStatusDescription' => '',
                'psStatusMessage' => '',
                'psSum' => 100,
                'psCurrency' => 'USD',
                'psResponseDate' => '2024-04-10T10:00:00',
                'payVoucherNum' => '',
                'payVoucherDate' => '2024-04-10T10:00:00',
                'datePayBefore' => '2024-04-10T10:00:00',
                'dateBill' => '2024-04-10T10:00:00',
                'xmlId' => '',
                'sum' => 100,
                'companyId' => 1,
                'payReturnNum' => '',
                'priceCod' => 100,
                'payReturnDate' => '2024-04-10T10:00:00',
                'empReturnId' => 1,
                'payReturnComment' => '',
                'responsibleId' => 1,
                'empResponsibleId' => 1,
                'isReturn' => 'N',
                'comments' => '',
                'updated1c' => 'N',
                'id1c' => '',
                'version1c' => '',
                'externalPayment' => 'N',
                'psInvoiceId' => 1,
                'marked' => 'N',
                'reasonMarked' => '',
                'empMarkedId' => 1,
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
        "payment": {
            "accountNumber": "356\/1",
            "comments": "",
            "companyId": 1,
            "currency": "USD",
            "dateBill": "2024-04-10T09:00:00+02:00",
            "dateMarked": "2024-04-16T16:32:49+02:00",
            "datePaid": "2024-04-10T09:00:00+02:00",
            "datePayBefore": "2024-04-10T09:00:00+02:00",
            "dateResponsibleId": "2024-04-16T16:32:49+02:00",
            "empMarkedId": 1,
            "empPaidId": 1,
            "empResponsibleId": 1,
            "empReturnId": 1,
            "externalPayment": "N",
            "id": 144,
            "id1c": "",
            "isReturn": "N",
            "marked": "N",
            "orderId": 200,
            "paid": "Y",
            "payReturnComment": "",
            "payReturnDate": "2024-04-10T09:00:00+02:00",
            "payReturnNum": "",
            "paySystemId": 1,
            "paySystemIsCash": "N",
            "paySystemName": "Bank Transfer (Company)",
            "paySystemXmlId": "",
            "payVoucherDate": "2024-04-10T09:00:00+02:00",
            "payVoucherNum": "",
            "priceCod": "100",
            "psCurrency": "USD",
            "psInvoiceId": 1,
            "psResponseDate": "2024-04-10T09:00:00+02:00",
            "psStatus": "Y",
            "psStatusCode": "",
            "psStatusDescription": "",
            "psStatusMessage": "",
            "psSum": 100,
            "reasonMarked": "",
            "responsibleId": 1,
            "sum": 100,
            "updated1c": "N",
            "version1c": "",
            "xmlId": ""
        }
    },
    "time": {
        "start": 1713454774.293692,
        "finish": 1713454778.895877,
        "duration": 4.602184772491455,
        "processing": 4.142766952514648,
        "date_start": "2024-04-18T18:39:34+02:00",
        "date_finish": "2024-04-18T18:39:38+02:00"
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
[`sale_order_payment`](../data-types.md) | Object with information about the updated payment ||
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
|| `200640400001` | The payment being updated was not found ||
|| `200040300020` | Insufficient permissions to update the payment ||
|| `100` | Parameter `id` not specified ||
|| `100` | Parameter `fields` not specified or empty ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-payment-add.md)
- [{#T}](./sale-payment-get.md)
- [{#T}](./sale-payment-list.md)
- [{#T}](./sale-payment-delete.md)
- [{#T}](./sale-payment-get-fields.md)