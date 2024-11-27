# Add Delivery Service sale.delivery.extra.service.add

> Scope: [`sale, delivery`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a delivery service.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DELIVERY_ID***
[`sale_delivery_service.ID`](../../data-types.md) | Identifier of the delivery service to which the created service will be linked.

You can obtain the identifiers of delivery services using the [sale.delivery.getlist](../delivery/sale-delivery-get-list.md) method.
||
|| **TYPE***
[`string`](../../../data-types.md) | Type of service. Possible values:
- `enum` — list (selecting an option from a pre-defined list)
- `checkbox` — single service (e.g., door delivery)
- `quantity` — quantitative service (e.g., required number of movers)
||
|| **NAME***
[`string`](../../../data-types.md) | Name of the service ||
|| **ACTIVE**
[`string`](../../../data-types.md) | Indicator of service activity. Possible values:
- `Y` — yes
- `N` — no

If the value is not provided, `N` is used by default.
||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the service ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Description of the service ||
|| **PRICE**
[`double`](../../../data-types.md) | Cost of the service in the currency of the delivery service.

This field is relevant only for services of type `single service (checkbox)` and `quantitative service (quantity)`.
||
|| **ITEMS***
[`object[]`](../../../data-types.md) | List of available options for selection (detailed description provided [below](#parameter-items)).

This field is relevant only for services of type `list (enum)`.
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
[`double`](../../../data-types.md) | Cost of the service when selecting this option in the currency of the delivery service ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Adding a service with type `Quantitative service`:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":197,"ACTIVE":"Y","CODE":"door_delivery","NAME":"Door Delivery","DESCRIPTION":"Door Delivery Description","TYPE":"checkbox","SORT":100,"PRICE":99.99}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.extra.service.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":197,"ACTIVE":"Y","CODE":"door_delivery","NAME":"Door Delivery","DESCRIPTION":"Door Delivery Description","TYPE":"checkbox","SORT":100,"PRICE":99.99,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.extra.service.add
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.extra.service.add', {
            DELIVERY_ID: 197,
            ACTIVE: "Y",
            CODE: "door_delivery",
            NAME: "Door Delivery",
            DESCRIPTION: "Door Delivery Description",
            TYPE: "checkbox",
            SORT: 100,
            PRICE: 99.99,
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

    $params = [
        'DELIVERY_ID' => 197,
        'ACTIVE' => 'Y',
        'CODE' => 'door_delivery',
        'NAME' => 'Door Delivery',
        'DESCRIPTION' => 'Door Delivery Description',
        'TYPE' => 'checkbox',
        'SORT' => 100,
        'PRICE' => 99.99
    ];

    $result = CRest::call(
        'sale.delivery.extra.service.add',
        $params
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

Adding a service with type `List`:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":198,"ACTIVE":"Y","CODE":"cargo_type","NAME":"Cargo Type","DESCRIPTION":"Cargo Type Description","TYPE":"enum","SORT":100,"ITEMS":[{"TITLE":"Small Package(s)","CODE":"small_package","PRICE":129.99},{"TITLE":"Documents","CODE":"documents","PRICE":69.99}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.extra.service.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":198,"ACTIVE":"Y","CODE":"cargo_type","NAME":"Cargo Type","DESCRIPTION":"Cargo Type Description","TYPE":"enum","SORT":100,"ITEMS":[{"TITLE":"Small Package(s)","CODE":"small_package","PRICE":129.99},{"TITLE":"Documents","CODE":"documents","PRICE":69.99}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.extra.service.add
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.extra.service.add', {
            DELIVERY_ID: 198,
            ACTIVE: "Y",
            CODE: "cargo_type",
            NAME: "Cargo Type",
            DESCRIPTION: "Cargo Type Description",
            TYPE: "enum",
            SORT: 100,
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
        'sale.delivery.extra.service.add',
        [
            'DELIVERY_ID' => 198,
            'ACTIVE' => "Y",
            'CODE' => "cargo_type",
            'NAME' => "Cargo Type",
            'DESCRIPTION' => "Cargo Type Description",
            'TYPE' => "enum",
            'SORT' => 100,
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
    "result": 128,
    "time": {
        "start": 1714204589.717545,
        "finish": 1714204589.95374,
        "duration": 0.23619484901428223,
        "processing": 0.031846046447753906,
        "date_start": "2024-04-27T10:56:29+02:00",
        "date_finish": "2024-04-27T10:56:29+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_delivery_extra_service.ID`](../../data-types.md) | Identifier of the added service ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error":"ERROR_CHECK_FAILURE",
    "error_description":"Parameter DELIVERY_ID is not defined"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_CHECK_FAILURE` | Validation error of incoming parameters (details in the error description) | `400` || 
|| `ERROR_EXTRA_SERVICE_ADD` | Error when trying to add a service | `400` || 
|| `ERROR_DELIVERY_NOT_FOUND` | Delivery service with the specified identifier not found | `400` ||
|| `ACCESS_DENIED` | Insufficient rights to add a delivery service | `403` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-extra-service-update.md)
- [{#T}](./sale-delivery-extra-service-get.md)
- [{#T}](./sale-delivery-extra-service-delete.md)