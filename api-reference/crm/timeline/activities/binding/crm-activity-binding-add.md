# Add a deal binding with a CRM entity crm.activity.binding.add

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

The method `crm.activity.binding.add` establishes a binding between a deal and a CRM entity. A deal can only be bound to an entity that the current user has edit access to.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **activityId*** 
[`integer`](../../../../data-types.md) | Integer identifier of the deal in the timeline, for example `999` ||
|| **entityTypeId*** 
[`integer`](../../../../data-types.md) | [Integer identifier of the CRM object type](../../../data-types.md#object_type) to which the deal should be bound, for example `2` for a deal ||
|| **entityId*** 
[`integer`](../../../../data-types.md) | Integer identifier of the CRM entity to which the deal should be bound, for example `1` ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":999, "entityTypeId":2, "entityId": 1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.binding.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":999, "entityTypeId":2, "entityId": 1, "auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.binding.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.activity.binding.add',
    		{
    			activityId: 999, // Deal ID
    			entityTypeId: 2, // CRM object type ID
    			entityId: 1 // CRM entity ID
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log('Result:', result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.binding.add',
                [
                    'activityId'   => 999, // Deal ID
                    'entityTypeId' => 2, // CRM object type ID
                    'entityId'     => 1 // CRM entity ID
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding activity binding: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.activity.binding.add',
        {
            activityId: 999, // Deal ID
            entityTypeId: 2, // CRM object type ID
            entityId: 1 // CRM entity ID
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
        'crm.activity.binding.add',
        [
            'activityId' => 999, // Deal ID
            'entityTypeId' => 2, // CRM object type ID
            'entityId' => 1 // CRM entity ID
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
    "result": true,
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
[`boolean`](../../../../data-types.md) | Result of the operation. Returns `true` if the binding was successfully created, otherwise `false` ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Entity not found"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `100` | Required fields not provided ||
|| `NOT_FOUND` | Entity not found ||
|| `OWNER_NOT_FOUND` | Owner of the entity not found ||
|| `ACCESS_DENIED` | Insufficient permissions to perform the operation ||
|| `ACTIVITY_IS_ALREADY_BOUND` | The deal is already bound to this entity ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-activity-binding-list.md)
- [{#T}](./crm-activity-binding-delete.md)
- [{#T}](./crm-activity-binding-move.md)
- [{#T}](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity-between-objects.md)