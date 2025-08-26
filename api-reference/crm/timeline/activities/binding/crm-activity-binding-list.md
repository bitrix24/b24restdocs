# Get a list of all bindings for the activity crm.activity.binding.list

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

The method `crm.activity.binding.list` retrieves a list of all bindings for the activity.

The method will return an array, where each element will be an array containing:

- `entityTypeId` — an integer identifier of the [CRM object type](../../../data-types.md#object_type)
- `entityId` — an integer identifier of the CRM entity

The result will only include elements that the current user has read access to.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **activityId***
[`integer`](../../../../data-types.md) | Integer identifier of the activity in the timeline, for example `999` ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":999}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.binding.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":999,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.binding.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'crm.activity.binding.list',
        {
          activityId: 999 // Deal ID
        }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative fetching using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.activity.binding.list', { activityId: 999 }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
      const response = await $b24.callMethod('crm.activity.binding.list', { activityId: 999 }, 0)
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
                'crm.activity.binding.list',
                [
                    'activityId' => 999, // Deal ID
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Result: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.activity.binding.list',
        {
            activityId: 999 // Activity ID
        },
        function(result) {
            if (result.error()) {
                console.error('Error:', result.error()); 
            } else {
                console.log('Result:', result.data()); 
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.binding.list',
        [
            'activityId' => 999 // Activity ID
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
    "result":[
        {
            "entityTypeId": 1,
            "entityId": 123
        },
        {
            "entityTypeId": 2,
            "entityId": 456
        },
        {
            "entityTypeId": 3,
            "entityId": 789
        }
    ],
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../../data-types.md) | The result of the operation. Returns an array, where each element will be an array containing:

- `entityTypeId` — an integer identifier of the [CRM object type](../../../data-types.md#object_type)
- `entityId` — an integer identifier of the CRM entity
||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Element not found"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `100` | Required fields not provided ||
|| `ACCESS_DENIED` | Insufficient permissions to perform the operation ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-activity-binding-add.md)
- [{#T}](./crm-activity-binding-delete.md)
- [{#T}](./crm-activity-binding-move.md)