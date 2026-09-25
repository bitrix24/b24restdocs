# Get Epic Fields by Its Identifier tasks.api.scrum.epic.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

The method retrieves the values of the epic fields by its identifier `id`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Epic identifier.

You can get the identifiers of epics using the method [`tasks.api.scrum.epic.list`](./tasks-api-scrum-epic-list.md) ||
|| **withFiles**
[`boolean`](../../../data-types.md) | Whether to return the epic files in the `files` field. Defaults to `true`.

To retrieve an epic without files, pass `false` or `0` ||
|#

{% note warning "Attention" %}

The method treats the strings `"false"` and `"N"` in `withFiles` as `true` and returns the files. Pass a boolean `false` in a JSON request, or `0`

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.api.scrum.epic.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.api.scrum.epic.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EpicGetResult = {
      id: number
      groupId: number
      name: string
      description: string
      createdBy: number
      modifiedBy: number
      color: string
      files: Record<string, unknown>
    }

    try {
      const response = await $b24.actions.v2.call.make<EpicGetResult>({
        method: 'tasks.api.scrum.epic.get',
        params: {
          id: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.id, result.name, result.color)
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
      async function getEpic() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.api.scrum.epic.get',
            params: {
              id: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.id, result.name, result.color)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getEpic)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.api.scrum.epic.get(
            bitrix_id=1,
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
        $epicId = 1;
        $response = $b24Service
            ->core
            ->call(
                'tasks.api.scrum.epic.get',
                [
                    'id' => $epicId,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting epic: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const epicId = 1;
    BX24.callMethod(
        'tasks.api.scrum.epic.get',
        {
            id: epicId,
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.api.scrum.epic.get',
        [
            'id' => 1
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "tasks.api.scrum.epic.get", b24.Params{
    	"id": 1,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.api.scrum.epic.get: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "id": 2,
        "groupId": 2,
        "name": "User Registration",
        "description": "Login form, registration, and password recovery",
        "createdBy": 1,
        "modifiedBy": 1,
        "color": "#69dafc",
        "files": {
            "ID": "60",
            "ENTITY_ID": "TASKS_SCRUM_EPIC",
            "FIELD_NAME": "UF_SCRUM_EPIC_FILES",
            "USER_TYPE_ID": "disk_file",
            "XML_ID": null,
            "SORT": "100",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "N",
            "EDIT_IN_LIST": "N",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "IBLOCK_ID": null,
                "SECTION_ID": null,
                "UF_TO_SAVE_ALLOW_EDIT": false
            },
            "USER_TYPE": {
                "USER_TYPE_ID": "disk_file",
                "CLASS_NAME": "Bitrix\\Disk\\Uf\\FileUserType",
                "DESCRIPTION": "File (Drive)",
                "BASE_TYPE": "int",
                "TAG": [
                    "DISK FILE ID",
                    "DOCUMENT ID"
                ]
            },
            "VALUE": [
                6
            ],
            "ENTITY_VALUE_ID": 2,
            "VALUE_EXISTS": true,
            "VALUE_RAW": "a:1:{i:0;i:6;}",
            "CUSTOM_DATA": {
                "PHOTO_TEMPLATE": ""
            },
            "EDIT_FORM_LABEL": "UF_SCRUM_EPIC_FILES",
            "TAG": "DOCUMENT ID"
        }
    },
    "time": {
        "start": 1790263942,
        "finish": 1790263942.418237,
        "duration": 0.4182369709014893,
        "processing": 0,
        "date_start": "2026-09-24T18:32:22+03:00",
        "date_finish": "2026-09-24T18:32:22+03:00",
        "operating_reset_at": 1790264542,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Epic data [(Detailed Description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Epic identifier ||
|| **groupId**
[`integer`](../../../data-types.md) | Identifier of the Scrum to which the epic belongs ||
|| **name**
[`string`](../../../data-types.md) | Epic name ||
|| **description**
[`string`](../../../data-types.md) | Epic description ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the epic ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who last modified the epic. `0` if the epic has not been modified ||
|| **color**
[`string`](../../../data-types.md) | Epic color ||
|| **files**
[`object`](../../../data-types.md) | Epic files as the `UF_SCRUM_EPIC_FILES` custom field [(Detailed Description)](#files) ||
|#

#### files Object {#files}

The file data you need is in the `VALUE` field. The other fields of the object contain internal metadata describing the custom field.

#|
|| **Name**
`type` | **Description** ||
|| **VALUE**
[`array`](../../../data-types.md) | Identifiers of the files attached to the epic. These are attachment identifiers, not Drive file identifiers: you can retrieve the file name, download link, and Drive file identifier `OBJECT_ID` using the [disk.attachedObject.get](../../../disk/attached-object/disk-attached-object-get.md) method.

An empty array if there are no files ||
|| **VALUE_EXISTS**
[`boolean`](../../../data-types.md) | Returned with the value `true` if files are attached to the epic. If there are no files, this field is absent from the response ||
|| **FIELD_NAME**
[`string`](../../../data-types.md) | Custom field code, always `UF_SCRUM_EPIC_FILES` ||
|| **USER_TYPE_ID**
[`string`](../../../data-types.md) | Custom field type, always `disk_file` ||
|| **ENTITY_VALUE_ID**
[`integer`](../../../data-types.md) | Epic identifier ||
|| **VALUE_RAW**
[`string`](../../../data-types.md) | The `VALUE` value in PHP serialized form. If there are no files, this field is absent from the response ||
|#

The remaining fields describe the settings of the `UF_SCRUM_EPIC_FILES` custom field itself. They are the same for all epics and do not depend on the attached files:

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../../data-types.md) | Custom field identifier ||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | Object the field belongs to, always `TASKS_SCRUM_EPIC` ||
|| **XML_ID**
[`string`](../../../data-types.md) \| `null` | External code of the field. `null` for the epic files field ||
|| **SORT**
[`string`](../../../data-types.md) | Field sort order ||
|| **MULTIPLE**
[`string`](../../../data-types.md) | Whether the field accepts multiple values, always `Y` ||
|| **MANDATORY**
[`string`](../../../data-types.md) | Whether the field is required, always `N` ||
|| **SHOW_FILTER**, **SHOW_IN_LIST**, **EDIT_IN_LIST**, **IS_SEARCHABLE**
[`string`](../../../data-types.md) | Field display settings in the interface, `Y` or `N` ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Field settings: `IBLOCK_ID`, `SECTION_ID`, `UF_TO_SAVE_ALLOW_EDIT` ||
|| **USER_TYPE**
[`object`](../../../data-types.md) | Field type description: `USER_TYPE_ID`, `CLASS_NAME`, `DESCRIPTION`, `BASE_TYPE`, `TAG` ||
|| **CUSTOM_DATA**
[`object`](../../../data-types.md) | Additional data of the field type ||
|| **EDIT_FORM_LABEL**
[`string`](../../../data-types.md) | Field label in the edit form ||
|| **TAG**
[`string`](../../../data-types.md) | Field type tag, `DOCUMENT ID` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Epic not found | An epic with this `id` does not exist ||
|| `400` | `0` | Access denied | The user has no access to the tasks of the group the epic belongs to ||
|| `400` | `100` | Could not find value for parameter {id} | The `id` parameter was not passed ||
|| `400` | `100` | Invalid value {stringValue} to match with parameter {id}. Should be value of type int. | A non-numeric value was passed in `id` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-epic-add.md)
- [{#T}](./tasks-api-scrum-epic-update.md)
- [{#T}](./tasks-api-scrum-epic-list.md)
- [{#T}](./tasks-api-scrum-epic-delete.md)
- [{#T}](./tasks-api-scrum-epic-get-fields.md)