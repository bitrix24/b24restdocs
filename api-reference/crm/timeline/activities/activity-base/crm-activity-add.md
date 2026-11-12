# Add System Activity crm.activity.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify each linked CRM entity

{% note warning "DEPRECATED" %}

Method development has been discontinued. Use [crm.activity.todo.add](../todo/crm-activity-todo-add.md).

{% endnote %}

The `crm.activity.add` method creates a new system activity.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../../data-types.md) | An object containing activity field values [(Detailed Description)](#fields). Example structure:

```json
{
    "fields": {
        "OWNER_TYPE_ID": 2,
        "OWNER_ID": 102,
        "TYPE_ID": 2,
        "COMMUNICATIONS": [{"VALUE": "+499990000000"}],
        "SUBJECT": "New Call"
    }
}
```
The `SETTINGS.DISABLE_SENDING_MESSAGE_COPY` field disables sending a copy of the email to the recipient from `MESSAGE_FROM` when set to `Y`. Example:

```json
{
    "fields": {
        "SETTINGS": {
            "DISABLE_SENDING_MESSAGE_COPY": "Y"
        }
    }
}
```
 ||
|#

### Parameter fields {#fields}

For the full list of fields and their descriptions, see [crm.activity.fields](./crm-activity-fields.md). The fields below include details on how to pass them when creating an activity.

Specify a CRM entity using `OWNER_TYPE_ID` and `OWNER_ID`, or pass a valid link in `COMMUNICATIONS`. Creating an activity requires at least one link to a CRM entity.

The linked CRM entities must exist, and the user must have permission to modify each of them. In the examples, replace `OWNER_TYPE_ID`, `OWNER_ID`, `ENTITY_TYPE_ID`, and `ENTITY_ID` with the types and identifiers of accessible entities. If a communication includes `VALUE` but omits `ENTITY_TYPE_ID` or `ENTITY_ID`, the method uses `OWNER_TYPE_ID` and `OWNER_ID` for it.

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Field** `type` | **Description** ||
|| **OWNER_ID**
[`integer`](../../../../data-types.md) | Identifier of the CRM entity ||
|| **OWNER_TYPE_ID**
[`integer`](../../../../data-types.md) | [Identifier of the CRM object type](../../../data-types.md#object_type) ||
|| **TYPE_ID**
[`crm_enum_activitytype`](../../../data-types.md#activity-enums) | Type of the activity. Required if `PROVIDER_ID` is not specified. If `PROVIDER_ID` is specified but `TYPE_ID` is omitted, the method uses the "provider activity" type. Available values are described in the [Activity Type Reference](../../../data-types.md#activity-enums).

To create an activity with the type "task", use the [creation](../../../../tasks/tasks-task-add.md) or [modification](../../../../tasks/tasks-task-update.md) method and specify the CRM entity in the `UF_CRM_TASK` field ||
|| **COMMUNICATIONS**
[`crm_activity_communication`](../../../data-types.md) | An array of [Communications](./crm-activity-communication-fields.md). Required for calls, meetings, and emails; can be omitted for provider activities. Calls and meetings allow no more than one communication ||
|| **COMPLETED**
[`char`](../../../../data-types.md) | Activity completion flag: `Y` — completed, `N` — not completed ||
|| **DEADLINE**
[`datetime`](../../../../data-types.md) | Date and time of the activity's deadline. The value passed is ignored: the activity provider calculates the deadline, and if it does not set one, `START_TIME` is used ||
|| **DESCRIPTION**
[`string`](../../../../data-types.md) | Text description of the activity ||
|| **DESCRIPTION_TYPE**
[`crm_enum_contenttype`](../../../data-types.md#activity-enums) | Description type ||
|| **DIRECTION**
[`crm_enum_activitydirection`](../../../data-types.md#activity-enums) | Direction of the activity: incoming/outgoing. Relevant for calls and emails, not used for meetings ||
|| **END_TIME**
[`datetime`](../../../../data-types.md) | Activity completion time ||
|| **FILES**
[`diskfile`](../../../data-types.md) | Files added to the activity ||
|| **LOCATION**
[`string`](../../../../data-types.md) | Location ||
|| **NOTIFY_TYPE**
[`crm_enum_activitynotifytype`](../../../data-types.md#activity-enums) | Time unit for the reminder: `1` — minutes, `2` — hours, `3` — days, `0` — no reminder set ||
|| **NOTIFY_VALUE**
[`integer`](../../../../data-types.md) | Number of `NOTIFY_TYPE` units for the reminder. Required if `NOTIFY_TYPE` is greater than `0` ||
|| **ORIGINATOR_ID**
[`string`](../../../../data-types.md) | Identifier of the data source, used only for linking to an external source ||
|| **ORIGIN_ID**
[`string`](../../../../data-types.md) | Identifier of the entity in the data source, used only for linking to an external source ||
|| **PRIORITY**
[`crm_enum_activitypriority`](../../../data-types.md#activity-enums) | Priority ||
|| **PROVIDER_DATA**
[`string`](../../../../data-types.md) | Additional provider data ||
|| **PROVIDER_GROUP_ID**
[`string`](../../../../data-types.md) | Identifier of the provider group ||
|| **PROVIDER_ID**
[`string`](../../../../data-types.md) | Identifier of the provider ||
|| **PROVIDER_TYPE_ID**
[`string`](../../../../data-types.md) | Identifier of the provider type, status from the directory ||
|| **PROVIDER_PARAMS**
[`object`](../../../../data-types.md) | Additional provider parameters ||
|| **RESPONSIBLE_ID**
[`user`](../../../../data-types.md) | Identifier of the user responsible for the activity. If omitted, the method tries to use the user responsible for the CRM entity, then the current user ||
|| **SETTINGS**
[`object`](../../../../data-types.md) | Additional settings ||
|| **START_TIME**
[`datetime`](../../../../data-types.md) | Time when the activity starts ||
|| **STATUS**
[`crm_enum_activitystatus`](../../../data-types.md#activity-enums) | Status of the activity ||
|| **SUBJECT***
[`string`](../../../../data-types.md) | Activity subject. This field is required and cannot be empty ||
|| **WEBDAV_ELEMENTS**
[`diskfile`](../../../data-types.md) | Added files. For Drive storage, this field is deprecated and retained for compatibility; pass files in `FILES`. For WebDAV storage, pass objects with `ELEMENT_ID` ||
|#

### Field Value Usage Options

For activities of type `e-mail`:
- if the e-mail should not be sent, set the `DIRECTION=2` and `COMPLETED='N'` parameters
- to mark the e-mail as completed, call the [crm.activity.update](./crm-activity-update.md) method and pass `COMPLETED='Y'`

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"OWNER_TYPE_ID":2,"OWNER_ID":102,"TYPE_ID":2,"COMMUNICATIONS":[{"VALUE":"+499832322323","ENTITY_ID":134,"ENTITY_TYPE_ID":3}],"SUBJECT":"New call","START_TIME":"2023-12-31T12:00:00+00:00","END_TIME":"2023-12-31T12:30:00+00:00","COMPLETED":"N","PRIORITY":3,"RESPONSIBLE_ID":1,"DESCRIPTION":"Important call","DESCRIPTION_TYPE":3,"DIRECTION":2,"FILES":[{"fileData":["example.jpg","base64_encoded_content_here"]}]} }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"OWNER_TYPE_ID":2,"OWNER_ID":102,"TYPE_ID":2,"COMMUNICATIONS":[{"VALUE":"+499832322323","ENTITY_ID":134,"ENTITY_TYPE_ID":3}],"SUBJECT":"New call","START_TIME":"2023-12-31T12:00:00+00:00","END_TIME":"2023-12-31T12:30:00+00:00","COMPLETED":"N","PRIORITY":3,"RESPONSIBLE_ID":1,"DESCRIPTION":"Important call","DESCRIPTION_TYPE":3,"DIRECTION":2,"FILES":[{"fileData":["example.jpg","base64_encoded_content_here"]}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'crm.activity.add',
        params: {
          fields: {
            OWNER_TYPE_ID: 2,
            OWNER_ID: 102,
            TYPE_ID: 2,
            COMMUNICATIONS: [
              { VALUE: '+499832322323', ENTITY_ID: 134, ENTITY_TYPE_ID: 3 },
            ],
            SUBJECT: 'New call',
            START_TIME: '2023-12-31T12:00:00+00:00',
            END_TIME: '2023-12-31T12:30:00+00:00',
            COMPLETED: 'N',
            PRIORITY: 3,
            RESPONSIBLE_ID: 1,
            DESCRIPTION: 'Important call',
            DESCRIPTION_TYPE: 3,
            DIRECTION: 2,
            FILES: [
              {
                fileData: [
                  'example.jpg',
                  'base64_encoded_content_here',
                ],
              },
            ],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created activity ID:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addActivity() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.add',
            params: {
              fields: {
                OWNER_TYPE_ID: 2,
                OWNER_ID: 102,
                TYPE_ID: 2,
                COMMUNICATIONS: [
                  { VALUE: '+499832322323', ENTITY_ID: 134, ENTITY_TYPE_ID: 3 },
                ],
                SUBJECT: 'New call',
                START_TIME: '2023-12-31T12:00:00+00:00',
                END_TIME: '2023-12-31T12:30:00+00:00',
                COMPLETED: 'N',
                PRIORITY: 3,
                RESPONSIBLE_ID: 1,
                DESCRIPTION: 'Important call',
                DESCRIPTION_TYPE: 3,
                DIRECTION: 2,
                FILES: [
                  {
                    fileData: [
                      'example.jpg',
                      'base64_encoded_content_here',
                    ],
                  },
                ],
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created activity ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addActivity)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.add(
            fields={
                "OWNER_TYPE_ID": 2,
                "OWNER_ID": 102,
                "TYPE_ID": 2,
                "COMMUNICATIONS": [
                    {"VALUE": "+499832322323", "ENTITY_ID": 134, "ENTITY_TYPE_ID": 3},
                ],
                "SUBJECT": "New call",
                "START_TIME": "2023-12-31T12:00:00+00:00",
                "END_TIME": "2023-12-31T12:30:00+00:00",
                "COMPLETED": "N",
                "RESPONSIBLE_ID": 1,
                "PRIORITY": 3,
                "DESCRIPTION": "Important call",
                "DESCRIPTION_TYPE": 3,
                "DIRECTION": 2,
                "FILES": [
                    {"fileData": ["example.jpg", "base64_encoded_content_here"]},
                ],
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.add',
                [
                    'fields' => [
                        'OWNER_TYPE_ID'    => 2,
                        'OWNER_ID'         => 102,
                        'TYPE_ID'          => 2,
                        'COMMUNICATIONS'   => [
                            ['VALUE' => '+499832322323', 'ENTITY_ID' => 134, 'ENTITY_TYPE_ID' => 3]
                        ],
                        'SUBJECT'          => 'New call',
                        'START_TIME'       => '2023-12-31T12:00:00+00:00',
                        'END_TIME'         => '2023-12-31T12:30:00+00:00',
                        'COMPLETED'        => 'N',
                        'PRIORITY'         => 3,
                        'RESPONSIBLE_ID'   => 1,
                        'DESCRIPTION'      => 'Important call',
                        'DESCRIPTION_TYPE' => 3,
                        'DIRECTION'        => 2,
                        'FILES'            => [
                            [
                                'fileData' => [
                                    'example.jpg',
                                    'base64_encoded_content_here'
                                ]
                            ]
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // The data processing logic you need
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding activity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        "crm.activity.add",
        {
            fields: {
                "OWNER_TYPE_ID": 2,
                "OWNER_ID": 102,
                "TYPE_ID": 2,
                "COMMUNICATIONS": [
                    { VALUE: "+499832322323", ENTITY_ID: 134, ENTITY_TYPE_ID: 3 }
                ],
                "SUBJECT": "New call",
                "START_TIME": "2023-12-31T12:00:00+00:00", // Example date and time
                "END_TIME": "2023-12-31T12:30:00+00:00", // Example date and time
                "COMPLETED": "N",
                "PRIORITY": 3,
                "RESPONSIBLE_ID": 1,
                "DESCRIPTION": "Important call",
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

- PHP CRest

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
                        'VALUE' => '+499832322323',
                        'ENTITY_ID' => 134,
                        'ENTITY_TYPE_ID' => 3
                    ]
                ],
                'SUBJECT' => 'New call',
                'START_TIME' => '2023-12-31T12:00:00+00:00', // Example date and time
                'END_TIME' => '2023-12-31T12:30:00+00:00', // Example date and time
                'COMPLETED' => 'N',
                'PRIORITY' => 3,
                'RESPONSIBLE_ID' => 1,
                'DESCRIPTION' => 'Important call',
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.add", b24.Params{
    	"fields": b24.Params{
    		"OWNER_TYPE_ID": 2,
    		"OWNER_ID":      102,
    		"TYPE_ID":       2,
    		"COMMUNICATIONS": []b24.Params{
    			{
    				"VALUE":          "+499832322323",
    				"ENTITY_ID":      134,
    				"ENTITY_TYPE_ID": 3,
    			},
    		},
    		"SUBJECT":          "New Call",
    		"START_TIME":       "2023-12-31T12:00:00+00:00",
    		"END_TIME":         "2023-12-31T12:30:00+00:00",
    		"COMPLETED":        "N",
    		"PRIORITY":         3,
    		"RESPONSIBLE_ID":   1,
    		"DESCRIPTION":      "Important call",
    		"DESCRIPTION_TYPE": 3,
    		"DIRECTION":        2,
    		"FILES": []b24.Params{
    			{
    				"fileData": []string{"example.jpg", "base64_encoded_content_here"},
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
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
[`integer`](../../../../data-types.md) | Activity identifier in the timeline ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

When creating an outgoing completed email (`TYPE_ID=4`, `COMPLETED=Y`, `DIRECTION=2`), the method first saves the activity, then sends the email. If sending fails with an `Email send error. ...` error, the activity has already been created. Before calling the method again, check the [Activity List](./crm-activity-list.md) to avoid creating a duplicate.

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Meaning** ||
|| `400` | Empty value | `The field SUBJECT is not defined or empty.` | The `SUBJECT` field is empty ||
|| `400` | Empty value | `The field RESPONSIBLE_ID is not defined or invalid.` | The user responsible for the activity could not be determined ||
|| `400` | Empty value | `The field TYPE_ID is not defined or invalid.` | The `TYPE_ID` field is missing or contains an invalid value ||
|| `400` | Empty value | `The field COMMUNICATIONS is not defined or invalid.` | No valid communications were provided for the call, meeting, or email ||
|| `400` | Empty value | `The only one communication is allowed for activity of specified type.` | More than one communication was provided for a call or meeting ||
|| `400` | Empty value | `Could not build binding. Please ensure that owner info and communications are defined correctly.` | A link to a CRM entity could not be built from the owner and communications ||
|| `400` | Empty value | `Entity type '<type>' is not supported in current context` | The linked CRM entity type is not supported; `<type>` is the type name or its identifier in square brackets ||
|| `400` | Empty value | `Could not find '<type>' with ID: <ID>` | The linked CRM entity was not found; `<type>` is the type name, `<ID>` is the entity identifier ||
|| `400` | Empty value | `The activity type "<type> is not supported in current context".` | The activity type is not supported; `<type>` is the activity type name ||
|| `400` | Empty value | `Email send error. Failed to load module "subscribe".` | The subscribe module could not be loaded ||
|| `400` | Empty value | `Email send error. Invalid data.` | Invalid email data ||
|| `400` | Empty value | `Email send error. Invalid email is specified.` | An invalid email address was specified ||
|| `400` | Empty value | `Email send error. "From" is not found.` | The email sender was not found ||
|| `400` | Empty value | `Email send error. "To" is not found.` | The email recipient was not found ||
|| `400` | Empty value | `Email send error. Failed to add posting. Please see details below.` | The mailing could not be created; details are provided in the error message ||
|| `400` | Empty value | `Email send error. Failed to save posting file. Please see details below.` | The mailing file could not be saved; details are provided in the error message ||
|| `400` | Empty value | `Email send error. Failed to update activity.` | The activity could not be updated when sending the email ||
|| `400` | Empty value | `Email send error. General error.` | General email sending error ||
|| `400` | Empty value | `The custom activity without provider is not supported in current context.` | No provider was found for the provider activity ||
|| `400` | Empty value | `Use crm.activity.configurable.add for this activity provider` | Use `crm.activity.configurable.add` for configurable activities ||
|| `400` | Empty value | `Access denied.` | No permission to modify the linked CRM entity ||
|| `400` | Empty value | `Application context required.` | An activity with an application provider can only be created in the application context ||
|| `400` | `ERROR_CORE` | `NOTIFY_VALUE is not assigned.<br>` | `NOTIFY_VALUE` was not provided for the selected notification type ||
|#

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-update.md)
- [{#T}](./crm-activity-get.md)
- [{#T}](./crm-activity-list.md)
- [{#T}](./crm-activity-delete.md)
- [{#T}](./crm-activity-communication-fields.md)
- [{#T}](./crm-activity-fields.md)
- [{#T}](../../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-activity-to-contact.md)
- [{#T}](../../../../../tutorials/crm/how-to-add-crm-objects/how-to-send-email.md)
