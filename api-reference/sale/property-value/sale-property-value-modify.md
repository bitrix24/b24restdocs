# Change the value of the property sale.propertyvalue.modify

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the property values of an order.

**Please note** that this method accepts **all** property values of the order as input. If any property values are not provided, their current values will be removed from the order as a result of the request.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | The root element where the request parameters are passed in the form of a structure:

```json
{
    "order":
    {
        "id": "value",
        "propertyValues": {
            "orderPropsId": "value",
            "value": "value"
        },
    }
}
```

||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **order***
[`object`](../../data-types.md) | An object containing the order ID and property values of the order in the form of a structure:

```json
"id": "value",
"propertyValues": {
    "orderPropsId": "value",
    "value": "value"
},
```

||
|#

### Parameter order

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order.id`](../data-types.md) | Order ID ||
|| **propertyValues***
[`object[]`](../../data-types.md) | An array of objects (see the description of the `propertyValues` object [below](#parametr-propertyvalues)), containing the order property ID and property value ||
|#

### Parameter propertyValues

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Value**
`type` | **Description** ||
|| **orderPropsId***
[`sale_order_property.id`](../data-types.md) | Order property ID ||
|| **value***
[`string`](../../data-types.md) | Order property value ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"order":{"id":2066,"propertyValues":[{"orderPropsId":20,"value":"John Smith"},{"orderPropsId":21,"value":"johnsmith@example.com"},{"orderPropsId":22,"value":"+10907996161"},{"orderPropsId":25,"value":"0000073738"},{"orderPropsId":26,"value":"900 S Holland Ave, Springfield, MO 65806, United States"},{"orderPropsId":51,"value":"04/17/2024"},{"orderPropsId":52,"value":"Y"},{"orderPropsId":53,"value":"948"},{"orderPropsId":54,"value":"10"}]}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyvalue.modify
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"order":{"id":2066,"propertyValues":[{"orderPropsId":20,"value":"John Smith"},{"orderPropsId":21,"value":"johnsmith@example.com"},{"orderPropsId":22,"value":"+10907996161"},{"orderPropsId":25,"value":"0000073738"},{"orderPropsId":26,"value":"900 S Holland Ave, Springfield, MO 65806, United States"},{"orderPropsId":51,"value":"04/17/2024"},{"orderPropsId":52,"value":"Y"},{"orderPropsId":53,"value":"948"},{"orderPropsId":54,"value":"10"}]}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyvalue.modify
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.propertyvalue.modify', {
            fields: {
                order: {
                    id: 2066,
                    propertyValues: [{
                            orderPropsId: 20,
                            value: 'John Smith'
                        },
                        {
                            orderPropsId: 21,
                            value: 'johnsmith@example.com'
                        },
                        {
                            orderPropsId: 22,
                            value: '+10907996161'
                        },
                        {
                            orderPropsId: 25,
                            value: '0000073738'
                        },
                        {
                            orderPropsId: 26,
                            value: '900 S Holland Ave, Springfield, MO 65806, United States'
                        },
                        {
                            orderPropsId: 51,
                            value: '04/17/2024'
                        },
                        {
                            orderPropsId: 52,
                            value: 'Y'
                        },
                        {
                            orderPropsId: 53,
                            value: '948'
                        },
                        {
                            orderPropsId: 54,
                            value: '10'
                        },
                    ],
                },
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
        'sale.propertyvalue.modify',
        [
            'fields' => [
                'order' => [
                    'id' => 2066,
                    'propertyValues' => [
                        [
                            'orderPropsId' => 20,
                            'value' => 'John Smith'
                        ],
                        [
                            'orderPropsId' => 21,
                            'value' => 'johnsmith@example.com'
                        ],
                        [
                            'orderPropsId' => 22,
                            'value' => '+10907996161'
                        ],
                        [
                            'orderPropsId' => 25,
                            'value' => '0000073738'
                        ],
                        [
                            'orderPropsId' => 26,
                            'value' => '900 S Holland Ave, Springfield, MO 65806, United States'
                        ],
                        [
                            'orderPropsId' => 51,
                            'value' => '04/17/2024'
                        ],
                        [
                            'orderPropsId' => 52,
                            'value' => 'Y'
                        ],
                        [
                            'orderPropsId' => 53,
                            'value' => '948' // Corrected quotation marks
                        ],
                        [
                            'orderPropsId' => 54,
                            'value' => '10' // Corrected quotation marks
                        ],
                    ],
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
    "result":{
        "propertyValues":[
            {
                "code":"DELIVERY_DATE",
                "id":13167,
                "name":"Delivery Date",
                "orderPropsId":51,
                "orderPropsXmlId":null,
                "value":"04/17/2024"
            },
            {
                "code":"DELEVERY_REQUIRED",
                "id":13168,
                "name":"Delivery Required",
                "orderPropsId":52,
                "orderPropsXmlId":null,
                "value":"Y"
            },
            {
                "code":"DOC",
                "id":13170,
                "name":"Document confirming the right to purchase the goods",
                "orderPropsId":53,
                "orderPropsXmlId":null,
                "value":{
                "contentType":"image\/jpeg",
                "description":"",
                "externalId":"89359a6dc806e5dc5404962d1be0ba42",
                "fileName":"fit.jpeg",
                "fileSize":"84058",
                "handlerId":null,
                "height":"800",
                "id":"948",
                "meta":"",
                "moduleId":"fileman",
                "originalName":"fit.jpeg",
                "src":"\/upload\/medialibrary\/5bd\/ukgwed4vdxe1qaqv85pqrvdgq5moyhqh\/fit.jpeg",
                "subdir":"medialibrary\/5bd\/ukgwed4vdxe1qaqv85pqrvdgq5moyhqh",
                "timestampX":"04/02/2024 09:13:29",
                "versionOriginalId":null,
                "width":"600"
                }
            },
            {
                "code":"MAX_DELIVERY_DAYS",
                "id":13171,
                "name":"Deliver no later than (days after order)",
                "orderPropsId":54,
                "orderPropsXmlId":null,
                "value":"10"
            },
            {
                "code":"FIO",
                "id":13163,
                "name":"Full Name",
                "orderPropsId":20,
                "orderPropsXmlId":null,
                "value":"John Smith"
            },
            {
                "code":"EMAIL",
                "id":13164,
                "name":"E-Mail",
                "orderPropsId":21,
                "orderPropsXmlId":null,
                "value":"johnsmith@example.com"
            },
            {
                "code":"PHONE",
                "id":13165,
                "name":"Phone",
                "orderPropsId":22,
                "orderPropsXmlId":null,
                "value":"+10907996161"
            },
            {
                "code":"LOCATION",
                "id":13166,
                "name":"Location",
                "orderPropsId":25,
                "orderPropsXmlId":"bx_63e3a3b974932",
                "value":"0000073738"
            },
            {
                "code":"ADDRESS",
                "id":13162,
                "name":"Delivery Address",
                "orderPropsId":26,
                "orderPropsXmlId":null,
                "value":"900 S Holland Ave, Springfield, MO 65806, United States"
            }
        ]
    },
    "time":{
        "start":1712049055.756753,
        "finish":1712049056.823234,
        "duration":1.066481113433838,
        "processing":0.7959079742431641,
        "date_start":"2024-04-02T12:10:55+03:00",
        "date_finish":"2024-04-02T12:10:56+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **propertyValues**
[`sale_order_property_value[]`](../data-types.md) | An array of objects containing information about the order property values ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":100,
    "error_description":"Could not find value for parameter {fields}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to change order property values ||
|| `100` | Parameter `fields` is missing or empty ||
|| `0` | Order not found ||
|| `0` | Required parameters are missing ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-value-get.md)
- [{#T}](./sale-property-value-list.md)
- [{#T}](./sale-property-value-delete.md)
- [{#T}](./sale-property-value-get-fields.md)