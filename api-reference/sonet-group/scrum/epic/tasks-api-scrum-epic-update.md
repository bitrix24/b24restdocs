# Update Epic in Scrum tasks.api.scrum.epic.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

This method updates an epic in Scrum.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Epic identifier.

You can obtain epic identifiers using the [`tasks.api.scrum.epic.list`](./tasks-api-scrum-epic-list.md) method. ||
|| **fields***
[`object`](../../../data-types.md) | Epic fields to change [(Detailed Description)](#fields), in the form of a structure:

```js
fields: {
    name: 'value',
    groupId: 'value',
    description: 'value',
    color: 'value',
    files: [
        'file1',
        'file2',
        ...
    ]

}
```
||
|#

### Parameter fields {#fields}

Pass only the fields you need to change. The method leaves the other epic fields unchanged.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Epic name, up to 255 characters. The method does not retain an empty string ||
|| **description**
[`string`](../../../data-types.md) | Epic description ||
|| **groupId**
[`integer`](../../../data-types.md) | Identifier of the Scrum to move the epic to. Requires access to the tasks of both groups ||
|| **color**
[`string`](../../../data-types.md) | Epic color, for example `#bbecf1`, up to 18 characters ||
|| **files**
[`array`](../../../data-types.md) | Array of Drive file identifiers with the `n` prefix, for example `["n429"]`.

New files are added to those already attached.

{% note warning "Attention" %}

If you pass an empty array, the method detaches all files from the epic. The method skips an identifier without the `n` prefix without an error

{% endnote %}

||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user to be set as the creator of the epic ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user to be set as the last modifier of the epic. Defaults to the current user ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 1,
    "fields": {
        "name": "Updated epic name",
        "description": "Updated description text",
        "color": "#bbecf1",
        "files": ["n429", "n243"]
    }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.epic.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 1,
    "fields": {
        "name": "Updated epic name",
        "description": "Updated description text",
        "color": "#bbecf1",
        "files": ["n429", "n243"]
    },
    "auth": "YOUR_ACCESS_TOKEN"
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.epic.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EpicUpdateResult = {
      id: number
      groupId: number
      name: string
      description: string
      createdBy: number
      modifiedBy: number
      color: string
    }

    try {
      const response = await $b24.actions.v2.call.make<EpicUpdateResult>({
        method: 'tasks.api.scrum.epic.update',
        params: {
          id: 1,
          fields: {
            name: 'Updated epic name',
            description: 'Updated description text',
            color: '#bbecf1',
            files: ['n429', 'n243'],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated epic:', result.id, result.name, result.color)
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
      async function updateEpic() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.api.scrum.epic.update',
            params: {
              id: 1,
              fields: {
                name: 'Updated epic name',
                description: 'Updated description text',
                color: '#bbecf1',
                files: ['n429', 'n243'],
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
          console.info('Updated epic:', result.id, result.name, result.color)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateEpic)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.api.scrum.epic.update(
            bitrix_id=1,
            fields={
                "name": "Updated epic name",
                "description": "Updated description text",
                "color": "#bbecf1",
                "files": [
                    "n429",
                    "n243",
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
        $epicId = 1;
        $name = 'Updated epic name';
        $description = 'Updated description text';
        $color = '#bbecf1';
        $files = ['n429', 'n243'];
    
        $response = $b24Service
            ->core
            ->call(
                'tasks.api.scrum.epic.update',
                [
                    'id' => $epicId,
                    'fields' => [
                        'name' => $name,
                        'description' => $description,
                        'color' => $color,
                        'files' => $files
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating epic: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const epicId = 1;
    const name = 'Updated epic name';
    const description = 'Updated description text';
    const color = '#bbecf1';
    const files = ['n429', 'n243'];
    BX24.callMethod(
        'tasks.api.scrum.epic.update',
        {
            id: epicId,
            fields:{
                name: name,
                description: description,
                color: color,
                files: files
            }
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK
    $epicId = 1;
    $name = 'Updated epic name';
    $description = 'Updated description text';
    $color = '#bbecf1';
    $files = ['n429', 'n243'];

    // executing request to REST API
    $result = CRest::call(
    'tasks.api.scrum.epic.update',
    [
        'id' => $epicId,
        'fields' => [
            'name' => $name,
            'description' => $description,
            'color' => $color,
            'files' => $files
        ]
    ]
    );

    // Processing the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    }
    else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "id": 1,
        "groupId": 143,
        "name": "Updated epic name",
        "description": "Updated description text",
        "createdBy": 1,
        "modifiedBy": 1,
        "color": "#bbecf1"
    },
    "time": {
        "start": 1790263154,
        "finish": 1790263154.532794,
        "duration": 0.5327939987182617,
        "processing": 0,
        "date_start": "2026-09-24T18:19:14+03:00",
        "date_finish": "2026-09-24T18:19:14+03:00",
        "operating_reset_at": 1790263754,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Epic data after the update [(Detailed Description)](#result) ||
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
[`integer`](../../../data-types.md) | Identifier of the user who last modified the epic ||
|| **color**
[`string`](../../../data-types.md) | Epic color ||
|#

The method does not return attached files. You can retrieve them using the [tasks.api.scrum.epic.get](./tasks-api-scrum-epic-get.md) method.

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Epic not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Epic not found | An epic with this `id` does not exist ||
|| `400` | `0` | Access denied | The user has no access to the tasks of the epic's group or of the group from `groupId`, or such a group does not exist ||
|| `400` | `0` | createdBy user not found | The user from `createdBy` does not exist ||
|| `400` | `0` | modifiedBy user not found | The user from `modifiedBy` does not exist ||
|| `400` | `0` | Epic not updated | Failed to save the changes, for example, the name is longer than 255 characters or the color is longer than 18 characters ||
|| `400` | `0` | Epic files not attached | The changes were saved, but the files could not be attached ||
|| `400` | `100` | Could not find value for parameter {id} | The `id` parameter was not passed ||
|| `400` | `100` | Invalid value {stringValue} to match with parameter {id}. Should be value of type int. | A non-numeric value was passed in `id` ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter was not passed ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-epic-add.md)
- [{#T}](./tasks-api-scrum-epic-get.md)
- [{#T}](./tasks-api-scrum-epic-list.md)
- [{#T}](./tasks-api-scrum-epic-delete.md)
- [{#T}](./tasks-api-scrum-epic-get-fields.md)