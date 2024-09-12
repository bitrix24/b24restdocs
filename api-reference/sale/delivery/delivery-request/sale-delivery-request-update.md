# Update Transportation Request sale.delivery.request.update

> Scope: [`sale, delivery`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the transportation request.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DELIVERY_ID***
[`sale_delivery_service.ID`](../../data-types.md) | Identifier of the delivery service related to the transportation request.

You can obtain the identifiers `sale_delivery_service.ID` of delivery services using the method [sale.delivery.getlist](../delivery/sale-delivery-get-list.md)
||
|| **REQUEST_ID***
[`string`](../../../data-types.md) | Identifier of the transportation request.

The identifier is assigned by the external system in response to the webhook for creating a delivery order (more details in the webhook description [Creating a Delivery Order](../webhooks/create-delivery-request.md))
||
|| **FINALIZE**
[`string`](../../../data-types.md) | Indicator of the need to complete (finalize) the transportation request.

It is implied that the indicator value should be set to `Y` when the transportation request is fulfilled. 

By default, if no value is provided, the request is not finalized.

Possible values:
- `Y` — yes
- `N` — no
||
|| **STATUS**
[`object`](../../../data-types.md) | Status of the transportation request (detailed description provided [below](#parametr-status)) ||
|| **PROPERTIES**
[`object[]`](../../../data-types.md) | Properties of the transportation request (detailed description provided [below](#parametr-properties)) ||
|| **OVERWRITE_PROPERTIES**
[`string`](../../../data-types.md) | Indicator of the need to completely overwrite the property values of the request during the update. 

By default, properties are only added during the update (equivalent to passing the value `N`). If the calling party needs to pass the full set of properties and overwrite existing properties, the value of this indicator must be set to `Y`.

Possible values:
- `Y` — yes
- `N` — no
||
|#

### Parameter STATUS {#parametr-status}

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TEXT***
[`string`](../../../data-types.md) | Text name of the transportation request status ||
|| **SEMANTIC***
[`string`](../../../data-types.md) | Value of the status semantics.

Possible values:
- `process` — request is in progress
- `success` — request completed successfully
||
|#

### Parameter PROPERTIES {#parametr-properties}

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the property ||
|| **VALUE***
[`string`](../../../data-types.md) | Value of the property ||
|| **TAGS**
[`string[]`](../../../data-types.md) | List of tags.

Possible values:
- `phone` — the provided value will be displayed as a phone number
||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":225,"REQUEST_ID":"4757aca4931a4f029f49c0db4374d13d","STATUS":{"TEXT":"Performer found","SEMANTIC":"process"},"PROPERTIES":[{"NAME":"Car","VALUE":"Gray Skoda Octavia, a777zn"},{"NAME":"Driver","VALUE":"John Smith"},{"NAME":"Phone Number","VALUE":"+11111111111","TAGS":["phone"]},{"NAME":"Something else","VALUE":"Some value"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.request.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":225,"REQUEST_ID":"4757aca4931a4f029f49c0db4374d13d","STATUS":{"TEXT":"Performer found","SEMANTIC":"process"},"PROPERTIES":[{"NAME":"Car","VALUE":"Gray Skoda Octavia, a777zn"},{"NAME":"Driver","VALUE":"John Smith"},{"NAME":"Phone Number","VALUE":"+11111111111","TAGS":["phone"]},{"NAME":"Something else","VALUE":"Some value"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.request.update
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.request.update', {
            DELIVERY_ID: 225,
            REQUEST_ID: "4757aca4931a4f029f49c0db4374d13d",
            STATUS: {
                TEXT: "Performer found",
                SEMANTIC: "process",
            },
            PROPERTIES: [{
                    NAME: "Car",
                    VALUE: "Gray Skoda Octavia, a777zn",
                },
                {
                    NAME: "Driver",
                    VALUE: "John Smith",
                },
                {
                    NAME: "Phone Number",
                    VALUE: "+11111111111",
                    TAGS: [
                        "phone"
                    ],
                },
                {
                    NAME: "Something else",
                    VALUE: "Some value",
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
        'sale.delivery.request.update',
        [
            'DELIVERY_ID' => 225,
            'REQUEST_ID' => "4757aca4931a4f029f49c0db4374d13d",
            'STATUS' => [
                'TEXT' => "Performer found",
                'SEMANTIC' => "process",
            ],
            'PROPERTIES' => [
                [
                    'NAME' => "Car",
                    'VALUE' => "Gray Skoda Octavia, a777zn",
                ],
                [
                    'NAME' => "Driver",
                    'VALUE' => "John Smith",
                ],
                [
                    'NAME' => "Phone Number",
                    'VALUE' => "+11111111111",
                    'TAGS' => [
                        "phone"
                    ],
                ],
                [
                    'NAME' => "Something else",
                    'VALUE' => "Some value",
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

HTTP Status: **200**

```json
{
    "result":true,
    "time":{
        "start":1714557963.841951,
        "finish":1714557964.052347,
        "duration":0.21039605140686035,
        "processing":0.04059791564941406,
        "date_start":"2024-05-01T13:06:03+03:00",
        "date_finish":"2024-05-01T13:06:04+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the transportation request update ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error":"DELIVERY_NOT_FOUND",
    "error_description":"Delivery service has not been found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `DELIVERY_ID_NOT_SPECIFIED` | Delivery service identifier not specified | `400` || 
|| `DELIVERY_NOT_FOUND` | Delivery service not found | `400` || 
|| `REQUEST_ID_NOT_SPECIFIED` | Transportation request identifier not specified | `400` ||
|| `REQUEST_NOT_FOUND` | Transportation request not found | `400` ||
|| `STATUS_UNEXPECTED_FORMAT` | Incorrect format of the `STATUS` parameter value | `400` ||
|| `STATUS_TEXT_NOT_SPECIFIED` | Status name value not specified | `400` ||
|| `STATUS_SEMANTIC_NOT_SPECIFIED` | Status semantics value not specified | `400` ||
|| `PROPERTIES_UNEXPECTED_FORMAT` | Incorrect format of the `PROPERTIES` parameter value | `400` ||
|| `PROPERTY_VALUE_UNEXPECTED_FORMAT` | Incorrect format of one of the provided properties | `400` ||
|| `PROPERTY_VALUE_TAGS_UNEXPECTED_FORMAT` | Incorrect format of the property tags value | `400` ||
|| `PROPERTY_VALUE_TAG_UNEXPECTED_FORMAT` | Incorrect format of the property tag value | `400` ||
|| `UNEXPECTED_REQUEST_FINALIZE_INDICATOR_VALUE` | Incorrect value of the `FINALIZE` parameter.

Allowed values: `Y`, `N`
 | `400` ||
|| `UNEXPECTED_OVERWRITE_PROPERTIES_VALUE` | Incorrect value of the `OVERWRITE_PROPERTIES` parameter.

Allowed values: `Y`, `N`
 | `400` ||
|| `UPDATE_REQUEST_INTERNAL_ERROR` | Error while attempting to update the transportation request
 | `400` ||
 || `EMPTY_UPDATE_PAYLOAD` | Empty set of fields for updating the transportation request
 | `400` ||
|| `ACCESS_DENIED` | Insufficient permissions to add the delivery service | `403` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-request-send-message.md)
- [{#T}](./sale-delivery-request-delete.md)