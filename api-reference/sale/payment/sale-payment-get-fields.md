# Get Payment Fields sale.payment.getFields

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves the available payment fields.

No parameters.

## Code Examples

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.payment.getfields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.payment.getfields
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.payment.getfields",
        {},
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
        'sale.payment.getfields',
        []
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
        "payment": {
            "accountNumber": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "string"
            },
            "comments": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "companyId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "currency": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "string"
            },
            "dateBill": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "datetime"
            },
            "dateMarked": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "datePaid": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "datetime"
            },
            "datePayBefore": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "datetime"
            },
            "dateResponsibleId": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "empMarkedId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "empPaidId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "empResponsibleId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "empReturnId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "externalPayment": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "id": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "integer"
            },
            "id1c": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "isReturn": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "marked": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "orderId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "paid": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "payReturnComment": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "payReturnDate": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "datetime"
            },
            "payReturnNum": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "paySystemId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "paySystemIsCash": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "char"
            },
            "paySystemName": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "string"
            },
            "paySystemXmlId": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "string"
            },
            "payVoucherDate": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "datetime"
            },
            "payVoucherNum": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "priceCod": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "psCurrency": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "psInvoiceId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "psResponseDate": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "datetime"
            },
            "psStatus": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "psStatusCode": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "psStatusDescription": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "psStatusMessage": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "psSum": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "double"
            },
            "reasonMarked": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "responsibleId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "sum": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "double"
            },
            "updated1c": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "version1c": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "xmlId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            }
        }
    },
    "time": {
        "start": 1713446682.387006,
        "finish": 1713446682.788821,
        "duration": 0.40181493759155273,
        "processing": 0.01438593864440918,
        "date_start": "2024-04-18T16:24:42+03:00",
        "date_finish": "2024-04-18T16:24:42+03:00"
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
[`object`](../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`. Where `field` is the identifier of the [sale_order_payment](../data-types.md) object, and `value` is an object of type [rest_field_description](../data-types.md#rest_field_description) ||
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
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-payment-add.md)
- [{#T}](./sale-payment-update.md)
- [{#T}](./sale-payment-get.md)
- [{#T}](./sale-payment-list.md)
- [{#T}](./sale-payment-delete.md)