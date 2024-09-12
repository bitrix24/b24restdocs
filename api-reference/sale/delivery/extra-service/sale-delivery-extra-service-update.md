# Update Delivery Service sale.delivery.extra.service.update

> Scope: [`sale, delivery`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the delivery service.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_delivery_extra_service.ID`](../../data-types.md) | Identifier of the service.

You can obtain the identifiers of delivery services using the method [sale.delivery.extra.service.get](./sale-delivery-extra-service-get.md)
||
|| **NAME**
[`string`](../../../data-types.md) | Name of the service ||
|| **ACTIVE**
[`string`](../../../data-types.md) | Indicator of the service's activity. Possible values:
- `Y` — yes
- `N` — no
||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the service ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Description of the service ||
|| **PRICE**
[`double`](../../../data-types.md) | Cost of the service in the delivery service's currency.

This field is relevant only for services of type `single service (checkbox)` and `quantity service (quantity)`
||
|| **ITEMS**
[`object[]`](../../../data-types.md) | List of available options for selection (detailed description is provided [below](#parameter-items)).

This field is relevant only for services of type `list (enum)`
||
|#

### Parameter ITEMS

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE***
[`string`](../../../data-types.md) | Name of the list option ||
|| **CODE***
[`string`](../../../data-types.md) | Symbolic code of the list option ||
|| **PRICE**
[`double`](../../../data-types.md) | Cost of the service when selecting this option in the delivery service's currency ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Updating a service of type `Quantity Service`:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":128,"ACTIVE":"N","CODE":"door_delivery","NAME":"Door Delivery New Name","DESCRIPTION":"Door Delivery New Description","SORT":200,"PRICE":399.99}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.extra.service.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":128,"ACTIVE":"N","CODE":"door_delivery","NAME":"Door Delivery New Name","DESCRIPTION":"Door Delivery New Description","SORT":200,"PRICE":399.99,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.extra.service.update
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.extra.service.update', {
            ID: 128,
            ACTIVE: "N",
            CODE: "door_delivery",
            NAME: "Door Delivery New Name",
            DESCRIPTION: "Door Delivery New Description",
            SORT: 200,
            PRICE: 399.99,
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
        'sale.delivery.extra.service.update',
        [
            'ID' => 128,
            'ACTIVE' => "N",
            'CODE' => "door_delivery",
            'NAME' => "Door Delivery New Name",
            'DESCRIPTION' => "Door Delivery New Description",
            'SORT' => 200,
            'PRICE' => 399.99,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

Updating a service of type `List`:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":129,"ACTIVE":"N","CODE":"cargo_type","NAME":"Cargo Type New Name","DESCRIPTION":"Cargo Type New Description","TYPE":"enum","SORT":500,"ITEMS":[{"TITLE":"Small Package(s)","CODE":"small_package","PRICE":129.99},{"TITLE":"Documents","CODE":"documents","PRICE":69.99},{"TITLE":"Large Package(s)","CODE":"large_package","PRICE":1290.99}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.extra.service.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":129,"ACTIVE":"N","CODE":"cargo_type","NAME":"Cargo Type New Name","DESCRIPTION":"Cargo Type New Description","TYPE":"enum","SORT":500,"ITEMS":[{"TITLE":"Small Package(s)","CODE":"small_package","PRICE":129.99},{"TITLE":"Documents","CODE":"documents","PRICE":69.99},{"TITLE":"Large Package(s)","CODE":"large_package","PRICE":1290.99}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.extra.service.update
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.extra.service.update', {
            ID: 129,
            ACTIVE: "N",
            CODE: "cargo_type",
            NAME: "Cargo Type New Name",
            DESCRIPTION: "Cargo Type New Description",
            TYPE: "enum",
            SORT: 500,
            ITEMS: [{
                    TITLE: "Small Package(s)",
                    CODE: "small_package",
                    PRICE: 129.99,
                },
                {
                    TITLE: "Documents",
                    CODE: "documents",
                    PRICE: 69.99,
                },
                {
                    TITLE: "Large Package(s)",
                    CODE: "large_package",
                    PRICE: 1290.99,
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
        'sale.delivery.extra.service.update',
        [
            'ID' => 129,
            'ACTIVE' => "N",
            'CODE' => "cargo_type",
            'NAME' => "Cargo Type New Name",
            'DESCRIPTION' => "Cargo Type New Description",
            'TYPE' => "enum",
            'SORT' => 500,
            'ITEMS' => [
                [
                    'TITLE' => "Small Package(s)",
                    'CODE' => "small_package",
                    'PRICE' => 129.99,
                ],
                [
                    'TITLE' => "Documents",
                    'CODE' => "documents",
                    'PRICE' => 69.99,
                ],
                [
                    'TITLE' => "Large Package(s)",
                    'CODE' => "large_package",
                    'PRICE' => 1290.99,
                ],
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
      "start":1714549724.272976,
      "finish":1714549724.479944,
      "duration":0.20696806907653809,
      "processing":0.02615499496459961,
      "date_start":"2024-05-01T10:48:44+03:00",
      "date_finish":"2024-05-01T10:48:44+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the service update ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
   "error":"ERROR_EXTRA_SERVICE_NOT_FOUND",
   "error_description":"Extra service not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_CHECK_FAILURE` | Validation error of incoming parameters (details in the error description) | `400` || 
|| `ERROR_EXTRA_SERVICE_UPDATE` | Error when trying to update the service | `400` || 
|| `ERROR_EXTRA_SERVICE_NOT_FOUND` | Service with the specified identifier (ID) not found | `400` ||
|| `ACCESS_DENIED` | Insufficient rights to add the service | `403` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-extra-service-add.md)
- [{#T}](./sale-delivery-extra-service-get.md)
- [{#T}](./sale-delivery-extra-service-delete.md)