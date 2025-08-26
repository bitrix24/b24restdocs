# Delete the delivery service sale.delivery.extra.service.delete

> Scope: [`sale, delivery`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a delivery service.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_delivery_extra_service.ID`](../../data-types.md) | Identifier of the service.

You can obtain the identifiers of delivery services using the [sale.delivery.extra.service.get](./sale-delivery-extra-service-get.md) method
 ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":134}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.extra.service.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":134,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.extra.service.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.delivery.extra.service.delete', {
    			ID: 134,
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
                'sale.delivery.extra.service.delete',
                [
                    'ID' => 134,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting extra service: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.delivery.extra.service.delete', {
            ID: 134,
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
        'sale.delivery.extra.service.delete',
        [
            'ID' => 134,
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
    "result":true,
    "time":{
        "start":1714551146.367727,
        "finish":1714551146.571992,
        "duration":0.20426487922668457,
        "processing":0.03886008262634277,
        "date_start":"2024-05-01T11:12:26+02:00",
        "date_finish":"2024-05-01T11:12:26+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the service deletion ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error":"ERROR_EXTRA_SERVICE_NOT_FOUND",
    "error_description":"Extra service not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_EXTRA_SERVICE_NOT_FOUND` | Service with the specified identifier not found | `400` || 
|| `ERROR_CHECK_FAILURE` | Validation error of incoming parameters (details in the error description) | `400` || 
|| `ERROR_EXTRA_SERVICE_DELETE` | Error when attempting to delete the service | `400` || 
|| `ACCESS_DENIED` | Insufficient rights to delete the service | `403` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-extra-service-add.md)
- [{#T}](./sale-delivery-extra-service-update.md)
- [{#T}](./sale-delivery-extra-service-get.md)