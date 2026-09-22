# Create Delivery Request CREATE_DELIVERY_REQUEST_URL

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Bitrix24 sends the request to the address from the `CREATE_DELIVERY_REQUEST_URL` parameter passed when creating a delivery handler using [sale.delivery.handler.add](../handler/sale-delivery-handler-add.md). The external system must create a delivery order and return its identifier.

## Request Parameters

#|
|| **Name**
`type` | **Description** ||
|| **SHIPMENTS**
[`object[]`](../../../data-types.md) | Information about shipments (detailed description provided [below](#shipment)) ||
|#

{% include [Row of tables describing parameters](./_includes/tables.md) %}

## Examples

Example JSON request:

```json
{
    "SHIPMENTS":[
        {
            "ID":4063,
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
                    "VALUE":2
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
                        "VALUE":"+19097996162"
                    }
                ]
            }
        }
    ]
}
```

## Response Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SUCCESS***
[`string`](../../../data-types.md) | Result of creating a delivery order. Possible values:

- `Y` — delivery order created
- `N` — delivery order not created
 ||
|| **REQUEST_ID***
[`string`](../../../data-types.md) | Identifier of the created delivery order in the external system. Required if `SUCCESS` = `Y` ||
|| **REASON**
[`object`](../../../data-types.md) | Reason why the delivery order was not created. Passed if `SUCCESS` = `N` [(detailed description)](#reason) ||
|#

### REASON Object {#reason}

#|
|| **Name**
`type` | **Description** ||
|| **TEXT***
[`string`](../../../data-types.md) | Description of the error ||
|#

## Example Response with Successful Delivery Request Creation

```json
{
    "SUCCESS": "Y",
    "REQUEST_ID": "4757aca4931a4f029f49c0db4374d13d"
}
```

## Example Response with Delivery Order Creation Error

```json
{
    "SUCCESS": "N",
    "REASON": {
        "TEXT": "Delivery is not available for the specified address"
    }
}
```

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calculate.md)
- [{#T}](./cancel-delivery-request.md)
