# Set Up Delivery for Use in CRM

## Creating a Delivery Service Handler

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.handler.add',
        {
            CODE: "uber",
            NAME: "Uber",
            SETTINGS: {
                CALCULATE_URL: "https://gateway.bx/calculate.php",
                CREATE_DELIVERY_REQUEST_URL: "https://gateway.bx/create_delivery_request.php",
                CANCEL_DELIVERY_REQUEST_URL: "https://gateway.bx/cancel_delivery_request.php",
                HAS_CALLBACK_TRACKING_SUPPORT: "Y",
                CONFIG: [
                    {
                        TYPE: "STRING",
                        CODE: "MY_FIRST_SETTING",
                        NAME: "My first setting",
                    },
                    {
                        TYPE: "STRING",
                        CODE: "MY_SECOND_SETTING",
                        NAME: "My second setting",
                    },
                ],
            },
            PROFILES: [
                {
                    NAME: "Taxi",
                    CODE: "TAXI",
                    DESCRIPTION: "Taxi Delivery",
                },
                {
                    NAME: "Cargo",
                    CODE: "CARGO",
                    DESCRIPTION: "Cargo Delivery",
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
        'sale.delivery.handler.add',
        [
            'CODE' => 'uber',
            'NAME' => 'Uber',
            'SETTINGS' => [
                'CALCULATE_URL' => 'https://gateway.bx/calculate.php',
                'CREATE_DELIVERY_REQUEST_URL' => 'https://gateway.bx/create_delivery_request.php',
                'CANCEL_DELIVERY_REQUEST_URL' => 'https://gateway.bx/cancel_delivery_request.php',
                'HAS_CALLBACK_TRACKING_SUPPORT' => 'Y',
                'CONFIG' => [
                    [
                        'TYPE' => 'STRING',
                        'CODE' => 'MY_FIRST_SETTING',
                        'NAME' => 'My first setting',
                    ],
                    [
                        'TYPE' => 'STRING',
                        'CODE' => 'MY_SECOND_SETTING',
                        'NAME' => 'My second setting',
                    ],
                ],
            ],
            'PROFILES' => [
                [
                    'NAME' => 'Taxi',
                    'CODE' => 'TAXI',
                    'DESCRIPTION' => 'Taxi Delivery',
                ],
                [
                    'NAME' => 'Cargo',
                    'CODE' => 'CARGO',
                    'DESCRIPTION' => 'Cargo Delivery',
                ],
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

Example response:

```json
{
    "result": 23,
    "time": {
        "start": 1714736790.260814,
        "finish": 1714736791.896773,
        "duration": 1.6359591484069824,
        "processing": 0.03880000114440918,
        "date_start": "2024-05-03T14:46:30+02:00",
        "date_finish": "2024-05-03T14:46:31+02:00"
    }
}
```

We created a delivery service handler with the identifier `23`. The delivery service handler is a template that we will use to create specific delivery services later.

More about the method [sale.delivery.handler.add](../../api-reference/sale/delivery/handler/sale-delivery-handler-add.md).

## Creating a Delivery Service

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.add',
        {
            REST_CODE: "uber",
            NAME: "Uber Taxi",
            CURRENCY: "USD",
            ACTIVE: "Y",
            CONFIG: [
                {
                    CODE: "MY_FIRST_SETTING",
                    VALUE: "My first setting value",
                },
                {
                    CODE: "MY_SECOND_SETTING",
                    VALUE: "My second setting value",
                },
            ]
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
        'sale.delivery.add',
        [
            'FIELDS' => [
                'REST_CODE' => 'uber',
                'NAME' => 'Uber Taxi',
                'CURRENCY' => 'USD',
                'ACTIVE' => 'Y',
                'CONFIG' => [
                    [
                        'CODE' => 'MY_FIRST_SETTING',
                        'VALUE' => 'My first setting value',
                    ],
                    [
                        'CODE' => 'MY_SECOND_SETTING',
                        'VALUE' => 'My second setting value',
                    ],
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

Example response:

```js
{
"result":{
    "parent":{
        "NAME":"Uber Taxi",
        "ACTIVE":"Y",
        "DESCRIPTION":"",
        "CURRENCY":"USD",
        "ID":226,
        "PARENT_ID":null,
        "SORT":100,
        "LOGOTYPE":null
    },
    "profiles":[
        {
            "NAME":"Taxi",
            "ACTIVE":"Y",
            "DESCRIPTION":"Taxi Delivery",
            "CURRENCY":"USD",
            "ID":227,
            "PARENT_ID":226,
            "SORT":100,
            "LOGOTYPE":null
        },
        {
            "NAME":"Cargo",
            "ACTIVE":"Y",
            "DESCRIPTION":"Cargo Delivery",
            "CURRENCY":"USD",
            "ID":228,
            "PARENT_ID":226,
            "SORT":100,
            "LOGOTYPE":null
        }
    ]
},
"time":{
    "start":1714737122.600765,
    "finish":1714737122.894801,
    "duration":0.2940359115600586,
    "processing":0.0942530632019043,
    "date_start":"2024-05-03T14:52:02+02:00",
    "date_finish":"2024-05-03T14:52:02+02:00"
}
}
```

We created a delivery service with the identifier `226`. This delivery service has two child delivery services, delivery profiles, with identifiers `227` and `228`.

More about the method [sale.delivery.add](../../api-reference/sale/delivery/delivery/sale-delivery-add.md).

## Creating Shipment Properties

To allow the manager to select "From" and "To" addresses when choosing a delivery service in the interface, we need to create the corresponding shipment properties. If such properties have been created before, you can proceed directly to the next step of linking shipment properties to the delivery service.

Let's create the property `Address From`. 

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'sale.shipmentproperty.add', {
            fields: {
                personTypeId: 3,
                propsGroupId: 6,
                name: "Address From",
                active: "Y",
                sort: "100",
                type: "ADDRESS",
                required: "Y",
                isAddressFrom: "Y"
            }
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
        'sale.shipmentproperty.add',
        [
            'fields' => [
                'personTypeId' => 3,
                'propsGroupId' => 6,
                'name' => 'Address From',
                'active' => 'Y',
                'sort' => '100',
                'type' => 'ADDRESS',
                'required' => 'Y',
                'isAddressFrom' => 'Y'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

Response:

```json
{
"result":{
    "property":{
        "active":"Y",
        "code":"",
        "defaultValue":"",
        "description":"",
        "id":102,
        "inputFieldLocation":"0",
        "isAddress":"N",
        "isAddressFrom":"Y",
        "isAddressTo":"N",
        "isEmail":"N",
        "isFiltered":"N",
        "isLocation":"N",
        "isLocation4tax":"N",
        "isPayer":"N",
        "isPhone":"N",
        "isProfileName":"N",
        "isZip":"N",
        "multiple":"N",
        "name":"Address From",
        "personTypeId":3,
        "propsGroupId":6,
        "required":"Y",
        "settings":[
            
        ],
        "sort":100,
        "type":"ADDRESS",
        "userProps":"N",
        "util":"N",
        "xmlId":"bx_6634d350b3f83"
    }
},
"time":{
    "start":1714737999.671308,
    "finish":1714738000.799885,
    "duration":1.1285769939422607,
    "processing":0.8807950019836426,
    "date_start":"2024-05-03T15:06:39+02:00",
    "date_finish":"2024-05-03T15:06:40+02:00"
}
}
```

Now let's create the property `Address To`. 

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'sale.shipmentproperty.add', {
            fields: {
                personTypeId: 3,
                propsGroupId: 6,
                name: "Address To",
                active: "Y",
                sort: "100",
                type: "ADDRESS",
                required: "Y",
                isAddressTo: "Y"
            }
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
        'sale.shipmentproperty.add',
        [
            'fields' => [
                'personTypeId' => 3,
                'propsGroupId' => 6,
                'name' => 'Address To',
                'active' => 'Y',
                'sort' => '100',
                'type' => 'ADDRESS',
                'required' => 'Y',
                'isAddressTo' => 'Y'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

Response:

```json
{
"result":{
    "property":{
        "active":"Y",
        "code":"",
        "defaultValue":"",
        "description":"",
        "id":103,
        "inputFieldLocation":"0",
        "isAddress":"N",
        "isAddressFrom":"N",
        "isAddressTo":"Y",
        "isEmail":"N",
        "isFiltered":"N",
        "isLocation":"N",
        "isLocation4tax":"N",
        "isPayer":"N",
        "isPhone":"N",
        "isProfileName":"N",
        "isZip":"N",
        "multiple":"N",
        "name":"Address To",
        "personTypeId":3,
        "propsGroupId":6,
        "required":"Y",
        "settings":[
            
        ],
        "sort":100,
        "type":"ADDRESS",
        "userProps":"N",
        "util":"N",
        "xmlId":"bx_6634d380a68e5"
    }
},
"time":{
    "start":1714738048.347586,
    "finish":1714738048.713083,
    "duration":0.3654971122741699,
    "processing":0.18678808212280273,
    "date_start":"2024-05-03T15:07:28+02:00",
    "date_finish":"2024-05-03T15:07:28+02:00"
}
}
```

We created two shipment properties: `Address From` with the identifier `102` and `Address To` with the identifier `103`.

Similarly, you can create properties of other types for specific needs. For example, if you want the manager to be able to write a comment for the transport company, you can create a shipment property with the type `STRING` and name it accordingly "Comment for Transport Company". In this case, the property will be displayed to the manager in the user interface, and he will be able to fill it out.

More about the method [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md).

## Linking Shipment Properties to the Delivery Service

After creating the properties, you need to link them to the previously created delivery services. By now, we have two delivery services with identifiers `227` and `228` and two properties with identifiers `102` and `103`. There is no need to link properties to the parent delivery service.

Let's link the properties to the delivery service with the identifier `227`. 

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'sale.propertyrelation.add',
        {
            fields: {
                entityId: 227,
                entityType: 'D',
                propertyId: 102
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );

    BX24.callMethod(
        'sale.propertyrelation.add', {
            fields: {
                entityId: 227,
                entityType: 'D',
                propertyId: 103
            }
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
        'sale.propertyrelation.add',
        [
            'fields' => [
                'entityId' => 227,
                'entityType' => 'D',
                'propertyId' => 102
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';

        $result = CRest::call(
        'sale.propertyrelation.add',
        [
            'fields' => [
                'entityId' => 227,
                'entityType' => 'D',
                'propertyId' => 103
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

Now let's link the properties to the delivery service with the identifier `228`. 

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'sale.propertyrelation.add', {
            fields: {
                entityId: 228,
                entityType: 'D',
                propertyId: 102
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );

    BX24.callMethod(
        'sale.propertyrelation.add', {
            fields: {
                entityId: 228,
                entityType: 'D',
                propertyId: 103
            }
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
        'sale.propertyrelation.add',
        [
            'fields' => [
                'entityId' => 228,
                'entityType' => 'D',
                'propertyId' => 102
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';

        $result = CRest::call(
        'sale.propertyrelation.add',
        [
            'fields' => [
                'entityId' => 228,
                'entityType' => 'D',
                'propertyId' => 103
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

More about the method [sale.propertyrelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md).

## Adding Services to Delivery Services

In some cases, it is necessary to give the manager the ability to choose certain services that may be available for the delivery service. For example, the cost of delivery may differ if door delivery is required. In this case, you can create a service that the manager can specify when calculating or placing an order for delivery.

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.extra.service.add', {
            DELIVERY_ID: 227,
            ACTIVE: "Y",
            CODE: "door_delivery",
            NAME: "Door Delivery",
            TYPE: "checkbox",
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
            'DELIVERY_ID' => 227,
            'ACTIVE' => 'Y',
            'CODE' => 'door_delivery',
            'NAME' => 'Door Delivery',
            'TYPE' => 'checkbox',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

Response:

```js
{
    "result": 140,
    "time": {
        "start": 1714739042.228152,
        "finish": 1714739042.50093,
        "duration": 0.2727780342102051,
        "processing": 0.09131193161010742,
        "date_start": "2024-05-03T15:24:02+02:00",
        "date_finish": "2024-05-03T15:24:02+02:00"
    }
}
```

In this example, we created a delivery service to the door with the symbolic code `door_delivery` and identifier `140`.

More about the method [sale.delivery.extra.service.add](../../api-reference/sale/delivery/extra-service/sale-delivery-extra-service-add.md).

## Handling Requests Coming to Webhooks

By this point, we have made all the necessary settings on the *Bitrix24* side, and the manager can fully utilize the delivery service we created. He can perform a preliminary cost calculation, place an order for delivery, and also have the option to cancel the delivery. When creating the handler, we specified specific URLs for this in the parameters.

#|
|| **Purpose** | **Parameter** | **URL** ||
|| Preliminary calculation of delivery cost | `CALCULATE_URL` | https://gateway.bx/calculate.php ||
|| Request to place a real delivery order | `CREATE_DELIVERY_REQUEST_URL` | https://gateway.bx/create_delivery_request.php ||
|| Request to cancel a previously formed delivery order | `CANCEL_DELIVERY_REQUEST_URL` | https://gateway.bx/cancel_delivery_request.php ||
|#

When requests come to these URLs, the external system must process the request and return a response in the required format.

More about webhooks in the section [Webhooks for Delivery Operations](../../api-reference/sale/delivery/webhooks/index.md).

## Notifications During Delivery Order Execution

During the execution of a delivery order, the external system may need to inform the manager or recipient about some information regarding the order status. For this, there are methods in the [sale.delivery.request.*](../../api-reference/sale/delivery/delivery-request/index.md) family.

#|
|| **Method** | **Purpose** ||
|| [sale.delivery.request.update](../../api-reference/sale/delivery/delivery-request/sale-delivery-request-update.md) | Updates the delivery order entity: status and set of its properties ||

|| [sale.delivery.request.delete](../../api-reference/sale/delivery/delivery-request/sale-delivery-request-delete.md) | Notifies about the cancellation of the delivery order on the external system side and attempts to cancel the delivery order on the Bitrix24 side ||

|| [sale.delivery.request.sendmessage](../../api-reference/sale/delivery/delivery-request/sale-delivery-request-send-message.md) | Sends a message to the manager or recipient about the current status of the delivery order ||
|#