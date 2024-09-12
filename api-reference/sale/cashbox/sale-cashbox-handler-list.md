# Get a list of available cash register handlers sale.cashbox.handler.list

> Scope: [`sale, cashbox`](../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow to modify settings")

The method returns a list of available REST handlers for cash registers.

No parameters.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.cashbox.handler.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.cashbox.handler.list
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.cashbox.handler.list",
        {
        },
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
        'sale.cashbox.handler.list',
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
    "result": [
        {
            "ID": "1",
            "NAME": "My REST Cash Register",
            "CODE": "my_rest_cashbox",
            "SORT": "200",
            "SETTINGS": {
                "PRINT_URL": "http://example.com/receipt_print.php",
                "CHECK_URL": "http://example.com/receipt_check.php",
                "CONFIG": {
                    "AUTH": {
                        "LABEL": "Authorization",
                        "ITEMS": {
                            "LOGIN": {
                                "TYPE": "STRING",
                                "REQUIRED": "Y",
                                "LABEL": "Login"
                            },
                            "PASSWORD": {
                                "TYPE": "STRING",
                                "REQUIRED": "Y",
                                "LABEL": "Password"
                            }
                        }
                    },
                    "COMPANY": {
                        "LABEL": "Organization Information",
                        "ITEMS": {
                            "INN": {
                                "TYPE": "STRING",
                                "REQUIRED": "Y",
                                "LABEL": "Organization INN",
                                "DISABLED": "N",
                                "MULTIPLE": "N",
                                "MULTILINE": "Y"
                            }
                        }
                    },
                    "INTERACTION": {
                        "LABEL": "Cash Register Interaction Settings",
                        "ITEMS": {
                            "MODE": {
                                "TYPE": "ENUM",
                                "LABEL": "Cash Register Operating Mode",
                                "OPTIONS": {
                                    "ACTIVE": "live",
                                    "TEST": "test"
                                }
                            }
                        }
                    }
                },
                "SUPPORTS_FFD105": "N"
            }
        }
    ],
    "time": {
        "start": 1712135957.057659,
        "finish": 1712135957.407821,
        "duration": 0.3501620292663574,
        "processing": 0.011919021606445312,
        "date_start": "2024-04-03T11:19:17+02:00",
        "date_finish": "2024-04-03T11:19:17+02:00",
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
[`sale_cashbox_handler[]`](../data-types.md#sale_cashbox_handler) | List of handlers registered in the system  ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient permissions to retrieve the list of handlers | 403 ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-cashbox-handler-add.md)
- [{#T}](./sale-cashbox-handler-update.md)
- [{#T}](./sale-cashbox-handler-delete.md)
- [{#T}](./sale-cashbox-add.md)
- [{#T}](./sale-cashbox-update.md)
- [{#T}](./sale-cashbox-list.md)
- [{#T}](./sale-cashbox-delete.md)
- [{#T}](./sale-cashbox-check-apply.md)