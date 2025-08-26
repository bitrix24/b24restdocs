# Update System Activity crm.activity.update

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user with permission to update the activity

{% note warning "Method development has been halted" %}

The method `crm.activity.update` continues to function, but there is a more current equivalent [crm.activity.todo.update](../todo/crm-activity-todo-update.md).

{% endnote %}

The method `crm.activity.update` updates an existing activity.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Integer identifier of the activity in the timeline, for example `999` ||
|| **fields***
[`array`](../../../../data-types.md) | Array of values for [activity fields](#fields) in the following structure:

```json
fields:
{
    "OWNER_TYPE_ID": 2, 
    "OWNER_ID": 102, 
    "TYPE_ID": 2, 
    "SUBJECT": "New call",
}
```
||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Field** `type` | **Description** ||
|| **OWNER_ID***
[`integer`](../../../data-types.md) | Identifier of the CRM entity ||
|| **OWNER_TYPE_ID***
[`integer`](../../../data-types.md) | [Identifier of the CRM object type](../../../data-types.md#object_type) ||
|| **TYPE_ID***
[`crm_enum_activitytype`](../../../data-types.md) | Type of activity ||
|| **ASSOCIATED_ENTITY_ID**
[`integer`](../../../../data-types.md) | Identifier of the entity associated with the activity ||
|| **COMMUNICATIONS***
[`crm_activity_communication`](../../../data-types.md) | [Description of the communication](./crm-activity-communication-fields.md) ||
|| **DEADLINE**
[`datetime`](../../../data-types.md) | Date and time of the activity deadline. This field is not set directly; the value is taken from START_TIME for calls and meetings and from END_TIME for tasks ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Text description of the activity ||
|| **DESCRIPTION_TYPE**
[`crm.enum.contenttype`](../../../data-types.md) | Type of description ||
|| **DIRECTION**
[`crm.enum.activitydirection`](../../../data-types.md) | Direction of the activity: incoming/outgoing. Relevant for calls and emails, not used for meetings ||
|| **END_TIME**
[`datetime`](../../../data-types.md) | End time of the activity | ||
|| **FILES**
[`diskfile`](../../../data-types.md) | Files added to the activity ||
|| **LOCATION**
[`string`](../../../data-types.md) | Location ||
|| **NOTIFY_TYPE**
[`crm.enum.activitynotifytype`](../../../data-types.md) | Type of notification ||
|| **ORIGINATOR_ID**
[`string`](../../../data-types.md) | Identifier of the data source, used only for linking to an external source ||
|| **ORIGIN_ID**
[`string`](../../../data-types.md) | Identifier of the item in the data source, used only for linking to an external source ||
|| **ORIGIN_VERSION**
[`string`](../../../data-types.md) | Original version, used to protect data from accidental overwriting by an external system. If the data was imported and not changed in the external system, it can be edited in CRM without fear that the next export will overwrite the data ||
|| **PRIORITY**
[`crm.enum.activitypriority`](../../../data-types.md) | Priority ||
|| **PROVIDER_DATA**
[`string`](../../../data-types.md) | Additional provider data ||
|| **PROVIDER_GROUP_ID**
[`string`](../../../data-types.md) | Identifier of the provider group ||
|| **PROVIDER_ID**
[`string`](../../../data-types.md) | Identifier of the provider ||
|| **PROVIDER_TYPE_ID**
[`string`](../../../data-types.md) | Identifier of the provider type, status from the directory ||
|| **PROVIDER_PARAMS**
[`object`](../../../data-types.md) | Additional provider parameters ||
|| **RESPONSIBLE_ID***
[`user`](../../../data-types.md) | Identifier of the user responsible for the activity ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Additional settings ||
|| **START_TIME**
[`datetime`](../../../data-types.md) | Start time of the activity ||
|| **STATUS**
[`crm_enum_activitystatus`](../../../data-types.md) | Status of the activity ||
|| **SUBJECT**
[`string`](../../../data-types.md) | Additional description of the activity ||
|| **WEBDAV_ELEMENTS**
[`diskfile`](../../../data-types.md) | Added files. Deprecated, kept for compatibility ||
|| **IS_INCOMING_CHANNEL**
[`char`](../../../data-types.md) | Flag indicating whether the activity was created from an incoming channel (`Y`/`N`) ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"fields":{"RESPONSIBLE_ID":1,"DESCRIPTION":"New activity description"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"fields":{"RESPONSIBLE_ID":1,"DESCRIPTION":"New activity description"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.activity.update',
    		{
    			id: 999,
    			fields: {
    				"RESPONSIBLE_ID": 1, 
    				"DESCRIPTION": "New activity description"
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
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
                'crm.activity.update',
                [
                    'id' => 999,
                    'fields' => [
                        "RESPONSIBLE_ID" => 1,
                        "DESCRIPTION" => "New activity description"
                    ]
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
        echo 'Error updating activity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.activity.update',
        {
            id: 999,
            fields: {
                "RESPONSIBLE_ID": 1, 
                "DESCRIPTION": "New activity description"
            }
        },
        result => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.update',
        [
            'id' => 999,
            'fields' => [
                'RESPONSIBLE_ID' => 1,
                'DESCRIPTION' => 'New activity description'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

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
[`boolean`](../../../../data-types.md) | Result of the operation. Returns `true` if the activity was successfully updated, otherwise `false` ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `Activity is not found` | The activity with the specified identifier was not found for the entity in CRM ||
|| `The field SUBJECT is not defined or empty` | The `SUBJECT` field is not set ||
|| `The field RESPONSIBLE_ID is not defined or invalid` | The `RESPONSIBLE_ID` field is not set ||
|| `The field TYPE_ID is not defined or invalid` | The `TYPE_ID` field is not set ||
|| `The field COMMUNICATIONS is not defined or invalid` | The `COMMUNICATIONS` field is not set ||
|| `The only one communication is allowed for activity of specified type` | More than one contact is allowed ||
|| `Could not build binding. Please ensure that owner info and communications are defined correctly` | Connections for the activity are not specified ||
|| `The custom activity without provider is not supported in current context` | The activity type is not supported in the given context ||
|| `Use crm.activity.configurable.update for this activity provider` | Incorrect method call for configurable activity ||
|| `Access denied` | No permission to update the entity in CRM ||
|| `Application context required` | Incorrect `PROVIDER_ID` parameter for the activity created in the application context ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-activity-add.md)
- [{#T}](./crm-activity-delete.md)
- [{#T}](./crm-activity-get.md)
- [{#T}](./crm-activity-list.md)
- [{#T}](./crm-activity-communication-fields.md)
- [{#T}](./crm-activity-fields.md)