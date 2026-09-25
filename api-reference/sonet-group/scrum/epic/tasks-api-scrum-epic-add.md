# Add Epic in Scrum tasks.api.scrum.epic.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

This method adds an epic to Scrum.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values of the new epic [(Detailed Description)](#fields) in the form of a structure:

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

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Epic name, up to 255 characters ||
|| **description**
[`string`](../../../data-types.md) | Epic description ||
|| **groupId***
[`integer`](../../../data-types.md) | Identifier of the Scrum in which the epic is created.

You can retrieve the identifier using the [socialnetwork.api.workgroup.list](../../socialnetwork-api-workgroup-list.md) method ||
|| **color**
[`string`](../../../data-types.md) | Epic color, for example `#69dafc`, up to 18 characters. The method does not validate the color format and retains the string as is ||
|| **files**
[`array`](../../../data-types.md) | Array of Drive file identifiers. Prefix each identifier with `n`, for example `["n428", "n345"]` ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user to be set as the creator of the epic. Defaults to the current user ||
|#

{% note warning "Attention" %}

The method skips a file identifier without the `n` prefix and a nonexistent file without an error: the epic is created without these files

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "fields": {
        "name": "Epic 1",
        "groupId": 1,
        "description": "Description text",
        "color": "#69dafc",
        "files": ["n428", "n345"]
    }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.epic.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "fields": {
        "name": "Epic 1",
        "groupId": 1,
        "description": "Description text",
        "color": "#69dafc",
        "files": ["n428", "n345"]
    },
    "auth": "YOUR_ACCESS_TOKEN"
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.epic.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EpicAddResult = {
      id: number
      groupId: number
      name: string
      description: string
      createdBy: number
      modifiedBy: number
      color: string
    }

    try {
      const response = await $b24.actions.v2.call.make<EpicAddResult>({
        method: 'tasks.api.scrum.epic.add',
        params: {
          fields: {
            name: 'Epic 1',
            groupId: 1,
            description: 'Description text',
            color: '#69dafc',
            files: ['n428', 'n345'],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Added epic:', result.id, result.name)
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
      async function addEpic() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.api.scrum.epic.add',
            params: {
              fields: {
                name: 'Epic 1',
                groupId: 1,
                description: 'Description text',
                color: '#69dafc',
                files: ['n428', 'n345'],
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
          console.info('Added epic:', result.id, result.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addEpic)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.api.scrum.epic.add(
            fields={
                "name": "Epic 1",
                "groupId": 1,
                "description": "Description text",
                "color": "#69dafc",
                "files": [
                    "n428",
                    "n345",
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
                'tasks.api.scrum.epic.add',
                [
                    'fields' => [
                        'name'        => $name,
                        'groupId'     => $groupId,
                        'description' => $description,
                        'color'       => $color,
                        'files'       => $files,
                    ],
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
        echo 'Error adding epic: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const groupId = 1;
    const name = 'Epic 1';
    const description = 'Description text';
    const color = '#69dafc';
    const files = ['n428', 'n345'];

    BX24.callMethod(
        'tasks.api.scrum.epic.add',
        {
            fields: {
                name: name,
                groupId: groupId,
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
    require_once('crest.php'); // include CRest PHP SDK

    $groupId = 1;
    $name = 'Epic 1';
    $description = 'Description text';
    $color = '#69dafc';
    $files = ['n428', 'n345'];

    // execute request to REST API
    $result = CRest::call(
        'tasks.api.scrum.epic.add',
        [
            'fields' => [
                'name' => $name,
                'groupId' => $groupId,
                'description' => $description,
                'color' => $color,
                'files' => $files
            ]
        ]
    );

    // Process response from Bitrix24
    if (isset($result['error'])) {
        echo 'Error: '.$result['error_description'];
    }
    else {
        print_r($result['result']);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "tasks.api.scrum.epic.add", b24.Params{
    	"fields": b24.Params{
    		"name":        "Epic 1",
    		"groupId":     1,
    		"description": "Description text",
    		"color":       "#69dafc",
    		"files":       []string{"n428", "n345"},
    	},
    })
    if err != nil {
    	return fmt.Errorf("tasks.api.scrum.epic.add: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "id": 4,
        "groupId": 1,
        "name": "Epic 1",
        "description": "Description text",
        "createdBy": 1,
        "modifiedBy": 0,
        "color": "#69dafc"
    },
    "time": {
        "start": 1790262925,
        "finish": 1790262925.771081,
        "duration": 0.7710809707641602,
        "processing": 0,
        "date_start": "2026-09-24T18:15:25+03:00",
        "date_finish": "2026-09-24T18:15:25+03:00",
        "operating_reset_at": 1790263525,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Data of the created epic [(Detailed Description)](#result) ||
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
[`string`](../../../data-types.md) | Epic description. An empty string if no description was passed ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the epic ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who last modified the epic. `0` for a new epic ||
|| **color**
[`string`](../../../data-types.md) | Epic color. An empty string if no color was passed ||
|#

The method does not return attached files. You can retrieve them using the [tasks.api.scrum.epic.get](./tasks-api-scrum-epic-get.md) method.

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Group id not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Group id not found | `groupId` was not passed ||
|| `400` | `0` | Name not found | The `name` was not passed, or an empty string was passed ||
|| `400` | `0` | Access denied | The user has no access to the tasks of the group, or a group with this `groupId` does not exist ||
|| `400` | `0` | createdBy user not found | The user from `createdBy` does not exist ||
|| `400` | `0` | Epic not created | Failed to save the epic, for example, the name is longer than 255 characters or the color is longer than 18 characters ||
|| `400` | `0` | Epic files not attached | The epic was created, but the files could not be attached ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter was not passed ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-epic-update.md)
- [{#T}](./tasks-api-scrum-epic-get.md)
- [{#T}](./tasks-api-scrum-epic-list.md)
- [{#T}](./tasks-api-scrum-epic-delete.md)
- [{#T}](./tasks-api-scrum-epic-get-fields.md)