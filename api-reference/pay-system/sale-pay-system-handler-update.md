# Update REST Handler for Payment System sale.paysystem.handler.update

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow to modify settings")

This method updates the REST handler for the payment system.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_paysystem_handler.ID`](../sale/data-types.md) | Identifier of the REST handler ||
|| **FIELDS***
[`object`](../data-types.md) | Set of values for updating (detailed description provided [below](#parametr-fields)) ||
|#

### FIELDS Parameter

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](../data-types.md) | Name of the handler ||
|| **CODE**
[`string`](../data-types.md) | Unique code of the handler in the system ||
|| **SETTINGS**
[`object`](../data-types.md) | Settings of the handler. The format is similar to the format in [sale.paysystem.handler.add](./sale-pay-system-handler-add.md) ||
|| **SORT**
[`integer`](../data-types.md) | Sorting ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":3,"FIELDS":{"CODE":"newresthandlercode","NAME":"New Handler Name","SORT":200,"SETTINGS":{"CURRENCY":["USD","BYN"],"FORM_DATA":{"ACTION_URI":"http://example.com/payment_form.php","METHOD":"POST","PARAMS":{"serviceid":"REST_SERVICE_ID_2","invoiceNumber":"PAYMENT_ID_2","Sum":"PAYMENT_SHOULD_PAY_2","customer":"PAYMENT_BUYER_ID_2"},"CODES":{"REST_SERVICE_ID_2":{"NAME":"Store Number","DESCRIPTION":"Store Number","SORT":"100"},"REST_SERVICE_KEY_2":{"NAME":"Secret Key","DESCRIPTION":"Secret Key","SORT":"300"},"PAYMENT_ID_2":{"NAME":"Payment Number","SORT":"400","GROUP":"PAYMENT","DEFAULT":{"PROVIDER_KEY":"PAYMENT","PROVIDER_VALUE":"ACCOUNT_NUMBER"}},"PAYMENT_SHOULD_PAY_2":{"NAME":"Payment Amount","SORT":"600","GROUP":"PAYMENT","DEFAULT":{"PROVIDER_KEY":"PAYMENT","PROVIDER_VALUE":"SUM"}},"PS_CHANGE_STATUS_PAY_2":{"NAME":"Automatic Payment Status Change","SORT":"700","INPUT":{"TYPE":"Y/N"}},"PAYMENT_BUYER_ID_2":{"NAME":"Buyer Code","SORT":"1000","GROUP":"PAYMENT","DEFAULT":{"PROVIDER_KEY":"ORDER","PROVIDER_VALUE":"USER_ID"}}}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.handler.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":3,"FIELDS":{"CODE":"newresthandlercode","NAME":"New Handler Name","SORT":200,"SETTINGS":{"CURRENCY":["USD","BYN"],"FORM_DATA":{"ACTION_URI":"http://example.com/payment_form.php","METHOD":"POST","PARAMS":{"serviceid":"REST_SERVICE_ID_2","invoiceNumber":"PAYMENT_ID_2","Sum":"PAYMENT_SHOULD_PAY_2","customer":"PAYMENT_BUYER_ID_2"},"CODES":{"REST_SERVICE_ID_2":{"NAME":"Store Number","DESCRIPTION":"Store Number","SORT":"100"},"REST_SERVICE_KEY_2":{"NAME":"Secret Key","DESCRIPTION":"Secret Key","SORT":"300"},"PAYMENT_ID_2":{"NAME":"Payment Number","SORT":"400","GROUP":"PAYMENT","DEFAULT":{"PROVIDER_KEY":"PAYMENT","PROVIDER_VALUE":"ACCOUNT_NUMBER"}},"PAYMENT_SHOULD_PAY_2":{"NAME":"Payment Amount","SORT":"600","GROUP":"PAYMENT","DEFAULT":{"PROVIDER_KEY":"PAYMENT","PROVIDER_VALUE":"SUM"}},"PS_CHANGE_STATUS_PAY_2":{"NAME":"Automatic Payment Status Change","SORT":"700","INPUT":{"TYPE":"Y/N"}},"PAYMENT_BUYER_ID_2":{"NAME":"Buyer Code","SORT":"1000","GROUP":"PAYMENT","DEFAULT":{"PROVIDER_KEY":"ORDER","PROVIDER_VALUE":"USER_ID"}}}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.handler.update
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.paysystem.handler.update",
        {
            'ID': 3,
            'FIELDS': {
                'CODE': 'newresthandlercode',
                'NAME': 'New Handler Name',
                'SORT': 200,
                'SETTINGS': {
                    "CURRENCY": [
                        "USD", "BYN"
                    ],
                    "FORM_DATA": {
                        "ACTION_URI": "http://example.com/payment_form.php",
                        "METHOD": "POST",
                        "PARAMS": {
                            "serviceid": "REST_SERVICE_ID_2",
                            "invoiceNumber": "PAYMENT_ID_2",
                            "Sum": "PAYMENT_SHOULD_PAY_2",
                            "customer": "PAYMENT_BUYER_ID_2"
                        }
                    },
                    "CODES": {
                        "REST_SERVICE_ID_2": {
                            "NAME": "Store Number",
                            "DESCRIPTION": "Store Number",
                            "SORT": "100"
                        },
                        "REST_SERVICE_KEY_2": {
                            "NAME": "Secret Key",
                            "DESCRIPTION": "Secret Key",
                            "SORT": "300"
                        },
                        "PAYMENT_ID_2": {
                            "NAME": "Payment Number",
                            "SORT": "400",
                            "GROUP": "PAYMENT",
                            "DEFAULT": {
                                "PROVIDER_KEY": "PAYMENT",
                                "PROVIDER_VALUE": "ACCOUNT_NUMBER"
                            }
                        },
                        "PAYMENT_SHOULD_PAY_2": {
                            "NAME": "Payment Amount",
                            "SORT": "600",
                            "GROUP": "PAYMENT",
                            "DEFAULT": {
                                "PROVIDER_KEY": "PAYMENT",
                                "PROVIDER_VALUE": "SUM"
                            }
                        },
                        "PS_CHANGE_STATUS_PAY_2": {
                            "NAME": "Automatic Payment Status Change",
                            "SORT": "700",
                            "INPUT": {
                                "TYPE": "Y/N"
                            }
                        },
                        "PAYMENT_BUYER_ID_2": {
                            "NAME": "Buyer Code",
                            "SORT": "1000",
                            "GROUP": "PAYMENT",
                            "DEFAULT": {
                                "PROVIDER_KEY": "ORDER",
                                "PROVIDER_VALUE": "USER_ID"
                            }
                        }
                    }
                }
            }
        }
        ,
        function (result) {
            if (result.error()) {
                console.error(result.error());
            }
            else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paysystem.handler.update',
        [
            'ID' => 3,
            'FIELDS' => [
                'CODE' => 'newresthandlercode',
                'NAME' => 'New Handler Name',
                'SORT' => 200,
                'SETTINGS' => [
                    'CURRENCY' => ['USD', 'BYN'],
                    'FORM_DATA' => [
                        'ACTION_URI' => 'http://example.com/payment_form.php',
                        'METHOD' => 'POST',
                        'PARAMS' => [
                            'serviceid' => 'REST_SERVICE_ID_2',
                            'invoiceNumber' => 'PAYMENT_ID_2',
                            'Sum' => 'PAYMENT_SHOULD_PAY_2',
                            'customer' => 'PAYMENT_BUYER_ID_2'
                        ]
                    ],
                    'CODES' => [
                        'REST_SERVICE_ID_2' => [
                            'NAME' => 'Store Number',
                            'DESCRIPTION' => 'Store Number',
                            'SORT' => '100'
                        ],
                        'REST_SERVICE_KEY_2' => [
                            'NAME' => 'Secret Key',
                            'DESCRIPTION' => 'Secret Key',
                            'SORT' => '300'
                        ],
                        'PAYMENT_ID_2' => [
                            'NAME' => 'Payment Number',
                            'SORT' => '400',
                            'GROUP' => 'PAYMENT',
                            'DEFAULT' => [
                                'PROVIDER_KEY' => 'PAYMENT',
                                'PROVIDER_VALUE' => 'ACCOUNT_NUMBER'
                            ]
                        ],
                        'PAYMENT_SHOULD_PAY_2' => [
                            'NAME' => 'Payment Amount',
                            'SORT' => '600',
                            'GROUP' => 'PAYMENT',
                            'DEFAULT' => [
                                'PROVIDER_KEY' => 'PAYMENT',
                                'PROVIDER_VALUE' => 'SUM'
                            ]
                        ],
                        'PS_CHANGE_STATUS_PAY_2' => [
                            'NAME' => 'Automatic Payment Status Change',
                            'SORT' => '700',
                            'INPUT' => [
                                'TYPE' => 'Y/N'
                            ]
                        ],
                        'PAYMENT_BUYER_ID_2' => [
                            'NAME' => 'Buyer Code',
                            'SORT' => '1000',
                            'GROUP' => 'PAYMENT',
                            'DEFAULT' => [
                                'PROVIDER_KEY' => 'ORDER',
                                'PROVIDER_VALUE' => 'USER_ID'
                            ]
                        ]
                    ]
                ]
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
    "result": true,
    "time": {
        "start": 1712135335.026931,
        "finish": 1712135335.407762,
        "duration": 0.3808310031890869,
        "processing": 0.0336611270904541,
        "date_start": "2024-04-03T11:08:55+02:00",
        "date_finish": "2024-04-03T11:08:55+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Result of updating the REST handler ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_HANDLER_NOT_FOUND",
    "error_description": "Handler not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Access denied. The application is trying to modify a handler added by another application, or lacks permissions to update the handler | 403 ||
|| `ERROR_CHECK_FAILURE` | The `ID` or `FIELDS` field value is not specified | 400 ||
|| `ERROR_HANDLER_NOT_FOUND` | Handler with the specified `ID` not found | 400 ||
|| `ERROR_HANDLER_UPDATE` | Other errors. For detailed information about the error, see `error_description` | 400 ||
|| `ERROR_UNEXPECTED_ANSWER` | Unexpected server response. One possible reason is attempting to specify a non-unique `CODE` parameter for the handler that already exists for another handler | 400 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-pay-system-handler-add.md)
- [{#T}](./sale-pay-system-handler-list.md)
- [{#T}](./sale-pay-system-handler-delete.md)
- [{#T}](./sale-pay-system-add.md)
- [{#T}](./sale-pay-system-update.md)
- [{#T}](./sale-pay-system-list.md)
- [{#T}](./sale-pay-system-settings-get.md)
- [{#T}](./sale-pay-system-settings-update.md)
- [{#T}](./sale-pay-system-delete.md)
- [{#T}](./sale-pay-system-pay-payment.md)
- [{#T}](./sale-pay-system-pay-invoice.md)
- [{#T}](./sale-pay-system-settings-payment-get.md)
- [{#T}](./sale-pay-system-settings-invoice-get.md)