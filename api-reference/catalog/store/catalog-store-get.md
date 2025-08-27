# Get Store Field Values catalog.store.get

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns the field values of a store by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_store.id`](../data-types.md#catalog_store) | Store identifier.

You can obtain store identifiers using the [catalog.store.list](./catalog-store-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.store.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.store.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.store.get', {
    			id: 1
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
                'catalog.store.get',
                [
                    'id' => 1
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting store information: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.store.get', {
            id: 1
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
        'catalog.store.get',
        [
            'id' => 1
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
    "result": {
        "store": {
            "active": "Y",
            "address": "Main St. 52",
            "code": "store_1",
            "dateCreate": "2024-10-18T16:30:45+02:00",
            "dateModify": "2024-10-21T14:29:06+02:00",
            "description": "Description",
            "email": "test@test.com",
            "gpsN": 54.71411,
            "gpsS": 21.56675,
            "id": 1,
            "imageId": {
                "id": 1,
                "url": "\/upload\/iblock\/6f1\/bkm7jmwso31wisk423gtp28iagy2e8v0\/test.jpeg"
            },
            "issuingCenter": "N",
            "modifiedBy": 1,
            "phone": "+1 (495) 212 85 06",
            "schedule": "Mon.-Fri. from 9:00 AM to 8:00 PM, Sat.-Sun. from 11:00 AM to 6:00 PM",
            "sort": 100,
            "title": "Store 1",
            "userId": 1,
            "xmlId": null
        }
    },
    "time": {
        "start": 1729519143.740275,
        "finish": 1729519144.2594,
        "duration": 0.5191249847412109,
        "processing": 0.0425570011138916,
        "date_start": "2024-10-21T16:59:03+02:00",
        "date_finish": "2024-10-21T16:59:04+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **store**
[`catalog_store`](../data-types.md#catalog_store) | Object containing store information ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to view the store ||
|| `201100000000` | Store with the specified identifier not found ||
|| `100` | Parameter `id` not specified || 
|| `0` | Other errors (e.g., fatal errors) || 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-store-add.md)
- [{#T}](./catalog-store-update.md)
- [{#T}](./catalog-store-list.md)
- [{#T}](./catalog-store-delete.md)
- [{#T}](./catalog-store-get-fields.md)