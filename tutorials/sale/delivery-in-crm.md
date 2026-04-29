# Setting Up a Delivery Service for CRM

> Scope: [`sale`](../../api-reference/scopes/permissions.md)
>
> Who can execute methods: administrator

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can connect external delivery services to Bitrix24. This allows managers to work with the delivery service within CRM cards: calculating costs and tracking status.

To set up the delivery service, we will sequentially execute the following methods:

1. [sale.delivery.handler.add](../../api-reference/sale/delivery/handler/sale-delivery-handler-add.md) — register the delivery handler,

2. [sale.delivery.add](../../api-reference/sale/delivery/delivery/sale-delivery-add.md) — create the parent service and profiles linked to the handler,

3. [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md) — add shipment properties for addresses,

4. [sale.propertyrelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md) — link properties to delivery profiles.

5. [sale.delivery.extra.service.add](../../api-reference/sale/delivery/extra-service/sale-delivery-extra-service-add.md) — connect additional services.

## 1. Create a Delivery Handler

We will register the handler using [sale.delivery.handler.add](../../api-reference/sale/delivery/handler/sale-delivery-handler-add.md). We will pass four parameters to the method.

- `CODE` — the symbolic code of the delivery handler. For example, we will specify `uber`.

- `NAME` — the name of the delivery handler. We will pass `Uber`.

- `SETTINGS` — an object containing information about the handler's settings.

    - `CALCULATE_URL` — the URL for calculating delivery costs, for example, `https://gateway.bx/calculate.php`.

    - `CREATE_DELIVERY_REQUEST_URL` — the URL for creating a delivery request. We will specify `https://gateway.bx/create_delivery_request.php`.

    - `CANCEL_DELIVERY_REQUEST_URL` — the URL for canceling a delivery, for example, `https://gateway.bx/cancel_delivery_request.php`.

    - `HAS_CALLBACK_TRACKING_SUPPORT` — an indicator of whether the service will send notifications. We will set it to `Y`. Notifications can be created using [sale.delivery.request.sendmessage](../../api-reference/sale/delivery/delivery-request/sale-delivery-request-send-message.md).

    - `CONFIG` — a list of settings. We will specify `MY_FIRST_SETTING` and `MY_SECOND_SETTING` with the type `STRING`.

- `PROFILES` — an array of delivery profiles. The handler must have at least one profile. We will set `Taxi` and `Cargo`.

The delivery service at the specified URLs must accept the request, process it, and return a response in the format expected by the CRM.

For more details on the request and response formats, refer to the section [Webhooks for Delivery Operations](../../api-reference/sale/delivery/webhooks/index.md).

{% include [Examples Note](../../_includes/examples.md) %}

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
                    },
                ],
            },
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

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            auth_token="your-webhook-token",
        )
    )

    try:
        response = client.sale.delivery.handler.add(
        code="uber",
        name="Uber",
        settings={
            "CALCULATE_URL": "https://gateway.bx/calculate.php",
            "CREATE_DELIVERY_REQUEST_URL": "https://gateway.bx/create_delivery_request.php",
            "CANCEL_DELIVERY_REQUEST_URL": "https://gateway.bx/cancel_delivery_request.php",
            "HAS_CALLBACK_TRACKING_SUPPORT": "Y",
            "CONFIG": [
                {
                    "TYPE": "STRING",
                    "CODE": "MY_FIRST_SETTING",
                    "NAME": "My first setting",
                },
                {
                    "TYPE": "STRING",
                    "CODE": "MY_SECOND_SETTING",
                    "NAME": "My second setting",
                },
            ],
        },
        profiles=[
            {
                "NAME": "Taxi",
                "CODE": "TAXI",
                "DESCRIPTION": "Taxi Delivery",
            },
            {
                "NAME": "Cargo",
                "CODE": "CARGO",
                "DESCRIPTION": "Cargo Delivery",
            },
        ],
        ).response
        print(response.result)
    except BitrixAPIError as error:
        print(error)
    ```

{% endlist %}

If the handler is successfully added, the method will return its identifier. If you receive an `error`, refer to the documentation for possible errors in the method [sale.delivery.handler.add](../../api-reference/sale/delivery/handler/sale-delivery-handler-add.md).

```json
{
    "result": 23,
    "time": {
        "start": 1714736790.260814,
        "finish": 1714736791.896773,
        "duration": 1.6359591484069824,
        "processing": 0.03880000114440918,
        "date_start": "2024-05-03T14:46:30+03:00",
        "date_finish": "2024-05-03T14:46:31+03:00"
    }
}
```

## 2. Create a Delivery Service {#second}

We will create a delivery service using the method [sale.delivery.add](../../api-reference/sale/delivery/delivery/sale-delivery-add.md). We will pass the following parameters to the method:

- `REST_CODE` — the symbolic code of the delivery handler. We will specify `uber`, which we set in the first step.

- `NAME` — the name of the delivery service, for example, `Uber Taxi`.

- `CURRENCY` — the symbolic code of the currency. We will pass `USD`. You can get a list of currencies using the method [crm.currency.list](../../api-reference/crm/currency/crm-currency-list.md).

- `ACTIVE` — the flag indicating whether the delivery service is active. We will set it to `Y`.

- `CONFIG` — the values of the handler's settings. We will pass the values for `MY_FIRST_SETTING` and `MY_SECOND_SETTING` that we set in the first step.

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
            'REST_CODE' => 'uber',
            'NAME' => 'Uber Taxi',
            'CURRENCY' => 'USD',
            'ACTIVE' => 'Y',
            'CONFIG' => [
                [
                    'CODE' => 'MY_FIRST_SETTING',
                    'VALUE' => 'My first setting value',
                },
                [
                    'CODE' => 'MY_SECOND_SETTING',
                    'VALUE' => 'My second setting value',
                },
            ]
        ]
    );
    
    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    ```python
    try:
        response = client.sale.delivery.add(
        rest_code="uber",
        name="Uber Taxi",
        currency="USD",
        active=True,
        config=[
            {
                "CODE": "MY_FIRST_SETTING",
                "VALUE": "My first setting value",
            },
            {
                "CODE": "MY_SECOND_SETTING",
                "VALUE": "My second setting value",
            },
        ],
        ).response
        print(response.result)
    except BitrixAPIError as error:
        print(error)
    ```

{% endlist %}

If the delivery service is successfully created, the method will return the parent service object and an array of profiles. If you receive an `error`, refer to the documentation for possible errors in the method [sale.delivery.add](../../api-reference/sale/delivery/delivery/sale-delivery-add.md).

```json
{
"result": {
    "parent": {
        "NAME": "Uber Taxi",
        "ACTIVE": "Y",
        "DESCRIPTION": "",
        "CURRENCY": "USD",
        "ID": 226,
        "PARENT_ID": null,
        "SORT": 100,
        "LOGOTYPE": null
    },
    "profiles": [
        {
            "NAME": "Taxi",
            "ACTIVE": "Y",
            "DESCRIPTION": "Taxi Delivery",
            "CURRENCY": "USD",
            "ID": 227,
            "PARENT_ID": 226,
            "SORT": 100,
            "LOGOTYPE": null
        },
        {
            "NAME": "Cargo",
            "ACTIVE": "Y",
            "DESCRIPTION": "Cargo Delivery",
            "CURRENCY": "USD",
            "ID": 228,
            "PARENT_ID": 226,
            "SORT": 100,
            "LOGOTYPE": null
        }
    ]
},
"time": {
    "start": 1714737122.600765,
    "finish": 1714737122.894801,
    "duration": 0.2940359115600586,
    "processing": 0.0942530632019043,
    "date_start": "2024-05-03T14:52:02+03:00",
    "date_finish": "2024-05-03T14:52:02+03:00"
}
}
```

## 3. Create Shipment Properties {#third}

In the shipment, the manager specifies the shipping address and the delivery address. We will sequentially create two properties, `Address From` and `Address To`, using the method [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md).

### Property Address From

We will pass an object `fields` with the values for the `Address From` property.

- `personTypeId` — the identifier of the payer type. We will pass `3`. You can get a list of types using the method [sale.persontype.list](../../api-reference/sale/person-type/sale-person-type-list.md).

- `propsGroupId` — the identifier of the property group. We will specify `6`. You can get a list of groups using the method [sale.propertygroup.list](../../api-reference/sale/property-group/sale-property-group-list.md).

- `name` — the name of the shipment property. We will specify `Address From`.

- `active` — the active flag. We will pass `Y`.

- `sort` — sorting.

- `type` — the type of the shipment property. We will pass `ADDRESS`. You can find a list of possible values in the documentation for the method [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md).

- `required` — the flag indicating whether the property is required. We will set it to `Y`.

- `isAddressFrom` — the flag indicating whether the shipment property is used as the sender's address. We will pass `Y`.

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

- Python

    ```python
    try:
        response = client.sale.shipmentproperty.add(
        fields={
            "personTypeId": 3,
            "propsGroupId": 6,
            "name": "Address From",
            "active": "Y",
            "sort": "100",
            "type": "ADDRESS",
            "required": "Y",
            "isAddressFrom": "Y",
        },
        ).response
        print(response.result)
    except BitrixAPIError as error:
        print(error)
    ```

{% endlist %}

If the property is successfully added, the method will return a `property` object with the property identifier. If you receive an `error`, refer to the documentation for possible errors in the method [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md).

```json
{
"result": {
    "property": {
        "active": "Y",
        "code": "",
        "defaultValue": "",
        "description": "",
        "id": 102,
        "isAddressFrom": "Y",
        "isAddressTo": "N",
        "maxLength": "",
        "name": "Address From",
        "personTypeId": 3,
        "propsGroupId": 6,
        "required": "Y",
        "settings": [],
        "sort": 100,
        "type": "ADDRESS",
        "xmlId": ""
    }
},
"time": {
    "start": 1714741422.531968,
    "finish": 1714741422.644666,
    "duration": 0.11269783973693848,
    "processing": 0.06191205978393555,
    "date_start": "2024-05-03T15:43:42+03:00",
    "date_finish": "2024-05-03T15:43:42+03:00"
}
}
```

### Property Address To

In the `fields` object for the `Address To` property, we will pass the name `Address To`. The other parameters will be similar to those for `Address From`.

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

- Python

    ```python
    try:
        response = client.sale.shipmentproperty.add(
        fields={
            "personTypeId": 3,
            "propsGroupId": 6,
            "name": "Address To",
            "active": "Y",
            "sort": "100",
            "type": "ADDRESS",
            "required": "Y",
            "isAddressTo": "Y",
        },
        ).response
        print(response.result)
    except BitrixAPIError as error:
        print(error)
    ```

{% endlist %}

If the property is successfully added, the method will return a `property` object with the property identifier. If you receive an `error`, refer to the documentation for possible errors in the method [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md).

```json
{
"result": {
    "property": {
        "active": "Y",
        "code": "",
        "defaultValue": "",
        "description": "",
        "id": 103,
        "isAddressFrom": "N",
        "isAddressTo": "Y",
        "maxLength": "",
        "name": "Address To",
        "personTypeId": 3,
        "propsGroupId": 6,
        "required": "Y",
        "settings": [],
        "sort": 100,
        "type": "ADDRESS",
        "xmlId": ""
    }
},
"time": {
    "start": 1714741719.195657,
    "finish": 1714741719.368018,
    "duration": 0.17236113548278809,
    "processing": 0.0712430477142334,
    "date_start": "2024-05-03T15:48:39+03:00",
    "date_finish": "2024-05-03T15:48:39+03:00"
}
}
```

## 4. Link Shipment Properties to the Delivery Service

To link the properties `Address From` and `Address To` to the profiles `Taxi` and `Cargo`, we will call the method [sale.propertyrelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md) four times. We will pass an object `fields` with the values for linking the properties.

- `entityId` — the identifier of the delivery profile. For the `Taxi` profile, we will pass `227`, and for `Cargo`, we will pass `228`, which were obtained [in the second step](#second).

- `entityType` — the type of object. Possible values: `P` — payment system, `D` — delivery, `L` — landing, `T` — trading platform. We will specify the value `D`.

- `propertyId` — the identifier of the property. For `Address From`, we will specify `102`, and for `Address To`, we will specify `103`, which were obtained [in the third step](#third).

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
    ```

- Python

    ```python
    try:
        response = client.sale.propertyrelation.add(
        fields={
            "entityId": 227,
            "entityType": "D",
            "propertyId": 102,
        },
        ).response
        print(response.result)
    except BitrixAPIError as error:
        print(error)
    ```

{% endlist %}

We will call the method [sale.propertyrelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md) in sequence.

1. For the `Taxi` service, property `Address From` — pass `entityId: 227, propertyId: 102`.

2. For the `Taxi` service, property `Address To` — pass `entityId: 227, propertyId: 103`.

3. For the `Cargo` service, property `Address From` — pass `entityId: 228, propertyId: 102`.

4. For the `Cargo` service, property `Address To` — pass `entityId: 228, propertyId: 103`.

If the bindings are successfully added, the method will return objects with information about them. If you receive an `error`, refer to the documentation for possible errors in the method [sale.propertyrelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md).

```json
{
    "result": {
        "propertyRelation": {
            "entityId": 227,
            "entityType": "D",
            "propertyId": 102
        }
    },
    "time": {
        "start": 1712244475.495277,
        "finish": 1712244476.402808,
        "duration": 0.9075310230255127,
        "processing": 0.08538603782653809,
        "date_start": "2024-05-03T18:27:55+03:00",
        "date_finish": "2024-05-03T18:27:56+03:00"
    }
}
```

## 5. Add Services to Delivery Services

To add an additional service to the delivery service, we will call the method [sale.delivery.extra.service.add](../../api-reference/sale/delivery/extra-service/sale-delivery-extra-service-add.md). We will pass the following parameters:

- `DELIVERY_ID` — the identifier of the delivery service to which the service will be linked. For the `Taxi` profile, we will specify the identifier `227`, which was obtained [in the second step](#second). For other profiles, substitute your own identifier. You can get a list of delivery service identifiers using the method [sale.delivery.getlist](../../api-reference/sale/delivery/delivery/sale-delivery-get-list.md).

- `ACTIVE` — the flag indicating whether the service is active. Possible values: `Y` — yes, `N` — no. We will pass `Y`.

- `CODE` — the symbolic code of the service. We will specify `door_delivery`.

- `NAME` — the name of the service, for example, `Door Delivery`.

- `TYPE` — the type of service. Possible values: `enum` — list, `checkbox` — single service, `quantity` — quantitative service. We will specify `checkbox`.

- `PRICE` — the cost of the service type in the currency of the delivery service. We will specify `1000`.

    {% note info "" %}

    For services of type `enum`, the cost is specified using the `ITEMS` parameter. For more details, refer to the documentation for the method [sale.delivery.extra.service.add](../../api-reference/sale/delivery/extra-service/sale-delivery-extra-service-add.md).

    {% endnote %}

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
            PRICE: 1000
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
            'PRICE' => 1000,
        ]
    );
    
    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    ```python
    try:
        response = client.sale.delivery.extra.service.add(
        delivery_id=227,
        type="checkbox",
        name="Door Delivery",
        active=True,
        code="door_delivery",
        price=1000,
        ).response
        print(response.result)
    except BitrixAPIError as error:
        print(error)
    ```

{% endlist %}

If the service is added, the method will return the identifier in the `result` parameter. If you receive an `error`, refer to the documentation for possible errors in the method [sale.delivery.extra.service.add](../../api-reference/sale/delivery/extra-service/sale-delivery-extra-service-add.md).

```json
{
    "result": 140,
    "time": {
        "start": 1714739042.228152,
        "finish": 1714739042.50093,
        "duration": 0.2727780342102051,
        "processing": 0.09131193161010742,
        "date_start": "2024-05-03T15:24:02+03:00",
        "date_finish": "2024-05-03T15:24:02+03:00"
    }
}
```

## Notifications about Delivery Statuses

To send notifications about the progress of delivery, you can use the methods from the group [sale.delivery.request.*](../../api-reference/sale/delivery/delivery-request/index.md).

| **Method** | **Description** |
|------------|-----------------|
| [sale.delivery.request.update](../../api-reference/sale/delivery/delivery-request/sale-delivery-request-update.md) | Updates the delivery order object: status and set of its properties |
| [sale.delivery.request.sendmessage](../../api-reference/sale/delivery/delivery-request/sale-delivery-request-send-message.md) | Sends a message to the manager or recipient about the current status of the delivery order |
| [sale.delivery.request.delete](../../api-reference/sale/delivery/delivery-request/sale-delivery-request-delete.md) | Notifies about the cancellation of the delivery order on the external system side and attempts to cancel the delivery order on the Bitrix24 side |