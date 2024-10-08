# Get a List of REST Handlers for the Payment System sale.paysystem.handler.list

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow to modify settings")

This method returns a list of REST handlers for the payment system.

No parameters.

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.handler.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.handler.list?auth=**put_access_token_here**
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.paysystem.handler.list",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paysystem.handler.list',
        []
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
    "result": [
        {
            "ID": "1",
            "NAME": "REST Handler",
            "CODE": "resthandlercodedoc",
            "SORT": "100",
            "SETTINGS": {
                "CURRENCY": [
                    "USD"
                ],
                "FORM_DATA": {
                    "ACTION_URI": "http://example.com/payment_form.php",
                    "METHOD": "POST",
                    "FIELDS": {
                        "phone": {
                            "VISIBLE": "Y",
                            "CODE": {
                                "NAME": "Phone Number",
                                "TYPE": "STRING"
                            }
                        },
                        "paymentId": {
                            "CODE": "PAYMENT_ID",
                            "VISIBLE": "Y"
                        },
                        "serviceid": {
                            "CODE": "REST_SERVICE_ID"
                        }
                    }
                },
                "CODES": {
                    "REST_SERVICE_ID": {
                        "NAME": "Store Number",
                        "DESCRIPTION": "Store Number",
                        "SORT": "100"
                    },
                    "REST_SERVICE_KEY": {
                        "NAME": "Secret Key",
                        "DESCRIPTION": "Secret Key",
                        "SORT": "300"
                    },
                    "PAYMENT_ID": {
                        "NAME": "Payment Number",
                        "SORT": "400",
                        "GROUP": "PAYMENT",
                        "DEFAULT": {
                            "PROVIDER_KEY": "PAYMENT",
                            "PROVIDER_VALUE": "ACCOUNT_NUMBER"
                        }
                    },
                    "PS_CHANGE_STATUS_PAY": {
                        "NAME": "Automatic Payment Status Change",
                        "SORT": "700",
                        "INPUT": {
                            "TYPE": "Y/N"
                        }
                    },
                    "PAYMENT_BUYER_ID": {
                        "NAME": "Buyer Code",
                        "SORT": "1000",
                        "GROUP": "PAYMENT",
                        "DEFAULT": {
                            "PROVIDER_KEY": "ORDER",
                            "PROVIDER_VALUE": "USER_ID"
                        }
                    },
                    "PS_WORK_MODE": {
                        "NAME": "Payment System Operating Mode",
                        "SORT": "1100",
                        "INPUT": {
                            "TYPE": "ENUM",
                            "OPTIONS": {
                                "TEST": "Test",
                                "REGULAR": "Live"
                            }
                        }
                    }
                }
            }
        }
    ],
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
[`sale_paysystem_handler[]`](../sale/data-types.md) | List of registered REST handlers for payment systems ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Access denied. The application is trying to modify a handler added by another application, or lacks sufficient rights to update the handler | 403 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-pay-system-handler-add.md)
- [{#T}](./sale-pay-system-handler-update.md)
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