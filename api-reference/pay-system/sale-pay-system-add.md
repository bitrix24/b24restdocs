# Add Payment System sale.paysystem.add

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow to modify settings")

This method adds a payment system.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../data-types.md) | Name of the payment system ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description of the payment system ||
|| **PERSON_TYPE_ID***
[`sale_person_type.id`](../sale/data-types.md) | Identifier of the payer type ||
|| **BX_REST_HANDLER***
[`sale_paysystem_handler.CODE`](../sale/data-types.md#sale_paysystem_handler) | Code of the REST handler specified when adding the handler using the method [sale.paysystem.handler.add](./sale-pay-system-handler-add.md) ||
|| **ACTIVE**
[`string`](../data-types.md) | Indicator of the payment system's activity. Possible values:
- `Y` — yes
- `N` — no

If not provided, defaults to `N` ||
|| **SETTINGS**
[`object`](../data-types.md) | List of handler settings values in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the name of the setting and `value` is an object containing the keys [TYPE](#possible-values-of-key-type) and [VALUE](#possible-values-of-key-value) (see description below). 

The structure of the settings is defined when adding the payment system handler in the method [sale.paysystem.handler.add](./sale-pay-system-handler-add.md) under the `CODES` key of the `SETTINGS` parameter ||
|| **ENTITY_REGISTRY_TYPE***
[`string`](../data-types.md) | Binding of the payment system:
- `ORDER` — value for store orders, deals, SPAs
- `CRM_INVOICE` — value for CRM invoices
- `CRM_QUOTE` — value for CRM estimates
||
|| **LOGOTYPE**
[`string`](../data-types.md) | Logo of the payment system (image in Base64 format) ||
|| **NEW_WINDOW**
[`string`](../data-types.md) | Flag for the "Open in new window" setting. Possible values:
- `Y` — yes
- `N` — no

If not provided, defaults to `N` ||
|| **XML_ID**
[`string`](../data-types.md) | External identifier of the payment system. Can be used as an additional parameter for filtering in [sale.paysystem.list](./sale-pay-system-list.md) ||
|#

### Possible Values of the TYPE Key

#|
|| **Name** | **Description** ||
|| **ORDER** | Parameters ||
|| **PROPERTY** | Invoice properties ||
|| **PAYMENT** | Payment ||
|| **USER** | User ||
|| **VALUE** | Arbitrary string value ||
|| **Y\N** | Checkbox ||
|#

### Possible Values of the VALUE Key

#|
|| **Name** | **Description** ||
|| **ORDER** | 
- `ID` — identifier
- `ACCOUNT_NUMBER` — order number
- `ORDER_TOPIC` — subject
- `DATE_INSERT` — order date 
- `DATE_INSERT_DATE` — order date without time
- `DATE_BILL` — date and time of billing
- `DATE_BILL_DATE` — billing date
- `DATE_PAY_BEFORE` — payment deadline
- `SHOULD_PAY` — invoice amount
- `CURRENCY` — currency
- `PRICE` — order cost
- `PRICE_DELIVERY` — delivery cost
- `DISCOUNT_VALUE` — discount amount
- `USER_ID` — buyer code
- `PAY_SYSTEM_ID` — payment system code
- `DELIVERY_ID` — delivery service code
- `TAX_VALUE` — tax
- `USER_DESCRIPTION` — comment
||
|| **PAYMENT** | 
- `ID` — identifier
- `ACCOUNT_NUMBER` — payment number
- `DATE_BILL` — date and time of billing
- `DATE_BILL_DATE` — billing date without time
- `SUM` — invoice amount
- `CURRENCY` — currency
- `PAID` — paid
- `DATE_PAID` — payment date
- `PAY_SYSTEM_ID` — payment system code
- `PAY_VOUCHER_NUM` — voucher number
- `PAY_VOUCHER_DATE` — voucher date
- `DATE_PAY_BEFORE` — pay by
- `XML_ID` — XML identifier
- `PAY_SYSTEM_NAME` — name of the payment system
- `COMPANY_ID` — company code
- `PAY_RETURN_NUM` — return number
- `PAY_RETURN_DATE` — return date
- `PAY_RETURN_COMMENT` — return comment
||
|| **USER** | 
- `ID` — buyer code,
- `LOGIN` — login
- `NAME` — first name
- `SECOND_NAME` — middle name
- `LAST_NAME` — last name
- `EMAIL` — e-mail
- `PERSONAL_PROFESSION` — profession
- `PERSONAL_WWW` — personal website
- `PERSONAL_ICQ` — ICQ number
- `PERSONAL_GENDER` — gender
- `PERSONAL_FAX` — fax number
- `PERSONAL_MOBILE` — mobile number
- `PERSONAL_STREET` — address
- `PERSONAL_MAILBOX` — mailbox
- `PERSONAL_CITY` — city
- `PERSONAL_STATE` — state
- `PERSONAL_ZIP` — zip code
- `PERSONAL_COUNTRY` — country
- `WORK_COMPANY` — company
- `WORK_DEPARTMENT` — department
- `WORK_POSITION` — position
- `WORK_WWW` — company website
- `WORK_PHONE` — work phone
- `WORK_FAX` — work fax
- `WORK_STREET` — company address
- `WORK_MAILBOX` — work mailbox
- `WORK_CITY` — company city
- `WORK_STATE` — company state
- `WORK_ZIP` — company zip code
- `WORK_COUNTRY` — company country
||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NAME":"Card Payment","DESCRIPTION":"Easily pay for purchases with a card.","XML_ID":"my_ps_id","PERSON_TYPE_ID":1,"BX_REST_HANDLER":"resthandlercode","ACTIVE":"Y","ENTITY_REGISTRY_TYPE":"ORDER","LOGOTYPE":"/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAIBAQIBAQICAgICAgICAwUDAwMDAwYEBAMFBwYHBwcGBwcICQsJCAgKCAcHCg0KCgsMDAwMBwkODw0MDgsMDAz/2wBDAQICAgMDAwYDAwYMCAcIDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAz/wAARCAASABUDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD73/4Oa/25vEf7CH/BK/XNW8GarrPh/wAZeONcsfCejazpk3k3GkyS+ZdTSh/vKWtbO5jDJhlaVWBBGR4V/wAFMP28vj1/wRc/4I+fs56O3jmLxh+0j4m1Wy0/V5tft11a9v4xDNc30SYJE/kTSWdn52S7o6tne+4fNv7f3/BSHwX8d/2xvjL+y/8A8FBrHxt4R+F+g/EKDxB8NtY8L6WLeTTbG3N5BDLdMqSy3FtdWkynfFFI4aSUDYQvl8r/AMFDv+Cr37P37eP/AAWX/Zz+IHw10/42fGi6+GOp2VlpnhbSrBNL03WLtLw3cF7ZtOzXJlExjWWGW1hEy20YM0aoSwB/RtRRRQBw/wAb/wBmb4b/ALTWk2On/Ej4feB/iDY6ZMbiztvEuhWurQ2khG0vGtwjhGK8EqASOKT4H/swfDX9mTTr+z+G3w78DfD201WRZr2Dw1oNrpMd46ghWkW3jQOwBIBbJAJoooA7miiigD//2Q==","NEW_WINDOW":"N","SETTINGS":{"REST_SERVICE_ID":{"TYPE":"VALUE","VALUE":"SERVICE ID VALUE"},"REST_SERVICE_KEY":{"TYPE":"VALUE","VALUE":"KEY ID VALUE"},"PAYMENT_ID":{"TYPE":"PAYMENT","VALUE":"ACCOUNT_NUMBER"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NAME":"Card Payment","DESCRIPTION":"Easily pay for purchases with a card.","XML_ID":"my_ps_id","PERSON_TYPE_ID":1,"BX_REST_HANDLER":"resthandlercode","ACTIVE":"Y","ENTITY_REGISTRY_TYPE":"ORDER","LOGOTYPE":"/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAIBAQIBAQICAgICAgICAwUDAwMDAwYEBAMFBwYHBwcGBwcICQsJCAgKCAcHCg0KCgsMDAwMBwkODw0MDgsMDAz/2wBDAQICAgMDAwYDAwYMCAcIDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAz/wAARCAASABUDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD73/4Oa/25vEf7CH/BK/XNW8GarrPh/wAZeONcsfCejazpk3k3GkyS+ZdTSh/vKWtbO5jDJhlaVWBBGR4V/wAFMP28vj1/wRc/4I+fs56O3jmLxh+0j4m1Wy0/V5tft11a9v4xDNc30SYJE/kTSWdn52S7o6tne+4fNv7f3/BSHwX8d/2xvjL+y/8A8FBrHxt4R+F+g/EKDxB8NtY8L6WLeTTbG3N5BDLdMqSy3FtdWkynfFFI4aSUDYQvl8r/AMFDv+Cr37P37eP/AAWX/Zz+IHw10/42fGi6+GOp2VlpnhbSrBNL03WLtLw3cF7ZtOzXJlExjWWGW1hEy20YM0aoSwB/RtRRRQBw/wAb/wBmb4b/ALTWk2On/Ej4feB/iDY6ZMbiztvEuhWurQ2khG0vGtwjhGK8EqASOKT4H/swfDX9mTTr+z+G3w78DfD201WRZr2Dw1oNrpMd46ghWkW3jQOwBIBbJAJoooA7miiigD//2Q==","NEW_WINDOW":"N","SETTINGS":{"REST_SERVICE_ID":{"TYPE":"VALUE","VALUE":"SERVICE ID VALUE"},"REST_SERVICE_KEY":{"TYPE":"VALUE","VALUE":"KEY ID VALUE"},"PAYMENT_ID":{"TYPE":"PAYMENT","VALUE":"ACCOUNT_NUMBER"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.add
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.paysystem.add",
        {
            'NAME' : 'Card Payment',
            'DESCRIPTION': 'Easily pay for purchases with a card.',
            'XML_ID': 'my_ps_id',
            'PERSON_TYPE_ID' : 1,
            'BX_REST_HANDLER' : 'resthandlercode',
            'ACTIVE' : 'Y',
            'ENTITY_REGISTRY_TYPE': 'ORDER',
            'LOGOTYPE': '/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAIBAQIBAQICAgICAgICAwUDAwMDAwYEBAMFBwYHBwcGBwcICQsJCAgKCAcHCg0KCgsMDAwMBwkODw0MDgsMDAz/2wBDAQICAgMDAwYDAwYMCAcIDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAz/wAARCAASABUDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD73/4Oa/25vEf7CH/BK/XNW8GarrPh/wAZeONcsfCejazpk3k3GkyS+ZdTSh/vKWtbO5jDJhlaVWBBGR4V/wAFMP28vj1/wRc/4I+fs56O3jmLxh+0j4m1Wy0/V5tft11a9v4xDNc30SYJE/kTSWdn52S7o6tne+4fNv7f3/BSHwX8d/2xvjL+y/8A8FBrHxt4R+F+g/EKDxB8NtY8L6WLeTTbG3N5BDLdMqSy3FtdWkynfFFI4aSUDYQvl8r/AMFDv+Cr37P37eP/AAWX/Zz+IHw10/42fGi6+GOp2VlpnhbSrBNL03WLtLw3cF7ZtOzXJlExjWWGW1hEy20YM0aoSwB/RtRRRQBw/wAb/wBmb4b/ALTWk2On/Ej4feB/iDY6ZMbiztvEuhWurQ2khG0vGtwjhGK8EqASOKT4H/swfDX9mTTr+z+G3w78DfD201WRZr2Dw1oNrpMd46ghWkW3jQOwBIBbJAJoooA7miiigD//2Q==',
            'NEW_WINDOW': 'N',
            'SETTINGS' : {
                'REST_SERVICE_ID' : {
                    'TYPE' : 'VALUE',
                    'VALUE' : 'SERVICE ID VALUE'
                },
                'REST_SERVICE_KEY' : {
                    'TYPE' : 'VALUE',
                    'VALUE' : 'KEY ID VALUE'
                },
                'PAYMENT_ID': {
                    'TYPE': 'PAYMENT',
                    'VALUE': 'ACCOUNT_NUMBER',
                }
            }
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paysystem.add',
        [
            'NAME' => 'Card Payment',
            'DESCRIPTION' => 'Easily pay for purchases with a card.',
            'XML_ID' => 'my_ps_id',
            'PERSON_TYPE_ID' => 1,
            'BX_REST_HANDLER' => 'resthandlercode',
            'ACTIVE' => 'Y',
            'ENTITY_REGISTRY_TYPE' => 'ORDER',
            'LOGOTYPE' => '/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAIBAQIBAQICAgICAgICAwUDAwMDAwYEBAMFBwYHBwcGBwcICQsJCAgKCAcHCg0KCgsMDAwMBwkODw0MDgsMDAz/2wBDAQICAgMDAwYDAwYMCAcIDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAz/wAARCAASABUDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD73/4Oa/25vEf7CH/BK/XNW8GarrPh/wAZeONcsfCejazpk3k3GkyS+ZdTSh/vKWtbO5jDJhlaVWBBGR4V/wAFMP28vj1/wRc/4I+fs56O3jmLxh+0j4m1Wy0/V5tft11a9v4xDNc30SYJE/kTSWdn52S7o6tne+4fNv7f3/BSHwX8d/2xvjL+y/8A8FBrHxt4R+F+g/EKDxB8NtY8L6WLeTTbG3N5BDLdMqSy3FtdWkynfFFI4aSUDYQvl8r/AMFDv+Cr37P37eP/AAWX/Zz+IHw10/42fGi6+GOp2VlpnhbSrBNL03WLtLw3cF7ZtOzXJlExjWWGW1hEy20YM0aoSwB/RtRRRQBw/wAb/wBmb4b/ALTWk2On/Ej4feB/iDY6ZMbiztvEuhWurQ2khG0vGtwjhGK8EqASOKT4H/swfDX9mTTr+z+G3w78DfD201WRZr2Dw1oNrpMd46ghWkW3jQOwBIBbJAJoooA7miiigD//2Q==',
            'NEW_WINDOW' => 'N',
            'SETTINGS' => [
                'REST_SERVICE_ID' => [
                    'TYPE' => 'VALUE',
                    'VALUE' => 'SERVICE ID VALUE'
                ],
                'REST_SERVICE_KEY' => [
                    'TYPE' => 'VALUE',
                    'VALUE' => 'KEY ID VALUE'
                ],
                'PAYMENT_ID' => [
                    'TYPE' => 'PAYMENT',
                    'VALUE' => 'ACCOUNT_NUMBER',
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
    "result": 1,
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
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
[`integer`](../data-types.md) | Identifier of the added payment system ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_CHECK_FAILURE",
    "error_description": "Parameter NAME is not defined"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient permissions to add the payment system | 403 ||
|| `ERROR_CHECK_FAILURE` | Required field value is missing or one of the field values is incorrect | 400 ||
|| `ERROR_PAY_SYSTEM_ADD` | Other errors. For detailed information about the error, see `error_description` | 400 ||
|| `ERROR_HANDLER_NOT_FOUND` | Handler specified in the `BX_REST_HANDLER` parameter not found | 400 ||
|| `ERROR_PERSON_TYPE_NOT_FOUND` | Payer type specified in the `PERSON_TYPE_ID` parameter not found | 400 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-pay-system-handler-add.md)
- [{#T}](./sale-pay-system-handler-update.md)
- [{#T}](./sale-pay-system-handler-list.md)
- [{#T}](./sale-pay-system-handler-delete.md)
- [{#T}](./sale-pay-system-update.md)
- [{#T}](./sale-pay-system-list.md)
- [{#T}](./sale-pay-system-settings-get.md)
- [{#T}](./sale-pay-system-settings-update.md)
- [{#T}](./sale-pay-system-delete.md)
- [{#T}](./sale-pay-system-pay-payment.md)
- [{#T}](./sale-pay-system-pay-invoice.md)
- [{#T}](./sale-pay-system-settings-payment-get.md)
- [{#T}](./sale-pay-system-settings-invoice-get.md)