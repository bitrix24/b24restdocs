# Update Delivery Service Settings sale.delivery.config.update

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

This method updates the delivery service settings.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_delivery_service.ID`](../../data-types.md) | Identifier of the delivery service.
 ||
|| **CONFIG**
[`object[]`](../../../data-types.md) | Values of the delivery service settings (detailed description provided [below](#parametr-config)).
The structure of the settings (code, name, data type) is defined when creating or updating the delivery service handler using the methods:
- [sale.delivery.handler.add](../handler/sale-delivery-handler-add.md)
- [sale.delivery.handler.update](../handler/sale-delivery-handler-update.md)||
|#

### Parameter CONFIG

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../../data-types.md) | Symbolic code of the setting ||
|| **VALUE***
[`any`](../../../data-types.md) | Value of the setting ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":196,"CONFIG":[{"CODE":"SETTING_1","VALUE":"New SETTING_1 string value"},{"CODE":"SETTING_2","VALUE":"N"},{"CODE":"SETTING_3","VALUE":999.99},{"CODE":"SETTING_4","VALUE":"Option2Code"},{"CODE":"SETTING_5","VALUE":"03/25/2023"},{"CODE":"SETTING_6","VALUE":"0000144962"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.config.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":196,"CONFIG":[{"CODE":"SETTING_1","VALUE":"New SETTING_1 string value"},{"CODE":"SETTING_2","VALUE":"N"},{"CODE":"SETTING_3","VALUE":999.99},{"CODE":"SETTING_4","VALUE":"Option2Code"},{"CODE":"SETTING_5","VALUE":"03/25/2023"},{"CODE":"SETTING_6","VALUE":"0000144962"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.config.update
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.config.update', {
            ID: 196,
            CONFIG: [{
                    CODE: "SETTING_1",
                    VALUE: "New SETTING_1 string value",
                },
                {
                    CODE: "SETTING_2",
                    VALUE: "N",
                },
                {
                    CODE: "SETTING_3",
                    VALUE: 999.99,
                },
                {
                    CODE: "SETTING_4",
                    VALUE: "Option2Code",
                },
                {
                    CODE: "SETTING_5",
                    VALUE: "03/25/2023",
                },
                {
                    CODE: "SETTING_6",
                    VALUE: "0000144962",
                },
            ],

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
        'sale.delivery.config.update',
        [
            'ID' => 196,
            'CONFIG' => [
                ['CODE' => "SETTING_1", 'VALUE' => "New SETTING_1 string value"],
                ['CODE' => "SETTING_2", 'VALUE' => "N"],
                ['CODE' => "SETTING_3", 'VALUE' => 999.99],
                ['CODE' => "SETTING_4", 'VALUE' => "Option2Code"],
                ['CODE' => "SETTING_5", 'VALUE' => "03/25/2023"],
                ['CODE' => "SETTING_6", 'VALUE' => "0000144962"],
            ]
        ]
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
   "result":true,
   "time":{
      "start":1714134319.861777,
      "finish":1714134320.202336,
      "duration":0.3405590057373047,
      "processing":0.07335114479064941,
      "date_start":"2024-04-26T15:25:19+03:00",
      "date_finish":"2024-04-26T15:25:20+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of updating the delivery service settings ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
   "error":"ERROR_DELIVERY_NOT_FOUND",
   "error_description":"Delivery not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_DELIVERY_NOT_FOUND` | Delivery service with the specified identifier (ID) not found | 400 ||
|| `ERROR_CHECK_FAILURE` | Validation error of incoming parameters (details in the error description) | 400 ||
|| `ERROR_DELIVERY_CONFIG_UPDATE` | Error while attempting to update the delivery service settings values | 400 ||
|| `ACCESS_DENIED` | Insufficient permissions to update the delivery service settings | 403 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-add.md)
- [{#T}](./sale-delivery-delete.md)
- [{#T}](./sale-delivery-update.md)
- [{#T}](./sale-delivery-config-get.md)
- [{#T}](./sale-delivery-get-list.md)