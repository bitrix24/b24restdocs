# Update Payment System

> Method Name: **sale.paysystem.update**
>
> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow to modify settings")

This method updates the payment system.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_paysystem.ID`](../sale/data-types.md) | Identifier of the payment system ||
|| **FIELDS**
[`object`](../data-types.md) | Object containing new field values (detailed description provided [below](#parametr-fields)) ||
|#

### FIELDS Parameter

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](../data-types.md) | Name of the payment system ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description of the payment system ||
|| **PERSON_TYPE_ID**
[`sale_person_type.id`](../sale/data-types.md) | Identifier of the payer type ||
|| **BX_REST_HANDLER**
[`sale_paysystem_handler.CODE`](../sale/data-types.md#sale_paysystem_handler) | Code of the REST handler specified when adding the handler using the method [sale.paysystem.handler.add](./sale-pay-system-handler-add.md) ||
|| **ACTIVE**
[`string`](../data-types.md) | Indicator of the payment system's activity. Possible values:
- `Y` — yes
- `N` — no
||
|| **LOGOTYPE**
[`string`](../data-types.md) | Logo of the payment system (image in Base64 format) ||
|| **NEW_WINDOW**
[`string`](../data-types.md) | Flag for the setting "Open in new window". Possible values:
- `Y` — yes
- `N` — no
||
|| **XML_ID**
[`string`](../data-types.md) | External identifier of the payment system ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":12,"FIELDS":{"NAME":"New Payment System Name","DESCRIPTION":"New Payment System Description","PERSON_TYPE_ID":1,"BX_REST_HANDLER":"resthandlercode","ACTIVE":"Y","NEW_WINDOW":"N","LOGOTYPE":"/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAIBAQIBAQICAgICAgICAwUDAwMDAwYEBAMFBwYHBwcGBwcICQsJCAgKCAcHCg0KCgsMDAwMBwkODw0MDgsMDAz/2wBDAQICAgMDAwYDAwYMCAcIDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAz/wAARCAASABUDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD73/4Oa/25vEf7CH/BK/XNW8GarrPh/wAZeONcsfCejazpk3k3GkyS+ZdTSh/vKWtbO5jDJhlaVWBBGR4V/wAFMP28vj1/wRc/4I+fs56O3jmLxh+0j4m1Wy0/V5tft11a9v4xDNc30SYJE/kTSWdn52S7o6tne+4fNv7f3/BSHwX8d/2xvjL+y/8A8FBrHxt4R+F+g/EKDxB8NtY8L6WLeTTbG3N5BDLdMqSy3FtdWkynfFFI4aSUDYQvl8r/AMFDv+Cr37P37eP/AAWX/Zz+IHw10/42fGi6+GOp2VlpnhbSrBNL03WLtLw3cF7ZtOzXJlExjWWGW1hEy20YM0aoSwB/RtRRRQBw/wAb/wBmb4b/ALTWk2On/Ej4feB/iDY6ZMbiztvEuhWurQ2khG0vGtwjhGK8EqASOKT4H/swfDX9mTTr+z+G3w78DfD201WRZr2Dw1oNrpMd46ghWkW3jQOwBIBbJAJoooA7miiigD//2Q=="}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":12,"FIELDS":{"NAME":"New Payment System Name","DESCRIPTION":"New Payment System Description","PERSON_TYPE_ID":1,"BX_REST_HANDLER":"resthandlercode","ACTIVE":"Y","NEW_WINDOW":"N","LOGOTYPE":"/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAIBAQIBAQICAgICAgICAwUDAwMDAwYEBAMFBwYHBwcGBwcICQsJCAgKCAcHCg0KCgsMDAwMBwkODw0MDgsMDAz/2wBDAQICAgMDAwYDAwYMCAcIDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAz/wAARCAASABUDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD73/4Oa/25vEf7CH/BK/XNW8GarrPh/wAZeONcsfCejazpk3k3GkyS+ZdTSh/vKWtbO5jDJhlaVWBBGR4V/wAFMP28vj1/wRc/4I+fs56O3jmLxh+0j4m1Wy0/V5tft11a9v4xDNc30SYJE/kTSWdn52S7o6tne+4fNv7f3/BSHwX8d/2xvjL+y/8A8FBrHxt4R+F+g/EKDxB8NtY8L6WLeTTbG3N5BDLdMqSy3FtdWkynfFFI4aSUDYQvl8r/AMFDv+Cr37P37eP/AAWX/Zz+IHw10/42fGi6+GOp2VlpnhbSrBNL03WLtLw3cF7ZtOzXJlExjWWGW1hEy20YM0aoSwB/RtRRRQBw/wAb/wBmb4b/ALTWk2On/Ej4feB/iDY6ZMbiztvEuhWurQ2khG0vGtwjhGK8EqASOKT4H/swfDX9mTTr+z+G3w78DfD201WRZr2Dw1oNrpMd46ghWkW3jQOwBIBbJAJoooA7miiigD//2Q=="},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.update
    ```

- JS

    ```js
    BX24.callMethod('sale.paysystem.update',
    {
        "ID": 12,
        "FIELDS": {
            "NAME": "New Payment System Name",
            "DESCRIPTION": "New Payment System Description",
            "PERSON_TYPE_ID": 1,
            "BX_REST_HANDLER": 'resthandlercode',
            "ACTIVE": 'Y',
            'NEW_WINDOW': 'N',
            'LOGOTYPE': '/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAIBAQIBAQICAgICAgICAwUDAwMDAwYEBAMFBwYHBwcGBwcICQsJCAgKCAcHCg0KCgsMDAwMBwkODw0MDgsMDAz/2wBDAQICAgMDAwYDAwYMCAcIDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAz/wAARCAASABUDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD73/4Oa/25vEf7CH/BK/XNW8GarrPh/wAZeONcsfCejazpk3k3GkyS+ZdTSh/vKWtbO5jDJhlaVWBBGR4V/wAFMP28vj1/wRc/4I+fs56O3jmLxh+0j4m1Wy0/V5tft11a9v4xDNc30SYJE/kTSWdn52S7o6tne+4fNv7f3/BSHwX8d/2xvjL+y/8A8FBrHxt4R+F+g/EKDxB8NtY8L6WLeTTbG3N5BDLdMqSy3FtdWkynfFFI4aSUDYQvl8r/AMFDv+Cr37P37eP/AAWX/Zz+IHw10/42fGi6+GOp2VlpnhbSrBNL03WLtLw3cF7ZtOzXJlExjWWGW1hEy20YM0aoSwB/RtRRRQBw/wAb/wBmb4b/ALTWk2On/Ej4feB/iDY6ZMbiztvEuhWurQ2khG0vGtwjhGK8EqASOKT4H/swfDX9mTTr+z+G3w78DfD201WRZr2Dw1oNrpMd46ghWkW3jQOwBIBbJAJoooA7miiigD//2Q==',
        }
    }, 
        function(result) 
        { 
            if(result.error()) 
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
        'sale.paysystem.update',
        [
            'ID' => 12,
            'FIELDS' => [
                'NAME' => 'New Payment System Name',
                'DESCRIPTION' => 'New Payment System Description',
                'PERSON_TYPE_ID' => 1,
                'BX_REST_HANDLER' => 'resthandlercode',
                'ACTIVE' => 'Y',
                'NEW_WINDOW' => 'N',
                'LOGOTYPE' => '/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAIBAQIBAQICAgICAgICAwUDAwMDAwYEBAMFBwYHBwcGBwcICQsJCAgKCAcHCg0KCgsMDAwMBwkODw0MDgsMDAz/2wBDAQICAgMDAwYDAwYMCAcIDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAz/wAARCAASABUDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD73/4Oa/25vEf7CH/BK/XNW8GarrPh/wAZeONcsfCejazpk3k3GkyS+ZdTSh/vKWtbO5jDJhlaVWBBGR4V/wAFMP28vj1/wRc/4I+fs56O3jmLxh+0j4m1Wy0/V5tft11a9v4xDNc30SYJE/kTSWdn52S7o6tne+4fNv7f3/BSHwX8d/2xvjL+y/8A8FBrHxt4R+F+g/EKDxB8NtY8L6WLeTTbG3N5BDLdMqSy3FtdWkynfFFI4aSUDYQvl8r/AMFDv+Cr37P37eP/AAWX/Zz+IHw10/42fGi6+GOp2VlpnhbSrBNL03WLtLw3cF7ZtOzXJlExjWWGW1hEy20YM0aoSwB/RtRRRQBw/wAb/wBmb4b/ALTWk2On/Ej4feB/iDY6ZMbiztvEuhWurQ2khG0vGtwjhGK8EqASOKT4H/swfDX9mTTr+z+G3w78DfD201WRZr2Dw1oNrpMd46ghWkW3jQOwBIBbJAJoooA7miiigD//2Q==',
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
[`boolean`](../data-types.md) | Result of the payment system update ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": " ERROR_HANDLER_NOT_FOUND",
    "error_description": " Handler not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Access denied. The application is trying to modify a payment system added by another application, or lacks sufficient permissions to modify the payment system | 403 ||
|| `ERROR_HANDLER_NOT_FOUND` | The handler specified in the `BX_REST_HANDLER` parameter was not found | 400 ||
|| `ERROR_PERSON_TYPE_NOT_FOUND` | The payer type specified in the `PERSON_TYPE_ID` parameter was not found | 400 ||
|| `ERROR_PAY_SYSTEM_NOT_FOUND` | The payment system with the specified `ID` was not found | 400 ||
|| `ERROR_CHECK_FAILURE` | The `ID` parameter was not specified | 400 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-pay-system-handler-add.md)
- [{#T}](./sale-pay-system-handler-update.md)
- [{#T}](./sale-pay-system-handler-list.md)
- [{#T}](./sale-pay-system-handler-delete.md)
- [{#T}](./sale-pay-system-add.md)
- [{#T}](./sale-pay-system-list.md)
- [{#T}](./sale-pay-system-settings-get.md)
- [{#T}](./sale-pay-system-settings-update.md)
- [{#T}](./sale-pay-system-delete.md)
- [{#T}](./sale-pay-system-pay-payment.md)
- [{#T}](./sale-pay-system-pay-invoice.md)
- [{#T}](./sale-pay-system-settings-payment-get.md)
- [{#T}](./sale-pay-system-settings-invoice-get.md)