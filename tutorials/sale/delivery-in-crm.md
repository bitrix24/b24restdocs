# Setting Up a Delivery Service for CRM

> Scope: [`sale`](../../api-reference/scopes/permissions.md)
>
> Who can execute methods: administrator

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

External delivery services can be connected to Bitrix24. This allows a manager to work with the delivery service within CRM cards: calculating costs and tracking status.

To configure a delivery service, perform the following methods in sequence:

1. [sale.delivery.handler.add](../../api-reference/sale/delivery/handler/sale-delivery-handler-add.md) — register a delivery handler,

2. [sale.delivery.add](../../api-reference/sale/delivery/delivery/sale-delivery-add.md) — create the parent service and profiles linked to the handler,

3. [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md) — add shipment properties for addresses,

4. [sale.propertyrelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md) — link properties to delivery profiles.

5. [sale.delivery.extra.service.add](../../api-reference/sale/delivery/extra-service/sale-delivery-extra-service-add.md) — connect additional services.

## 1. Create a Delivery Handler

Register a handler using [sale.delivery.handler.add](../../api-reference/sale/delivery/handler/sale-delivery-handler-add.md). Pass four parameters to the method.

- `CODE` — the symbolic code of the delivery service handler. For example, specify `uber`.

- `NAME` — the name of the delivery service handler. Pass `Uber`.

- `SETTINGS` — an object containing information about the handler settings.

    - `CALCULATE_URL` — the URL for calculating the delivery price, for example `https://gateway.bx/calculate.php`.

    - `CREATE_DELIVERY_REQUEST_URL` — the URL for processing the delivery. Specify `https://gateway.bx/create_delivery_request.php`.

    - `CANCEL_DELIVERY_REQUEST_URL` — the URL for canceling the delivery, for example `https://gateway.bx/cancel_delivery_request.php`.

    - `HAS_CALLBACK_TRACKING_SUPPORT` — an indicator of whether the service will send notifications. Set `Y`. Notifications can be created using [sale.delivery.request.sendmessage](../../api-reference/sale/delivery/delivery-request/sale-delivery-request-send-message.md).

    - `CONFIG` — a list of configurations. Specify `MY_FIRST_SETTING` and `MY_SECOND_SETTING` with type `STRING`.

- `PROFILES` — an array of delivery profiles. The handler must have at least one profile. Set `Taxi` and `Cargo`.

The delivery service must receive a request at the specified URLs, process it, and return a response in the format expected by the CRM.

For more details on the request and response format, see the [Webhooks for Working with Deliveries](../../api-reference/sale/delivery/webhooks/index.md) section.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const response = await $b24.actions.v2.call.make({
        method: 'sale.delivery.handler.add',
        params: {
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
        requestId: 'delivery-handler-add'
    })

    if (response.isSuccess) {
        console.info(response.getData().result)
    } else {
        console.error(response.getErrorMessages().join('; '))
    }
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $result = $sb->getSaleScope()->deliveryHandler()->add([
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
    ]);

    echo '<PRE>';
    print_r($result->getId());
    echo '</PRE>';
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
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

If the handler is successfully added, the method returns its identifier. If you receive error `error`, review the possible errors described in the [sale.delivery.handler.add](../../api-reference/sale/delivery/handler/sale-delivery-handler-add.md) method documentation.

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

Create a delivery service using the [sale.delivery.add](../../api-reference/sale/delivery/delivery/sale-delivery-add.md) method. Pass the following parameters to the method:

- `REST_CODE` — the symbolic code of the delivery service handler. We will specify `uber`, which was set in the first step.

- `NAME` — the name of the delivery service, for example, `Uber Taxi`.

- `CURRENCY` — the symbolic code of the currency. We will pass `RUB`. You can retrieve a list of currencies using the [crm.currency.list](../../api-reference/crm/currency/crm-currency-list.md) method.

- `ACTIVE` — the delivery service activity flag. We will specify `Y`.

- `CONFIG` — the handler configuration values. We are passing values for `MY_FIRST_SETTING` and `MY_SECOND_SETTING`, which were set in the first step.

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'sale.delivery.add',
        params: {
            REST_CODE: "uber",
            NAME: "Uber Taxi",
            CURRENCY: "EUR",
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
        requestId: 'delivery-add'
    })

    if (response.isSuccess) {
        console.info(response.getData().result)
    } else {
        console.error(response.getErrorMessages().join('; '))
    }
    ```

- PHP

    ```php
    $result = $sb->getSaleScope()->delivery()->add([
        'REST_CODE' => 'uber',
        'NAME' => 'Uber Taxi',
        'CURRENCY' => 'EUR',
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
    ]);

    echo '<PRE>';
    print_r($result->getParent()->ID);
    echo '</PRE>';
    ```

- Python

    ```python
    try:
        response = client.sale.delivery.add(
            rest_code="uber",
            name="Uber Taxi",
            currency="RUB",
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

If the delivery service is successfully created, the method returns a parent service object and an array of profiles. If you receive error `error`, review the description of possible errors in the [sale.delivery.add](../../api-reference/sale/delivery/delivery/sale-delivery-add.md) method documentation.

```json
{
"result":{
    "parent":{
        "NAME":"Uber Taxi",
        "ACTIVE":"Y",
        "DESCRIPTION":"",
        "CURRENCY":"EUR",
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
            "CURRENCY":"EUR",
            "ID":227,
            "PARENT_ID":226,
            "SORT":100,
            "LOGOTYPE":null
        },
        {
            "NAME":"Cargo",
            "ACTIVE":"Y",
            "DESCRIPTION":"Cargo Delivery",
            "CURRENCY":"EUR",
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
    "date_start":"2024-05-03T14:52:02+03:00",
    "date_finish":"2024-05-03T14:52:02+03:00"
}
}
```

## 3. Create Shipment Properties {#third}

In a shipment, a manager specifies the origin address and the delivery address. We will sequentially create two properties, `Address From` and `Address To`, using the [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md) method.

### Address from Property

Pass the `fields` object to the method with the values for the `Address From` property fields.

- `personTypeId` — the payer type identifier. We will pass `3`. You can retrieve the list of types using the [sale.persontype.list](../../api-reference/sale/person-type/sale-person-type-list.md) method.

- `propsGroupId` — the property group identifier. We will specify `6`. You can retrieve the list of groups using the [sale.propertygroup.list](../../api-reference/sale/property-group/sale-property-group-list.md) method.

- `name` — the shipment property name. We will specify `Address From`.

- `active` — the activity flag. We will pass `Y`.

- `sort` — sorting.

- `type` — the shipment property type. We will pass `ADDRESS`. See the [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md) method documentation for a list of possible values.

- `required` — a flag indicating whether the property is required. We will specify `Y`.

- `isAddressFrom` — a flag indicating whether the shipment property is used as the origin address. We will pass `Y`.

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'sale.shipmentproperty.add',
        params: {
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
        requestId: 'shipmentproperty-add-from'
    })

    if (response.isSuccess) {
        console.info(response.getData().result)
    } else {
        console.error(response.getErrorMessages().join('; '))
    }
    ```

- PHP

    ```php
    $result = $sb->getSaleScope()->shipmentProperty()->add([
        'personTypeId' => 3,
        'propsGroupId' => 6,
        'name' => 'Address From',
        'active' => 'Y',
        'sort' => '100',
        'type' => 'ADDRESS',
        'required' => 'Y',
        'isAddressFrom' => 'Y'
    ]);

    echo '<PRE>';
    print_r($result->getId());
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

If the property is successfully added, the method returns an `property` object containing the property identifier. If you receive error `error`, review the description of possible errors in the [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md) method documentation.

```json
{
"result":{
    "property":{
        "active":"Y",
        "code":"",
        "defaultValue":"",
        "description":"",
        "id":102,
        "isAddressFrom":"Y",
        "isAddressTo":"N",
        "maxLength":"",
        "name":"Address From",
        "personTypeId":3,
        "propsGroupId":6,
        "required":"Y",
        "settings":[],
        "sort":100,
        "type":"ADDRESS",
        "xmlId":""
    }
},
"time":{
    "start":1714741422.531968,
    "finish":1714741422.644666,
    "duration":0.11269783973693848,
    "processing":0.06191205978393555,
    "date_start":"2024-05-03T15:43:42+03:00",
    "date_finish":"2024-05-03T15:43:42+03:00"
}
}
```

### Address to Property

In the `fields` object for the `Address To` property, we pass the name `Address To`. The other parameters are similar `Address From`.

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'sale.shipmentproperty.add',
        params: {
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
        requestId: 'shipmentproperty-add-to'
    })

    if (response.isSuccess) {
        console.info(response.getData().result)
    } else {
        console.error(response.getErrorMessages().join('; '))
    }
    ```

- PHP

    ```php
    $result = $sb->getSaleScope()->shipmentProperty()->add([
        'personTypeId' => 3,
        'propsGroupId' => 6,
        'name' => 'Address To',
        'active' => 'Y',
        'sort' => '100',
        'type' => 'ADDRESS',
        'required' => 'Y',
        'isAddressTo' => 'Y'
    ]);

    echo '<PRE>';
    print_r($result->getId());
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

If the property is successfully added, the method returns an `property` object containing the property identifier. If you receive error `error`, review the description of possible errors in the [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md) method documentation.

```json
{
"result":{
    "property":{
        "active":"Y",
        "code":"",
        "defaultValue":"",
        "description":"",
        "id":103,
        "isAddressFrom":"N",
        "isAddressTo":"Y",
        "maxLength":"",
        "name":"Address To",
        "personTypeId":3,
        "propsGroupId":6,
        "required":"Y",
        "settings":[],
        "sort":100,
        "type":"ADDRESS",
        "xmlId":""
    }
},
"time":{
    "start":1714741719.195657,
    "finish":1714741719.368018,
    "duration":0.17236113548278809,
    "processing":0.0712430477142334,
    "date_start":"2024-05-03T15:48:39+03:00",
    "date_finish":"2024-05-03T15:48:39+03:00"
}
}
```

## 4. Link Shipment Properties to a Delivery Service

To link properties `Address From` and `Address To` to profiles `Taxi` and `Cargo`, we will call the method [sale.propertyrelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md) four times. We will pass the `fields` object to the method with field values for linking the properties.

- `entityId` — the delivery profile identifier. For profile `Taxi` we will pass `227`, and for `Cargo` — `228`, which were obtained [in the second step](#second).

- `entityType` — the object type. Possible values: `P` — payment system, `D` — delivery, `L` — landing page, `T` — trading platform. We will specify the value `D`.

- `propertyId` — the property identifier. For `Address From` we will specify `102`, and for `Address To` — `103`, which were obtained [in the third step](#third).

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'sale.propertyrelation.add',
        params: {
            fields: {
                entityId: 227,
                entityType: 'D',
                propertyId: 102
            }
        },
        requestId: 'propertyrelation-add'
    })

    if (response.isSuccess) {
        console.info(response.getData().result)
    } else {
        console.error(response.getErrorMessages().join('; '))
    }
    ```

- PHP

    ```php
    $result = $sb->getSaleScope()->propertyRelation()->add([
        'entityId' => 227,
        'entityType' => 'D',
        'propertyId' => 102
    ]);
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

Call the [sale.propertyrelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md) method sequentially.

1. Service `Taxi`, property `Address From` — pass `entityId: 227, propertyId: 102`.

2. Service `Taxi`, property `Address To` — pass `entityId: 227, propertyId: 103`.

3. Service `Cargo`, property `Address From` — pass `entityId: 228, propertyId: 102`.

4. Service `Cargo`, property `Address To` — pass `entityId: 228, propertyId: 103`.

If the links are successfully added, the method will return objects containing information about them. If you receive error `error`, review the description of possible errors in the [sale.propertyrelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md) method documentation.

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

## 5. Adding Services to Delivery Services

To add an extra service to a delivery service, call the [sale.delivery.extra.service.add](../../api-reference/sale/delivery/extra-service/sale-delivery-extra-service-add.md) method. Pass the following parameters to it:

- `DELIVERY_ID` — the identifier of the delivery service to which the service will be linked. For the `Taxi` profile, we will specify the `227` identifier obtained [in the second step](#second). For other profiles, substitute your own identifier. You can retrieve a list of delivery service identifiers using the [sale.delivery.getlist](../../api-reference/sale/delivery/delivery/sale-delivery-get-list.md) method.

- `ACTIVE` — the service activity flag. Possible values: `Y` — yes, `N` — no. We will pass `Y`.

- `CODE` — the symbolic code of the service. We will specify `door_delivery`.

- `NAME` — the name of the service, for example, `Door Delivery`.

- `TYPE` — the service type. Possible values: `enum` — list, `checkbox` — single service, `quantity` — quantity service. We will specify `checkbox`.

- `PRICE` — the cost of the service type in the delivery service currency. We will specify `1000`.

    {% note info "" %}

    For services of type `enum`, the cost is specified using the `ITEMS` parameter. For more details, see the [sale.delivery.extra.service.add](../../api-reference/sale/delivery/extra-service/sale-delivery-extra-service-add.md) method documentation.

    {% endnote %}

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'sale.delivery.extra.service.add',
        params: {
            DELIVERY_ID: 227,
            ACTIVE: "Y",
            CODE: "door_delivery",
            NAME: "Door Delivery",
            TYPE: "checkbox",
            PRICE: 1000
        },
        requestId: 'delivery-extra-service-add'
    })

    if (response.isSuccess) {
        console.info(response.getData().result)
    } else {
        console.error(response.getErrorMessages().join('; '))
    }
    ```

- PHP

    ```php
    $result = $sb->getSaleScope()->deliveryExtraService()->add([
        'DELIVERY_ID' => 227,
        'ACTIVE' => 'Y',
        'CODE' => 'door_delivery',
        'NAME' => 'Door Delivery',
        'TYPE' => 'checkbox',
        'PRICE' => 1000,
    ]);

    echo '<PRE>';
    print_r($result->getId());
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
            price=1000.0,
        ).response
        print(response.result)
    except BitrixAPIError as error:
        print(error)
    ```

{% endlist %}

If the service is added, the method returns the identifier in the `result` parameter. If an error is received `error`, review the description of possible errors in the [sale.delivery.extra.service.add](../../api-reference/sale/delivery/extra-service/sale-delivery-extra-service-add.md) method documentation.

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

## Delivery Status Notifications

To send notifications regarding the progress of a delivery, you can use the [sale.delivery.request.*](../../api-reference/sale/delivery/delivery-request/index.md) group of methods.

#|
|| **Method** | **Description** ||
|| [sale.delivery.request.update](../../api-reference/sale/delivery/delivery-request/sale-delivery-request-update.md) | Updates the delivery order object: status and its set of properties ||
|| [sale.delivery.request.sendmessage](../../api-reference/sale/delivery/delivery-request/sale-delivery-request-send-message.md) | Sends a message to the manager or recipient about the current status of the delivery order ||
|| [sale.delivery.request.delete](../../api-reference/sale/delivery/delivery-request/sale-delivery-request-delete.md) | Reports the cancellation of a delivery order on the external system side and attempts to cancel the delivery order on the Bitrix24 side ||
|#
