# Get a list of fields for crm.activity.fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: users with permission to read CRM items or digital workspaces

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method `crm.activity.field.list` from the [Activities](../../../rest-v3/activities/index.md) group. The REST 3.0 method does not return some activity fields: the settings `settings`, the provider data `providerParams`, `location`, `status`, `priority`, and others. If you need them, use this method.

{% endnote %}

The method `crm.activity.fields` returns the description of the fields of the system activity.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
     curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.fields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ActivityFieldsResult = Record<string, ActivityField>

    type ActivityField = {
      type: string
      isRequired: boolean
      isReadOnly: boolean
      isImmutable: boolean
      isMultiple: boolean
      isDynamic: boolean
      title: string
      isDeprecated?: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<ActivityFieldsResult>({
        method: 'crm.activity.fields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Activity field names:', Object.keys(result))
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
      async function getActivityFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.fields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Activity field names:', Object.keys(result))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getActivityFields)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.fields().response
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
                'crm.activity.fields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.activity.fields',
        {},
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.fields", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.activity.fields: %w", err)
    }

    keys, ok := b24.Keys(res.Result)
    if !ok {
    	return fmt.Errorf("expected an object in the response")
    }
    fmt.Println("fields in response:", len(keys))
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "ID"
        },
        "OWNER_ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Owner ID"
        },
        "OWNER_TYPE_ID": {
            "type": "crm_enum_ownertype",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Owner Type"
        },
        "TYPE_ID": {
            "type": "crm_enum_activitytype",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Type"
        },
        "PROVIDER_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Provider ID"
        },
        "PROVIDER_TYPE_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Provider Type"
        },
        "PROVIDER_GROUP_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Connector Type"
        },
        "ASSOCIATED_ENTITY_ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Associated Entity ID"
        },
        "SUBJECT": {
            "type": "string",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Subject"
        },
        "START_TIME": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Start"
        },
        "END_TIME": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Deadline"
        },
        "DEADLINE": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Execution Deadline"
        },
        "COMPLETED": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Completed"
        },
        "STATUS": {
            "type": "crm_enum_activitystatus",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Status"
        },
        "RESPONSIBLE_ID": {
            "type": "user",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Responsible"
        },
        "PRIORITY": {
            "type": "crm_enum_activitypriority",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Priority"
        },
        "NOTIFY_TYPE": {
            "type": "crm_enum_activitynotifytype",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Notification Type"
        },
        "NOTIFY_VALUE": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Notification Parameter"
        },
        "DESCRIPTION": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Description"
        },
        "DESCRIPTION_TYPE": {
            "type": "crm_enum_contenttype",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Description Type"
        },
        "DIRECTION": {
            "type": "crm_enum_activitydirection",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Direction"
        },
        "LOCATION": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Location"
        },
        "CREATED": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Creation Date"
        },
        "AUTHOR_ID": {
            "type": "user",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Created By"
        },
        "LAST_UPDATED": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Last Updated Date"
        },
        "EDITOR_ID": {
            "type": "user",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Edited By"
        },
        "SETTINGS": {
            "type": "object",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Settings"
        },
        "ORIGIN_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "External Code"
        },
        "ORIGINATOR_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "External Source"
        },
        "RESULT_STATUS": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RESULT_STATUS"
        },
        "RESULT_STREAM": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RESULT_STREAM"
        },
        "RESULT_SOURCE_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RESULT_SOURCE_ID"
        },
        "PROVIDER_PARAMS": {
            "type": "object",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Provider Parameters"
        },
        "PROVIDER_DATA": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Provider Data"
        },
        "RESULT_MARK": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RESULT_MARK"
        },
        "RESULT_VALUE": {
            "type": "double",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RESULT_VALUE"
        },
        "RESULT_SUM": {
            "type": "double",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RESULT_SUM"
        },
        "RESULT_CURRENCY_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RESULT_CURRENCY_ID"
        },
        "AUTOCOMPLETE_RULE": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Autocomplete"
        },
        "BINDINGS": {
            "type": "crm_activity_binding",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": true,
            "isDynamic": false,
            "title": "Bindings"
        },
        "COMMUNICATIONS": {
            "type": "crm_activity_communication",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": true,
            "isDynamic": false,
            "title": "Communication Channel"
        },
        "FILES": {
            "type": "diskfile",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": true,
            "isDynamic": false,
            "title": "Files"
        },
        "WEBDAV_ELEMENTS": {
            "type": "diskfile",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": true,
            "isDynamic": false,
            "isDeprecated": true,
            "title": "Added Files"
        },
        "IS_INCOMING_CHANNEL": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "IS_INCOMING_CHANNEL"
        }
    },
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
[`object`](../../../../data-types.md) | An object whose keys are [Activity Field Names](#all-fields) and whose values are [Field Metadata Objects](#field-metadata) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Field Metadata {#field-metadata}

#|
|| **Name** `type` | **Description** ||
|| **type** `string` | Field value type ||
|| **isRequired** `boolean` | Indicates whether the field is required in the metadata. Actual requirements when creating an activity depend on the activity type and whether a value can be assigned automatically ||
|| **isReadOnly** `boolean` | The field is read-only. Any value passed when creating or updating an activity is ignored ||
|| **isImmutable** `boolean` | Indicates whether the field is immutable after creation ||
|| **isMultiple** `boolean` | The field supports multiple values ||
|| **isDynamic** `boolean` | Indicates whether the field is dynamic ||
|| **title** `string` | Field name ||
|| **isDeprecated** `boolean` | Indicates whether the field is deprecated. Returned only for deprecated fields with the value `true` ||
|#

#### Overview of System Activity Fields {#all-fields}

An asterisk marks fields with `isRequired: true` in the metadata. This is not a list of parameters that must be passed in every call. When [Creating an Activity](./crm-activity-add.md), the clarifications in the notes below apply. When [Updating an Activity](./crm-activity-update.md), pass only the fields to be changed.

The set of file fields depends on the storage. The table and response example are for Drive: `FILES` is returned, and `WEBDAV_ELEMENTS` is marked as deprecated. For WebDAV storage, `FILES` is absent, and `WEBDAV_ELEMENTS` has the type `webdav` without the `isDeprecated` flag.

#|
|| **Field** `type` | **Description** | **Note** ||
|| **ID**
[`integer`](../../../../data-types.md) | Activity ID | Read-only ||
|| **OWNER_ID**
[`integer`](../../../../data-types.md) | CRM object ID | Together with `OWNER_TYPE_ID`, defines the binding when creating an activity. The binding can also be built from `COMMUNICATIONS`. Can be changed using the [crm.activity.binding.move](../binding/crm-activity-binding-move.md) method ||
|| **OWNER_TYPE_ID**
[`crm_enum_ownertype`](../../../data-types.md#object_type) | [CRM object type ID](../../../data-types.md#object_type) | Immutable according to the metadata. Together with `OWNER_ID`, defines the binding when creating an activity if it is not built from `COMMUNICATIONS` ||
|| **TYPE_ID***
[`crm_enum_activitytype`](../../../data-types.md#activity-enums) | Activity type | Immutable. Can be omitted when creating an activity if `PROVIDER_ID` is specified: `6` is then assigned for a provider activity ||
|| **ASSOCIATED_ENTITY_ID**
[`integer`](../../../../data-types.md) | Integer ID of the associated object | Read-only ||
|| **AUTHOR_ID**
[`user`](../../../../data-types.md) | Integer ID of the user who created the activity | Read-only ||
|| **AUTOCOMPLETE_RULE**
[`integer`](../../../../data-types.md) | Integer ID of the rule that triggered the autocomplete | ||
|| **BINDINGS**
[`crm_activity_binding`](../../../data-types.md) | Bindings to CRM entities | Multiple, read-only ||
|| **COMMUNICATIONS***
[`crm_activity_communication`](../../../data-types.md) | [Description of communication](./crm-activity-communication-fields.md) | Multiple. Required when creating calls, meetings, and emails; can be omitted for a provider activity if a binding is specified through `OWNER_ID` and `OWNER_TYPE_ID` ||
|| **COMPLETED**
[`char`](../../../../data-types.md) | Activity completion flag: `Y` — completed, `N` — not completed | Optional ||
|| **CREATED**
[`datetime`](../../../../data-types.md) | Date and time the activity was created | Read-only ||
|| **DEADLINE**
[`datetime`](../../../../data-types.md) | Date and time of the activity's execution deadline | Any value passed when creating or updating an activity is removed. The provider calculates the deadline. When creating an activity, if the provider has not set a deadline, `START_TIME` is used, or the maximum database date if it is absent. The maximum database date is used for activities with `IS_INCOMING_CHANNEL = Y` ||
|| **DESCRIPTION**
[`string`](../../../../data-types.md) | Text description of the activity | ||
|| **DESCRIPTION_TYPE**
[`crm_enum_contenttype`](../../../data-types.md#activity-enums) | Description type | ||
|| **DIRECTION**
[`crm_enum_activitydirection`](../../../data-types.md#activity-enums) | Direction of the activity: incoming or outgoing | Relevant for calls and emails, not used for meetings ||
|| **EDITOR_ID**
[`user`](../../../../data-types.md) | Integer ID of the user who edited the activity | Read-only ||
|| **END_TIME**
[`datetime`](../../../../data-types.md) | Time the activity ended | ||
|| **FILES**
[`diskfile`](../../../data-types.md) | Files added to the activity | Multiple ||
|| **LAST_UPDATED**
[`datetime`](../../../../data-types.md) | Date of the last update | Read-only ||
|| **LOCATION**
[`string`](../../../../data-types.md) | Location | ||
|| **NOTIFY_TYPE**
[`crm_enum_activitynotifytype`](../../../data-types.md#activity-enums) | Notification type | ||
|| **NOTIFY_VALUE**
[`integer`](../../../../data-types.md) | Notification value | Used together with `NOTIFY_TYPE` ||
|| **ORIGINATOR_ID**
[`string`](../../../../data-types.md) | Data source ID | Used only for binding to an external source ||
|| **ORIGIN_ID**
[`string`](../../../../data-types.md) | ID of the element in the data source | Used only for binding to an external source ||
|| **PRIORITY**
[`crm_enum_activitypriority`](../../../data-types.md#activity-enums) | Priority | ||
|| **PROVIDER_DATA**
[`string`](../../../../data-types.md) | Additional provider data | ||
|| **PROVIDER_GROUP_ID**
[`string`](../../../../data-types.md) | Provider group ID | ||
|| **PROVIDER_ID**
[`string`](../../../../data-types.md) | Provider ID | Requirements depend on the activity type and provider ||
|| **PROVIDER_TYPE_ID**
[`string`](../../../../data-types.md) | Provider type ID | If the value is empty and a provider is defined, it is populated automatically by the provider's `getTypeId` method ||
|| **PROVIDER_PARAMS**
[`object`](../../../../data-types.md) | Additional provider parameters | ||
|| **RESPONSIBLE_ID***
[`user`](../../../../data-types.md) | Integer ID of the user responsible for the activity | When creating an activity without a positive value, the user responsible for the CRM item is used, or the current user if no responsible user is defined ||
|| **RESULT_CURRENCY_ID**
[`string`](../../../../data-types.md) | ID of the currency for the result amount | Used together with `RESULT_SUM` ||
|| **RESULT_MARK**
[`integer`](../../../../data-types.md) | Activity result rating: `0` — no rating, `1` — negative, `2` — positive, `3` — neutral | Used in activity statistics ||
|| **RESULT_SOURCE_ID**
[`string`](../../../../data-types.md) | Result source ID | Used to group ratings in activity statistics ||
|| **RESULT_STATUS**
[`integer`](../../../../data-types.md) | Result status: `0` — not specified, `1` — unanswered, `2` — answered | Used in activity statistics ||
|| **RESULT_STREAM**
[`integer`](../../../../data-types.md) | Statistics stream: `0` — not specified, `1` — incoming, `2` — outgoing, `3` — reverse, `4` — missed | Used in activity statistics ||
|| **RESULT_SUM**
[`double`](../../../../data-types.md) | Result amount | The currency is specified in `RESULT_CURRENCY_ID` ||
|| **RESULT_VALUE**
[`double`](../../../../data-types.md) | Numeric value of the activity result | A `double` field with no fixed set of values in the metadata ||
|| **SETTINGS**
[`object`](../../../../data-types.md) | Additional settings | ||
|| **START_TIME**
[`datetime`](../../../../data-types.md) | Time the activity starts | ||
|| **STATUS**
[`crm_enum_activitystatus`](../../../data-types.md#activity-enums) | Activity status | ||
|| **SUBJECT***
[`string`](../../../../data-types.md) | Activity subject | A non-empty string is required when creating an activity ||
|| **WEBDAV_ELEMENTS**
[`diskfile`](../../../data-types.md) | Added files | Multiple. For Drive storage, the field is deprecated and retained for compatibility; files are passed in `FILES`. For WebDAV storage, the type `webdav` is used ||
|| **IS_INCOMING_CHANNEL**
[`char`](../../../../data-types.md) | Indicates whether the activity was created from an incoming channel: `Y` — yes, `N` — no | Read-only ||
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
|| **Status** | **Code** | **Description** | **Meaning** ||
|| `400` | Empty value | `Access denied.` | No permission to read CRM items or digital workspaces ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-add.md)
- [{#T}](./crm-activity-update.md)
- [{#T}](./crm-activity-get.md)
- [{#T}](./crm-activity-list.md)
- [{#T}](./crm-activity-delete.md)
- [{#T}](./crm-activity-call-get-transcript.md)
- [{#T}](./crm-activity-communication-fields.md)