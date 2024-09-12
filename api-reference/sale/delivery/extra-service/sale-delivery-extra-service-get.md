# Get Information About All Services of a Specific Delivery Service sale.delivery.extra.service.get

> Scope: [`sale, delivery`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves information about all services of a specific delivery service.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DELIVERY_ID***
[`sale_delivery_service.ID`](../../data-types.md) | Identifier of the delivery service ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":198}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.extra.service.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":198,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.extra.service.get
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.extra.service.get', {
            DELIVERY_ID: 198,
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
        'sale.delivery.extra.service.get',
        [
            'DELIVERY_ID' => 198,
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
         "ID":"136",
         "CODE":"door_delivery",
         "NAME":"Door Delivery",
         "DESCRIPTION":"Door Delivery Description",
         "ACTIVE":"Y",
         "SORT":"100",
         "TYPE":"checkbox",
         "PRICE":99.99
      },
      {
         "ID":"129",
         "CODE":"cargo_type",
         "NAME":"Cargo Type",
         "DESCRIPTION":"Cargo Type",
         "ACTIVE":"Y",
         "SORT":"200",
         "TYPE":"enum",
         "ITEMS":[
            {
               "TITLE":"Documents",
               "CODE":null,
               "PRICE":"69.99"
            },
            {
               "TITLE":"Small Package(s)",
               "CODE":null,
               "PRICE":"129.99"
            },
            {
               "TITLE":"Large Package(s)",
               "CODE":null,
               "PRICE":"199.99"
            }
         ]
      }
   ],
   "time":{
      "start":1714551728.295288,
      "finish":1714551728.519896,
      "duration":0.2246079444885254,
      "processing":0.01918506622314453,
      "date_start":"2024-05-01T11:22:08+03:00",
      "date_finish":"2024-05-01T11:22:08+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_delivery_extra_service[]`](../../data-types.md) | List of all services of the delivery service ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
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
|| `ERROR_DELIVERY_NOT_FOUND` | Delivery service with the specified identifier not found | `400` || 
|| `ERROR_CHECK_FAILURE` | Error in validating incoming parameters (details in the error description) | `400` || 
|| `ACCESS_DENIED` | Insufficient permissions to retrieve the list of services | `403` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-extra-service-add.md)
- [{#T}](./sale-delivery-extra-service-update.md)
- [{#T}](./sale-delivery-extra-service-delete.md)