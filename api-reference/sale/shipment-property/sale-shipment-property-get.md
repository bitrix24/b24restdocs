# Get Shipment Property sale.shipmentproperty.get

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves the shipment property.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_shipment_property.id`](../data-types.md) | Identifier of the shipment property ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":22}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentproperty.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":22,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentproperty.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.shipmentproperty.get", {
    			"id": 22
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.shipmentproperty.get',
                [
                    'id' => 22,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting shipment property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.shipmentproperty.get", {
            "id": 22
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.shipmentproperty.get',
        [
            'id' => 22
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP status: **200**

```json
{
    "result": {
        "property": {
            "active": "Y",
            "code": "PHONE",
            "defaultValue": "",
            "description": "",
            "id": 22,
            "inputFieldLocation": "0",
            "isAddress": "N",
            "isAddressFrom": "N",
            "isAddressTo": "N",
            "isEmail": "N",
            "isFiltered": "N",
            "isLocation": "N",
            "isLocation4tax": "N",
            "isPayer": "N",
            "isPhone": "Y",
            "isProfileName": "N",
            "isZip": "N",
            "multiple": "N",
            "name": "Phone",
            "personTypeId": 3,
            "propsGroupId": 5,
            "required": "Y",
            "settings": [],
            "sort": 120,
            "type": "STRING",
            "userProps": "Y",
            "util": "N",
            "xmlId": ""
        }
    },
    "time": {
        "start": 1712818820.026505,
        "finish": 1712818820.232912,
        "duration": 0.2064070701599121,
        "processing": 0.018364906311035156,
        "date_start": "2024-04-11T10:00:20+02:00",
        "date_finish": "2024-04-11T10:00:20+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **property**
[`sale_shipment_property`](../data-types.md) | Information about the shipment property ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":200840400001,
   "error_description":"property does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200840400001` | Shipment property not found ||
|| `200040300010` | Insufficient permissions to read the shipment property ||
|| `100` | Parameter `id` not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-property-add.md)
- [{#T}](./sale-shipment-property-update.md)
- [{#T}](./sale-shipment-property-list.md)
- [{#T}](./sale-shipment-property-delete.md)
- [{#T}](./sale-shipment-property-get-fields-by-type.md)