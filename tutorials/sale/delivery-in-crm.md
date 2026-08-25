# How to Set Up a Delivery Service for CRM

> Scope: [`delivery`](../../api-reference/scopes/permissions.md), [`sale`](../../api-reference/scopes/permissions.md)
>
> Who can execute methods: administrator

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

External delivery services can be connected to Bitrix24. This allows a manager to work with the delivery service within CRM cards: calculating costs and tracking status.

As a result of the scenario, a delivery service with profiles, shipment address properties, and an extra service will appear in CRM.

The scenario consists of five steps.

1. Register a delivery handler using the [sale.delivery.handler.add](../../api-reference/sale/delivery/handler/sale-delivery-handler-add.md) method
2. Create the parent service and profiles using the [sale.delivery.add](../../api-reference/sale/delivery/delivery/sale-delivery-add.md) method
3. Add shipment properties for addresses using the [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md) method
4. Link properties to delivery profiles using the [sale.propertyRelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md) method
5. Connect an extra service using the [sale.delivery.extra.service.add](../../api-reference/sale/delivery/extra-service/sale-delivery-extra-service-add.md) method

## Before You Start

Prepare the values used in the examples.

- an inbound webhook or OAuth token of a user with administrator permissions
- public HTTPS handler URLs: `CALCULATE_URL`, `CREATE_DELIVERY_REQUEST_URL`, `CANCEL_DELIVERY_REQUEST_URL`
- a unique delivery handler code, for example `uber`
- the payer type identifier `personTypeId`. You can retrieve the list of types using the [sale.persontype.list](../../api-reference/sale/person-type/sale-person-type-list.md) method
- the property group identifier `propsGroupId`. You can retrieve the list of groups using the [sale.propertygroup.list](../../api-reference/sale/property-group/sale-property-group-list.md) method

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

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

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
{% endlist %}

If the handler is successfully added, the method returns its identifier.

```json
{
    "result": 23
}
```

## 2. Create a Delivery Service {#second}

Create a delivery service using the [sale.delivery.add](../../api-reference/sale/delivery/delivery/sale-delivery-add.md) method. Pass the following parameters to the method:

- `REST_CODE` — the symbolic code of the delivery service handler. We will specify `uber`, which was set in the first step.

- `NAME` — the name of the delivery service, for example, `Uber Taxi`.

- `CURRENCY` — the symbolic code of the currency. We will pass `EUR`. You can retrieve a list of currencies using the [crm.currency.list](../../api-reference/crm/currency/crm-currency-list.md) method.

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

- Python

    ```python
    try:
        response = client.sale.delivery.add(
            rest_code="uber",
            name="Uber Taxi",
            currency="EUR",
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
{% endlist %}

If the delivery service is successfully created, the method returns a parent service object and an array of profiles. Retain the profile identifiers from the `profiles` array: they are required to link shipment properties and extra services.

```json
{
    "result": {
        "parent": {
            "NAME": "Uber Taxi",
            "ACTIVE": "Y",
            "CURRENCY": "EUR",
            "ID": 226,
            "PARENT_ID": null
        },
        "profiles": [
            {
                "NAME": "Taxi",
                "ACTIVE": "Y",
                "ID": 227,
                "PARENT_ID": 226
            },
            {
                "NAME": "Cargo",
                "ACTIVE": "Y",
                "ID": 228,
                "PARENT_ID": 226
            }
        ]
    }
}
```

## 3. Create Shipment Properties {#third}

In a shipment, a manager specifies the origin address and the delivery address. We will sequentially create two properties, `Address From` and `Address To`, using the [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md) method.

The examples use `personTypeId: 3` and `propsGroupId: 6`. If your Bitrix24 has different payer types or property groups, substitute the values retrieved by the [sale.persontype.list](../../api-reference/sale/person-type/sale-person-type-list.md) and [sale.propertygroup.list](../../api-reference/sale/property-group/sale-property-group-list.md) methods.

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
{% endlist %}

If the property is successfully added, the method returns a `property` object with the property identifier. Retain the `property.id` value: it is required to link the property to delivery profiles.

```json
{
    "result": {
        "property": {
            "id": 102,
            "name": "Address From",
            "isAddressFrom": "Y",
            "isAddressTo": "N",
            "type": "ADDRESS"
        }
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
{% endlist %}

If the property is successfully added, the method returns a `property` object with the property identifier. Retain the `property.id` value: it is required to link the property to delivery profiles.

```json
{
    "result": {
        "property": {
            "id": 103,
            "name": "Address To",
            "isAddressFrom": "N",
            "isAddressTo": "Y",
            "type": "ADDRESS"
        }
    }
}
```

## 4. Link Shipment Properties to a Delivery Service

To link properties `Address From` and `Address To` to profiles `Taxi` and `Cargo`, we will call the [sale.propertyRelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md) method four times. We will pass the `fields` object to the method with field values for linking the properties.

- `entityId` — the delivery profile identifier. For profile `Taxi` we will pass `227`, and for `Cargo` — `228`, which were obtained [in the second step](#second).

- `entityType` — the object type. Possible values: `P` — payment system, `D` — delivery, `L` — landing page, `T` — trading platform. We will specify the value `D`.

- `propertyId` — the property identifier. For `Address From` we will specify `102`, and for `Address To` — `103`, which were obtained [in the third step](#third).

The values `227`, `228`, `102`, and `103` are examples. In a production scenario, substitute the identifiers from the responses of the `sale.delivery.add` and `sale.shipmentproperty.add` methods.

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'sale.propertyRelation.add',
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


- PHP

    ```php
    $result = $sb->getSaleScope()->propertyRelation()->add([
        'entityId' => 227,
        'entityType' => 'D',
        'propertyId' => 102
    ]);
    ```
{% endlist %}

Call the [sale.propertyRelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md) method sequentially.

1. Service `Taxi`, property `Address From` — pass `entityId: 227, propertyId: 102`.

2. Service `Taxi`, property `Address To` — pass `entityId: 227, propertyId: 103`.

3. Service `Cargo`, property `Address From` — pass `entityId: 228, propertyId: 102`.

4. Service `Cargo`, property `Address To` — pass `entityId: 228, propertyId: 103`.

If the links are successfully added, the method returns objects with information about them.

```json
{
    "result": {
        "propertyRelation": {
            "entityId": 227,
            "entityType": "D",
            "propertyId": 102
        }
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
{% endlist %}

If the service is added, the method returns the identifier in the `result` parameter.

```json
{
    "result": 140
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

## Check the Result

Open a CRM card with a shipment and verify that the `Uber Taxi` delivery service, its `Taxi` and `Cargo` profiles, the `Address From` and `Address To` shipment properties, and the `Door Delivery` extra service are available.

Through REST, check the created objects using the [sale.delivery.getlist](../../api-reference/sale/delivery/delivery/sale-delivery-get-list.md), [sale.shipmentproperty.list](../../api-reference/sale/shipment-property/sale-shipment-property-list.md), [sale.propertyRelation.list](../../api-reference/sale/property-relation/sale-property-relation-list.md), and [sale.delivery.extra.service.get](../../api-reference/sale/delivery/extra-service/sale-delivery-extra-service-get.md) methods.

{% list tabs %}

- JS

    ```js
    const deliveryResponse = await $b24.actions.v2.call.make({
        method: 'sale.delivery.getlist',
        params: {
            SELECT: ['ID', 'NAME', 'PARENT_ID', 'ACTIVE'],
            FILTER: { '=NAME': 'Uber Taxi' },
        },
        requestId: 'delivery-getlist-check',
    })

    const propertyResponse = await $b24.actions.v2.call.make({
        method: 'sale.shipmentproperty.list',
        params: {
            select: ['id', 'name', 'isAddressFrom', 'isAddressTo'],
            filter: { '=name': ['Address From', 'Address To'] },
        },
        requestId: 'shipmentproperty-list-check',
    })

    const relationResponse = await $b24.actions.v2.call.make({
        method: 'sale.propertyRelation.list',
        params: {
            select: ['entityId', 'entityType', 'propertyId'],
            filter: { entityId: 227, entityType: 'D' },
        },
        requestId: 'propertyrelation-list-check',
    })

    const extraServiceResponse = await $b24.actions.v2.call.make({
        method: 'sale.delivery.extra.service.get',
        params: { DELIVERY_ID: 227 },
        requestId: 'delivery-extra-service-get-check',
    })

    console.log(deliveryResponse.getData().result)
    console.log(propertyResponse.getData().result)
    console.log(relationResponse.getData().result)
    console.log(extraServiceResponse.getData().result)
    ```

- Python

    ```python
    deliveries = token.call_method(
        "sale.delivery.getlist",
        {
            "SELECT": ["ID", "NAME", "PARENT_ID", "ACTIVE"],
            "FILTER": {"=NAME": "Uber Taxi"},
        },
    )
    properties = token.call_method(
        "sale.shipmentproperty.list",
        {
            "select": ["id", "name", "isAddressFrom", "isAddressTo"],
            "filter": {"=name": ["Address From", "Address To"]},
        },
    )
    relations = token.call_method(
        "sale.propertyRelation.list",
        {
            "select": ["entityId", "entityType", "propertyId"],
            "filter": {"entityId": 227, "entityType": "D"},
        },
    )
    extra_services = token.call_method(
        "sale.delivery.extra.service.get",
        {"DELIVERY_ID": 227},
    )

    print(deliveries)
    print(properties)
    print(relations)
    print(extra_services)
    ```


- PHP

    ```php
    $deliveryResponse = $sb->core->call('sale.delivery.getlist', [
        'SELECT' => ['ID', 'NAME', 'PARENT_ID', 'ACTIVE'],
        'FILTER' => ['=NAME' => 'Uber Taxi'],
    ]);

    $propertyResponse = $sb->core->call('sale.shipmentproperty.list', [
        'select' => ['id', 'name', 'isAddressFrom', 'isAddressTo'],
        'filter' => ['=name' => ['Address From', 'Address To']],
    ]);

    $relationResponse = $sb->core->call('sale.propertyRelation.list', [
        'select' => ['entityId', 'entityType', 'propertyId'],
        'filter' => ['entityId' => 227, 'entityType' => 'D'],
    ]);

    $extraServiceResponse = $sb->core->call('sale.delivery.extra.service.get', [
        'DELIVERY_ID' => 227,
    ]);

    print_r($deliveryResponse->getResponseData()->getResult());
    print_r($propertyResponse->getResponseData()->getResult());
    print_r($relationResponse->getResponseData()->getResult());
    print_r($extraServiceResponse->getResponseData()->getResult());
    ```
{% endlist %}

The scenario is successful if the method responses confirm these results:

- `sale.delivery.add` returned the parent service in the `parent` object and the profiles in the `profiles` array
- `sale.shipmentproperty.add` returned the identifiers of the `Address From` and `Address To` properties
- `sale.propertyRelation.add` returned the links between the properties and the delivery profiles
- `sale.delivery.extra.service.add` returned the identifier of the extra service

## Errors and Diagnostics

If a method returns an error, verify the request data and the values passed between the steps.

#|
|| **Error code or text** | **Cause and action** ||
|| `ACCESS_DENIED` | The method was called by a user without administrator permissions ||
|| `ERROR_CHECK_FAILURE` | A required parameter is missing or a value failed validation. Check `CODE`, `NAME`, `SETTINGS`, `PROFILES`, `REST_CODE`, `CURRENCY`, `fields`, and `DELIVERY_ID` ||
|| `ERROR_HANDLER_ALREADY_EXIST` | A handler with this `CODE` already exists. Specify a different code or reuse the existing handler ||
|| `ERROR_HANDLER_NOT_FOUND` | The delivery service is being created with a `REST_CODE` that has no handler. Check that `REST_CODE` matches the `CODE` from step 1 ||
|| `ERROR_DELIVERY_ADD`, `ERROR_HANDLER_ADD`, `ERROR_EXTRA_SERVICE_ADD` | An error occurred while adding the service, handler, or extra service. Review `error_description` for details ||
|| `ERROR_DELIVERY_NOT_FOUND` | No delivery service was found for the specified `DELIVERY_ID`. For an extra service, pass the delivery profile identifier ||
|| `Required fields: entityId` | The property relation request does not include the delivery profile identifier. Take it from the `profiles` array returned by `sale.delivery.add` ||
|| `201650000001` | This property relation already exists. You do not need to create it again when rerunning the example ||
|| `201650000002` | The property was not found. Check the `propertyId` value returned by `sale.shipmentproperty.add` ||
|#

## What to Consider

- rerunning the example with the same `CODE` may fail because the handler code must be unique
- delivery profiles are created in step 2; save their identifiers before you configure properties and extra services
- the external service must accept requests on the HTTPS endpoints specified in the handler settings

## Continue Learning

- [Webhooks for Working with Deliveries](../../api-reference/sale/delivery/webhooks/index.md)
- [Delivery Services](../../api-reference/sale/delivery/delivery/index.md)
- [Shipment Properties](../../api-reference/sale/shipment-property/index.md)
- [Extra Services of Delivery Services](../../api-reference/sale/delivery/extra-service/index.md)
