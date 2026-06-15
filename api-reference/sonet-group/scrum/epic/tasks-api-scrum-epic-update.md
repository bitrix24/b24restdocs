# Update Epic in Scrum tasks.api.scrum.epic.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

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
[`array`](../../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for adding a new epic in the form of a structure:

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

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Epic name ||
|| **description**
[`string`](../../../data-types.md) | Epic description ||
|| **groupId***
[`integer`](../../../data-types.md) | Group identifier (Scrum) to which the epic belongs ||
|| **color**
[`string`](../../../data-types.md) | Epic color ||
|| **files**
[`array`](../../../data-types.md) | Array of files associated with the epic.

In `files`, you can pass an array of values with file identifiers, specifying the prefix `n` for each identifier.

{% note warning "Attention" %}

If you pass an empty array, the files will be deleted.

{% endnote %}

||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "fields": {
        "id": 1,
        "fields": {
            "name": "Updated epic name",
            "description": "Updated description text",
            "color": "#bbecf1",
            "files": ["n429", "n243"]
        }
    },
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.epic.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "fields": {
        "id": 1,
        "fields": {
            "name": "Updated epic name",
            "description": "Updated description text",
            "color": "#bbecf1",
            "files": ["n429", "n243"]
        }
    },
    auth=YOUR_ACCESS_TOKEN
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
    "id": 1,
    "groupId": 143,
    "name": "Updated epic name",
    "description": "Updated description text",
    "createdBy": 1,
    "modifiedBy": 1,
    "color": "#bbecf1"
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Epic identifier ||
|| **groupId**
[`integer`](../../../data-types.md) | Group identifier (Scrum) to which the epic is linked ||
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

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "Epic not updated"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description**  | **Value** ||
|| `0` | Access denied | No access to view epic data ||
|| `0` | Epic not found | The epic does not exist ||
|| `0` | Epic not updated | Failed to update the epic ||
|| `0` | createdBy user not found | User in the "creator" field not found ||
|| `0` | modifiedBy user not found | User in the "last modified by" field not found ||
|| `100` | Could not find value for parameter {id} | Incorrect parameter name or parameter not set ||
|| `100` | Invalid value {stringValue} to match with parameter {id}. Should be value of type int. | Invalid parameter type ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-epic-add.md)
- [{#T}](./tasks-api-scrum-epic-get.md)
- [{#T}](./tasks-api-scrum-epic-list.md)
- [{#T}](./tasks-api-scrum-epic-delete.md)
- [{#T}](./tasks-api-scrum-epic-get-fields.md)