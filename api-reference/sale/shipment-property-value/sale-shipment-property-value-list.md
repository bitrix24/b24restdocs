# Get a List of Shipment Property Values sale.shipmentpropertyvalue.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method returns a list of shipment property values.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to select (see fields of the object [sale_shipment_property_value](../data-types.md#sale_shipment_property_value)).

If the array is not provided or an empty array is passed, all available property value fields will be selected.
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected property values in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [sale_shipment_property_value](../data-types.md#sale_shipment_property_value).

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search is done from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected property values in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [sale_shipment_property_value](../data-types.md#sale_shipment_property_value).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for pagination control.

The page size of results is always static: 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

## Code Examples

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["code","id","name","shipmentId","shipmentPropsId","shipmentPropsXmlId","value"],"filter":{"@shipmentId":[4120]},"order":{"shipmentId":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentpropertyvalue.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["code","id","name","shipmentId","shipmentPropsId","shipmentPropsXmlId","value"],"filter":{"@shipmentId":[4120]},"order":{"shipmentId":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentpropertyvalue.list
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.shipmentpropertyvalue.list", {
            "select": [
                "code",
                "id",
                "name",
                "shipmentId",
                "shipmentPropsId",
                "shipmentPropsXmlId",
                "value",
            ],
            "filter": {
                "@shipmentId": [4120],
            },
            "order": {
                "shipmentId": "desc",
            },
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
        'sale.shipmentpropertyvalue.list',
        [
            'select' => [
                "code",
                "id",
                "name",
                "shipmentId",
                "shipmentPropsId",
                "shipmentPropsXmlId",
                "value",
            ],
            'filter' => [
                "@shipmentId" => [4120],
            ],
            'order' => [
                "shipmentId" => "desc",
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP Status: **200**

```json
{
    "result":{
        "propertyValues":[
            {
                "code":null,
                "id":38164,
                "name":"Comments",
                "shipmentId":4120,
                "shipmentPropsId":105,
                "shipmentPropsXmlId":"bx_6661d1b6690d5",
                "value":"Comments value"
            },
            {
                "code":null,
                "id":38166,
                "name":"Description",
                "shipmentId":4120,
                "shipmentPropsId":106,
                "shipmentPropsXmlId":"bx_6666cc212db52",
                "value":"Description value"
            }
        ]
    },
    "total":2,
    "time":{
        "start":1718023945.124614,
        "finish":1718023945.312019,
        "duration":0.18740510940551758,
        "processing":0.016345977783203125,
        "date_start":"2024-06-10T15:52:25+03:00",
        "date_finish":"2024-06-10T15:52:25+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **propertyValues**
[`sale_shipment_property_value[]`](../data-types.md#sale_shipment_property_value) | An array of objects containing information about the selected shipment property values ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":0,
    "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read property values ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-shipment-property-value-modify.md)
- [{#T}](./sale-shipment-property-value-get.md)
- [{#T}](./sale-shipment-propertyvalue-delete.md)
- [{#T}](./sale-shipment-property-value-get-fields.md)