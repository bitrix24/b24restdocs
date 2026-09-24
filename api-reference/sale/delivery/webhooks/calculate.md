# Calculate Delivery Costs CALCULATE_URL

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Bitrix24 sends an HTTP `POST` request to the address from the `CALCULATE_URL` parameter passed when creating a delivery handler using [sale.delivery.handler.add](../handler/sale-delivery-handler-add.md). The external system must calculate the delivery cost and return the result in JSON format.

## Request Parameters

#|
|| **Name**
`type` | **Description** ||
|| **SHIPMENT**
[`object`](../../../data-types.md) | Information about the shipment (detailed description provided [below](#shipment)) ||
|#

{% include [Row of tables describing parameters](./_includes/tables.md) %}

## Example Request

```json
{
    "SHIPMENT":{
        "ID":4060,
        "DELIVERY_SERVICE":{
            "ID":225,
            "CONFIG":[
                {
                    "CODE":"PROFILE_TYPE",
                    "VALUE":"CARGO"
                }
            ],
            "PARENT":{
                "ID":223,
                "CONFIG":[
                    {
                        "CODE":"SETTING_1",
                        "VALUE":"String Example Value"
                    }
                ]
            }
        },
        "PRICE":179998,
        "CURRENCY":"USD",
        "WEIGHT":600,
        "PROPERTY_VALUES":[
            {
                "ID":100,
                "TYPE":"ADDRESS",
                "VALUE":{
                    "LATITUDE":55.726421,
                    "LONGITUDE":37.61187,
                    "FIELDS":{
                        "COUNTRY":"USA",
                        "ADM_LEVEL_1":"Los Angeles",
                        "ADM_LEVEL_2":"Los Angeles",
                        "ADM_LEVEL_3":"South",
                        "LOCALITY":"Los Angeles",
                        "SUB_LOCALITY_LEVEL_1":"Central",
                        "STREET":"Flowers Street",
                        "BUILDING":"9",
                        "ADDRESS_LINE_1":"Flowers Street, 9"
                    }
                }
            },
            {
                "ID":101,
                "TYPE":"ADDRESS",
                "VALUE":{
                    "LATITUDE":55.724779,
                    "LONGITUDE":37.614294,
                    "FIELDS":{
                        "POSTAL_CODE":"115162",
                        "COUNTRY":"USA",
                        "ADM_LEVEL_1":"Los Angeles",
                        "ADM_LEVEL_2":"South",
                        "LOCALITY":"Los Angeles",
                        "STREET":"Flowers Street",
                        "BUILDING":"13 b10",
                        "ADDRESS_LINE_1":"Flowers Street, 13 b10"
                    }
                }
            }
        ],
        "ITEMS":[
            {
                "NAME":"iPhone 14",
                "PRICE":89999,
                "WEIGHT":300,
                "CURRENCY":"USD",
                "QUANTITY":2,
                "DIMENSIONS":{
                    "WIDTH":400,
                    "HEIGHT":80,
                    "LENGTH":500
                }
            }
        ],
        "EXTRA_SERVICES_VALUES":[
            {
                "ID":138,
                "CODE":"cargo_type",
                "VALUE":"small_package"
            },
            {
                "ID":137,
                "CODE":"door_delivery",
                "VALUE":"Y"
            },
            {
                "ID":139,
                "CODE":"some_quantity_service",
                "VALUE":3
            }
        ],
        "RESPONSIBLE_CONTACT":{
            "NAME":"Ronald Perez",
            "PHONES":[
                {
                    "TYPE":"MOBILE",
                    "VALUE":"+19097996161"
                }
            ]
        },
        "RECIPIENT_CONTACT":{
            "NAME":"James Johnson",
            "PHONES":[
                {
                    "TYPE":"WORK",
                    "VALUE":"+19097996161"
                }
            ]
        }
    }
}
```

## Response Parameters

The handler must return HTTP status `200` and a JSON object.

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SUCCESS***
[`string`](../../../data-types.md) | Indicator of the success of the delivery cost calculation. Possible values:

- `Y` — cost calculated successfully
- `N` — an error occurred while attempting to calculate the cost
 ||
|| **PRICE**
[`double`](../../../data-types.md) | Calculated delivery cost in the currency of the delivery service ||
|| **PERIOD_DESCRIPTION**
[`string`](../../../data-types.md) | Text description of the delivery period ||
|| **PERIOD_FROM**
[`integer`](../../../data-types.md) | Lower bound of the delivery period in the units specified in `PERIOD_TYPE` ||
|| **PERIOD_TO**
[`integer`](../../../data-types.md) | Upper bound of the delivery period in the units specified in `PERIOD_TYPE` ||
|| **PERIOD_TYPE**
[`string`](../../../data-types.md) | Unit of measurement for the delivery period. Possible values:

- `MIN` — minutes
- `H` — hours
- `D` — days
- `M` — months
 ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Additional description of the calculation result ||
|| **REASON**
[`object`](../../../data-types.md) | Reason for the error. Provided in case of an unsuccessful cost calculation attempt (detailed description provided [below](#reason)) ||
|#

### REASON Object {#reason}

#|
|| **Name**
`type` | **Description** ||
|| **TEXT***
[`string`](../../../data-types.md) | Description of the error ||
|#

## Example Response with Successful Cost Calculation

```json
{
    "SUCCESS": "Y",
    "PRICE": 79.99,
    "PERIOD_DESCRIPTION": "1–2 days",
    "PERIOD_FROM": 1,
    "PERIOD_TO": 2,
    "PERIOD_TYPE": "D",
    "DESCRIPTION": "Door-to-door courier delivery"
}
```

## Example Response with Error in Cost Calculation

```json
{
    "SUCCESS": "N",
    "REASON": {
        "TEXT": "Delivery is not available for the specified address"
    }
}
```

## Error Handling

If `SUCCESS` is absent or differs from `Y`, Bitrix24 considers the calculation unsuccessful. Provide an explanation in `REASON.TEXT`. If `REASON.TEXT` is absent or empty, Bitrix24 uses the standard delivery calculation error message.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./create-delivery-request.md)
- [{#T}](./cancel-delivery-request.md)
