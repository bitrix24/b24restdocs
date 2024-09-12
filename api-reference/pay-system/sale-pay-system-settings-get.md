# Get Payment System Settings sale.paysystem.settings.get

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow changing settings")

This method returns the settings of the payment system. The structure of the settings is defined when adding the payment system handler in the method [sale.paysystem.handler.add](./sale-pay-system-handler-add.md) under the `CODES` key of the `SETTINGS` parameter.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_paysystem.ID`](../sale/data-types.md) | Identifier of the payment system for which to retrieve settings
||
|| **PERSON_TYPE_ID***
[`sale_person_type.id`](../sale/data-types.md) | Identifier of the payer type for which to retrieve settings. To get default settings, pass `0`
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
    -d '{"ID":11,"PERSON_TYPE_ID":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.settings.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":11,"PERSON_TYPE_ID":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.settings.get
    ```

- JS

    ```js
    BX24.callMethod('sale.paysystem.settings.get', {
        'ID': 11,
        'PERSON_TYPE_ID': 1,
    }, 
        function(result) 
        { 
            if(result.error()) 
                console.error(result.error()); 
            else 
            { 
                console.dir(result.data()); 
            } 
        } 
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paysystem.settings.get',
        [
            'ID' => 11,
            'PERSON_TYPE_ID' => 1
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
        "REST_SERVICE_ID_IFRAME": "snum",
        "REST_SERVICE_KEY_IFRAME": "skey",
        "PS_WORK_MODE_IFRAME": "REGULAR"
    },
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
[`object`](../data-types.md) | Root element of the response. 

The keys of the object are the parameter codes specified when adding the handler via [sale.paysystem.handler.add](./sale-pay-system-handler-add.md) in the `CODES` parameter. 

The values of the object are the parameter values:
- either filled in manually by the user when creating the payment system
- or specified when adding the payment system via [sale.paysystem.add](./sale-pay-system-add.md)
- or specified when executing the method [sale.paysystem.settings.update](./sale-pay-system-settings-update.md) ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_CHECK_FAILURE",
    "error_description": "Pay system not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient permissions to read settings | 403 ||
|| `ERROR_CHECK_FAILURE` | One of the required fields is missing or the specified payment system was not found (details can be found in the error description) | 400 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-pay-system-handler-add.md)
- [{#T}](./sale-pay-system-handler-update.md)
- [{#T}](./sale-pay-system-handler-list.md)
- [{#T}](./sale-pay-system-handler-delete.md)
- [{#T}](./sale-pay-system-add.md)
- [{#T}](./sale-pay-system-update.md)
- [{#T}](./sale-pay-system-list.md)
- [{#T}](./sale-pay-system-settings-update.md)
- [{#T}](./sale-pay-system-delete.md)
- [{#T}](./sale-pay-system-pay-payment.md)
- [{#T}](./sale-pay-system-pay-invoice.md)
- [{#T}](./sale-pay-system-settings-payment-get.md)
- [{#T}](./sale-pay-system-settings-invoice-get.md)