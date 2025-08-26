# Update the activity's connection with the CRM entity crm.activity.binding.move

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

The method `crm.activity.binding.move` updates the connection of an activity with a CRM entity. The update is only possible if the current user has edit access to the CRM entities they are modifying the connections for.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **activityId***
[`integer`](../../../../data-types.md) | Identifier of the activity in the timeline, for example `999` ||
|| **sourceEntityTypeId***
[`integer`](../../../../data-types.md) | [Identifier of the CRM object type](../../../data-types.md#object_type) to which the activity is linked, for example `2` for an activity ||
|| **sourceEntityId***
[`integer`](../../../../data-types.md) | Identifier of the CRM entity to which the activity is linked, for example `1` ||
|| **targetEntityTypeId***
[`integer`](../../../../data-types.md) | [Identifier of the CRM object type](../../../data-types.md#object_type) to which the activity needs to be linked, for example `2` for an activity ||
|| **targetEntityId***
[`integer`](../../../../data-types.md) | Identifier of the CRM entity to which the activity needs to be linked, for example `100` ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":999, "sourceEntityTypeId":2, "sourceEntityId": 1, "targetEntityTypeId":2, "targetEntityId": 100}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.binding.move
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":999,"sourceEntityTypeId":2,"sourceEntityId":1,"targetEntityTypeId":2,"targetEntityId":100,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.binding.move
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.activity.binding.move',
    		{
    			activityId: 999, // ID of the activity
    			sourceEntityTypeId: 2, // Type of the object to which the activity is linked
    			sourceEntityId: 1, // ID of the entity to which the activity is linked
    			targetEntityTypeId: 2, // Type of the object to which the activity will be linked
    			targetEntityId: 100 // ID of the entity to which the activity will be linked
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
                'crm.activity.binding.move',
                [
                    'activityId'         => 999,
                    'sourceEntityTypeId' => 2,
                    'sourceEntityId'     => 1,
                    'targetEntityTypeId' => 2,
                    'targetEntityId'     => 100,
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
        echo 'Error moving activity binding: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.activity.binding.move',
        {
            activityId: 999, // ID of the activity
            sourceEntityTypeId: 2, // Type of the object to which the activity is linked
            sourceEntityId: 1, // ID of the entity to which the activity is linked
            targetEntityTypeId: 2, // Type of the object to which the activity will be linked
            targetEntityId: 100 // ID of the entity to which the activity will be linked
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
        'crm.activity.binding.move',
        [
            'activityId' => 999, // ID of the activity
            'sourceEntityTypeId' => 2, // Type of the object to which the activity is linked
            'sourceEntityId' => 1, // ID of the entity to which the activity is linked
            'targetEntityTypeId' => 2, // Type of the object to which the activity will be linked
            'targetEntityId' => 100 // ID of the entity to which the activity will be linked
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
[`boolean`](../../../../data-types.md) | Result of the operation. Returns `true` if the connection was successfully changed, otherwise `false` ||
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
|| `NOT_FOUND` | Element not found ||
|| `OWNER_NOT_FOUND` | Owner of the element not found ||
|| `SOURCE_AND_TARGET_ENTITY_TYPES_ARE_NOT_EQUAL` | Cannot move the activity from one CRM object type to another ||
|| `SOURCE_AND_TARGET_ENTITY_ID_ARE_EQUAL_ERROR` | Cannot move the activity to the same activity ||
|| `ACCESS_DENIED` | Insufficient permissions to perform the operation ||
|| `ACTIVITY_IS_ALREADY_BOUND` | The activity is already linked to this entity ||
|| `BINDING_NOT_FOUND` | The activity is not linked to the specified entity ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-activity-binding-list.md)
- [{#T}](./crm-activity-binding-delete.md)
- [{#T}](./crm-activity-binding-add.md)
- [{#T}](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity.md)
- [{#T}](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity-between-objects.md)