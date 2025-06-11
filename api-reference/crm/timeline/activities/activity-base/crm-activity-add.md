# Add System Activity crm.activity.add

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: user with permission to add an activity

{% note warning "Method development has been halted" %}

The method `crm.activity.add` continues to function, but there is a more relevant alternative [crm.activity.todo.add](../todo/crm-activity-todo-add.md).

{% endnote %}

The method `crm.activity.add` creates a new system activity.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`array`](../../../../data-types.md) | Array of values for [activity fields](#fields) in the form of a structure:

```json
fields:
{
    "OWNER_TYPE_ID": 2, 
    "OWNER_ID": 102, 
    "TYPE_ID": 2, 
    "SUBJECT": "New Call",
}
```
There is an additional field `DISABLE_SENDING_MESSAGE_COPY`. It is intended to forcibly disable sending a copy of the message to the recipient from MESSAGE_FROM. If the parameter is not filled or any value other than `Y` is specified, a copy will be sent. Example:

```js
[
    'fields'=> array(
        'SETTINGS'=> array(
            'DISABLE_SENDING_MESSAGE_COPY'=>'Y'
        )
    )
]
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
[`crm_activity_communication`](../../../data-types.md) | [Description of communication](./crm-activity-communication-fields.md) ||
|| **DEADLINE**
[`datetime`](../../../data-types.md) | Date and time of the activity deadline. The field is not set directly; the value is taken from START_TIME for calls and meetings and from END_TIME for tasks ||
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
[`string`](../../../data-types.md) | Identifier of the entity in the data source, used only for linking to an external source ||
|| **ORIGIN_VERSION**
[`string`](../../../data-types.md) | Original version, used to protect data from accidental overwriting by an external system. If the data was imported and not changed in the external system, such data can be edited in CRM without fear that the next export will lead to data overwriting ||
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
[`char`](../../../data-types.md) | Flag indicating whether the activity was created from an incoming channel or not (`Y`/`N`) ||
|#

### Usage Scenarios for Field Values

For activities of type `e-mail`:
- if the email should not be sent, set the parameters `DIRECTION=2` and `COMPLETED='N'`;
- if it is necessary to mark emails as completed, update the activities by setting the completion flag.

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"OWNER_TYPE_ID":2,"OWNER_ID":102,"TYPE_ID":2,"COMMUNICATIONS":[{"VALUE":"+18005551234","ENTITY_ID":134,"ENTITY_TYPE_ID":3}],"SUBJECT":"New Call","START_TIME":"2023-12-31T12:00:00+00:00","END_TIME":"2023-12-31T12:30:00+00:00","COMPLETED":"N","PRIORITY":3,"RESPONSIBLE_ID":1,"DESCRIPTION":"Important Call","DESCRIPTION_TYPE":3,"DIRECTION":2,"FILES":[{"fileData":["example.jpg","base64_encoded_content_here"]}]} }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"OWNER_TYPE_ID":2,"OWNER_ID":102,"TYPE_ID":2,"COMMUNICATIONS":[{"VALUE":"+18005551234","ENTITY_ID":134,"ENTITY_TYPE_ID":3}],"SUBJECT":"New Call","START_TIME":"2023-12-31T12:00:00+00:00","END_TIME":"2023-12-31T12:30:00+00:00","COMPLETED":"N","PRIORITY":3,"RESPONSIBLE_ID":1,"DESCRIPTION":"Important Call","DESCRIPTION_TYPE":3,"DIRECTION":2,"FILES":[{"fileData":["example.jpg","base64_encoded_content_here"]}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.add
    ```

- JS

    ```javascript
    BX24.callMethod(
        "crm.activity.add",
        {
            fields: {
                "OWNER_TYPE_ID": 2,
                "OWNER_ID": 102,
                "TYPE_ID": 2,
                "COMMUNICATIONS": [
                    { VALUE: "+18005551234", ENTITY_ID: 134, ENTITY_TYPE_ID: 3 }
                ],
                "SUBJECT": "New Call",
                "START_TIME": "2023-12-31T12:00:00+00:00", // Example date and time
                "END_TIME": "2023-12-31T12:30:00+00:00", // Example date and time
                "COMPLETED": "N",
                "PRIORITY": 3,
                "RESPONSIBLE_ID": 1,
                "DESCRIPTION": "Important Call",
                "DESCRIPTION_TYPE": 3,
                "DIRECTION": 2,
                "FILES": [
                    {
                        fileData: [
                            "example.jpg", // File name
                            "base64_encoded_content_here" // File content in base64 format
                        ]
                    }
                ]
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.add',
        [
            'fields' => [
                'OWNER_TYPE_ID' => 2,
                'OWNER_ID' => 102,
                'TYPE_ID' => 2,
                'COMMUNICATIONS' => [
                    [
                        'VALUE' => '+18005551234',
                        'ENTITY_ID' => 134,
                        'ENTITY_TYPE_ID' => 3
                    ]
                ],
                'SUBJECT' => 'New Call',
                'START_TIME' => '2023-12-31T12:00:00+00:00', // Example date and time
                'END_TIME' => '2023-12-31T12:30:00+00:00', // Example date and time
                'COMPLETED' => 'N',
                'PRIORITY' => 3,
                'RESPONSIBLE_ID' => 1,
                'DESCRIPTION' => 'Important Call',
                'DESCRIPTION_TYPE' => 3,
                'DIRECTION' => 2,
                'FILES' => [
                    [
                        'fileData' => [
                            'example.jpg', // File name
                            'base64_encoded_content_here' // File content in base64 format
                        ]
                    ]
                ]
            ]
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo '<PRE>';
        print_r($result['result']);
        echo '</PRE>';
    }
    ```

{% endlist %}    

## Response Handling

HTTP status: **200**

```json
{
    "result": 999,
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
[`boolean`](../../../../data-types.md) | Result of the operation. Returns the identifier of the activity in the timeline in case of success, otherwise — `false` ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

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
|| `The field SUBJECT is not defined or empty` | The `SUBJECT` field is not set ||
|| `The field RESPONSIBLE_ID is not defined or invalid` | The `RESPONSIBLE_ID` field is not set ||
|| `The field TYPE_ID is not defined or invalid` | The `TYPE_ID` field is not set ||
|| `The field COMMUNICATIONS is not defined or invalid` | The `COMMUNICATIONS` field is not set ||
|| `The only one communication is allowed for activity of specified type` | More than one contact is specified ||
|| `Could not build binding. Please ensure that owner info and communications are defined correctly` | Connections for the activity are not specified ||
|| 
- `Email send error. Failed to load module "subscribe"`
- `Email send error. Invalid data`
- `Email send error. Invalid email is specified`
- `Email send error. "From" is not found`
- `Email send error. "To" is not found`
- `Email send error. Failed to add posting. Please see details below`
- `Email send error. Failed to save posting file. Please see details below`
- `Email send error. Failed to update activity`
- `Email send error. General error`
 | Errors related to "email" activities ||
|| `The custom activity without provider is not supported in current context` | Activity type is not supported in the specified context ||
|| `Use crm.activity.configurable.add for this activity provider` | Incorrect method call for configurable activity ||
|| `Access denied` | No permission to add an entity in CRM ||
|| `Application context required` | Incorrect `PROVIDER_ID` parameter for the activity created in the application context ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-activity-update.md)
- [{#T}](./crm-activity-delete.md)
- [{#T}](./crm-activity-get.md)
- [{#T}](./crm-activity-list.md)
- [{#T}](./crm-activity-communication-fields.md)
- [{#T}](./crm-activity-fields.md)