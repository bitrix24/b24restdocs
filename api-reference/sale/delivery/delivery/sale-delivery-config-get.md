# Get Delivery Service Settings sale.delivery.config.get

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

This method retrieves the settings of the delivery service.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_delivery_service.ID`](../../data-types.md) | Identifier of the delivery service.
 ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":196}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.config.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":196,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.config.get
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.config.get', {
            ID: 196,
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
        'sale.delivery.config.get',
        [
            'ID' => 196
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
   "result":[
      {
         "CODE":"SETTING_1",
         "VALUE":"New SETTING_1 string value"
      },
      {
         "CODE":"SETTING_2",
         "VALUE":"N"
      },
      {
         "CODE":"SETTING_3",
         "VALUE":"999.99"
      },
      {
         "CODE":"SETTING_4",
         "VALUE":"Option2Code"
      },
      {
         "CODE":"SETTING_5",
         "VALUE":"03/25/2023"
      },
      {
         "CODE":"SETTING_6",
         "VALUE":"0000144962"
      }
   ],
   "time":{
      "start":1714137257.450324,
      "finish":1714137257.672526,
      "duration":0.22220182418823242,
      "processing":0.029755115509033203,
      "date_start":"2024-04-26T16:14:17+03:00",
      "date_finish":"2024-04-26T16:14:17+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | Values of the delivery service settings.
The structure of the settings (code, name, data type) is defined when creating or updating the delivery service handler using the methods:
- [sale.delivery.handler.add](../handler/sale-delivery-handler-add.md)
- [sale.delivery.handler.update](../handler/sale-delivery-handler-update.md) ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

### Key result

#|
|| **Name**
`type` | **Description** ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the setting ||
|| **VALUE**
[`any`](../../../data-types.md) | Value of the setting ||
|#

## Error Handling

HTTP Status: **400**, **403**

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
|| `ACCESS_DENIED` | Insufficient permissions to retrieve delivery service settings | 403 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-add.md)
- [{#T}](./sale-delivery-delete.md)
- [{#T}](./sale-delivery-update.md)
- [{#T}](./sale-delivery-config-update.md)
- [{#T}](./sale-delivery-get-list.md)