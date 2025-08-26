# Get a list of delivery service handlers sale.delivery.handler.list

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

This method retrieves a list of delivery service handlers.

No parameters.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","PARENT_ID","NAME","ACTIVE","DESCRIPTION","SORT","LOGOTIP","CURRENCY"],"FILTER":{"@ID":[196,197,198]},"ORDER":{"SORT":"ASC","ID":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.getlist
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","PARENT_ID","NAME","ACTIVE","DESCRIPTION","SORT","LOGOTIP","CURRENCY"],"FILTER":{"@ID":[196,197,198]},"ORDER":{"SORT":"ASC","ID":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.getlist
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'sale.delivery.handler.list',
        {},
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('sale.delivery.handler.list', {}, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. Suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
      const response = await $b24.callMethod('sale.delivery.handler.list', {}, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.delivery.handler.list',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching delivery handlers: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.delivery.handler.list', {},
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
        'sale.delivery.getlist',
        [
            'SELECT' => [
                "ID",
                "PARENT_ID",
                "NAME",
                "ACTIVE",
                "DESCRIPTION",
                "SORT",
                "LOGOTIP",
                "CURRENCY",
            ],
            'FILTER' => [
                "@ID" => [196, 197, 198],
            ],
            'ORDER' => [
                "SORT" => "ASC",
                "ID" => "DESC",
            ]
        ]
    );
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
   "result":[
      {
         "ID":"14",
         "NAME":"Uber",
         "CODE":"uber",
         "SORT":"250",
         "DESCRIPTION":"Uber Description",
         "SETTINGS":{
            "CALCULATE_URL":"http:\/\/gateway.bx\/calculate.php",
            "CREATE_DELIVERY_REQUEST_URL":"http:\/\/gateway.bx\/create_delivery_request.php",
            "CANCEL_DELIVERY_REQUEST_URL":"http:\/\/gateway.bx\/cancel_delivery_request.php",
            "HAS_CALLBACK_TRACKING_SUPPORT":"Y",
            "CONFIG":[
               {
                  "TYPE":"STRING",
                  "NAME":"String Example",
                  "CODE":"SETTING_1"
               },
               {
                  "TYPE":"Y\/N",
                  "NAME":"Checkbox Example",
                  "CODE":"SETTING_2"
               },
               {
                  "TYPE":"NUMBER",
                  "NAME":"Number Example",
                  "CODE":"SETTING_3"
               },
               {
                  "TYPE":"ENUM",
                  "NAME":"Enum Example",
                  "OPTIONS":{
                     "Option1Code":"Option1Value",
                     "Option2Code":"Option2Value",
                     "Option3Code":"Option3Value",
                     "Option4Code":"Option4Value",
                     "Option5Code":"Option5Value"
                  },
                  "CODE":"SETTING_4"
               },
               {
                  "TYPE":"DATE",
                  "NAME":"Date Example",
                  "CODE":"SETTING_5"
               },
               {
                  "TYPE":"LOCATION",
                  "NAME":"Location Example",
                  "CODE":"SETTING_6"
               }
            ]
         },
         "PROFILES":[
            {
               "NAME":"Taxi",
               "DESCRIPTION":"Taxi Delivery",
               "CODE":"TAXI"
            },
            {
               "NAME":"Cargo",
               "DESCRIPTION":"Cargo Delivery",
               "CODE":"CARGO"
            }
         ]
      }
   ],
   "time":{
      "start":1713872315.334967,
      "finish":1713872315.655173,
      "duration":0.3202061653137207,
      "processing":0.013887882232666016,
      "date_start":"2024-04-23T14:38:35+02:00",
      "date_finish":"2024-04-23T14:38:35+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_delivery_handler[]`](../../data-types.md) | Array of objects with information about the selected delivery service handlers  ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
   "error":"ACCESS_DENIED",
   "error_description":"Access denied!"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient rights to retrieve the list of delivery services | 403 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-handler-add.md)
- [{#T}](./sale-delivery-handler-delete.md)
- [{#T}](./sale-delivery-handler-update.md)