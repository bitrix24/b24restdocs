# Calculate Delivery Costs CALCULATE_URL

The request is sent to the address specified in `CALCULATE_URL` when creating a delivery handler using the method [sale.delivery.handler.add](../handler/sale-delivery-handler-add.md).

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
                        "ADM_LEVEL_1":"Moscow",
                        "ADM_LEVEL_2":"Moscow",
                        "ADM_LEVEL_3":"Yakimanka",
                        "LOCALITY":"Moscow",
                        "SUB_LOCALITY_LEVEL_1":"Central Administrative Okrug",
                        "STREET":"Shabolovka Street",
                        "BUILDING":"9",
                        "ADDRESS_LINE_1":"Shabolovka Street, 9"
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
                        "ADM_LEVEL_1":"Moscow",
                        "ADM_LEVEL_2":"Yakimanka District",
                        "LOCALITY":"Moscow",
                        "STREET":"Shabolovka Street",
                        "BUILDING":"13 b10",
                        "ADDRESS_LINE_1":"Shabolovka Street, 13 b10"
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
            "NAME":"Roman Gorshkov",
            "PHONES":[
                {
                    "TYPE":"MOBILE",
                    "VALUE":"+19097996161"
                }
            ]
        },
        "RECIPIENT_CONTACT":{
            "NAME":"Alexey Mironov",
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

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

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
|| **REASON**
[`object`](../../../data-types.md) | Reason for the error. Provided in case of an unsuccessful cost calculation attempt (detailed description provided [below](#reason)) ||
|#

### REASON

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
    "PRICE": 79.99
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

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./create-delivery-request.md)
- [{#T}](./cancel-delivery-request.md)