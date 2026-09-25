# Get a list of available fields for epic tasks.api.scrum.epic.getFields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.api.scrum.epic.getFields` returns the available fields for an epic.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.epic.getFields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "auth": "YOUR_ACCESS_TOKEN"
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.epic.getFields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EpicFieldItem = {
      type: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EpicFieldsResult = {
      fields: Record<string, EpicFieldItem>
    }

    try {
      const response = await $b24.actions.v2.call.make<EpicFieldsResult>({
        method: 'tasks.api.scrum.epic.getFields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(Object.keys(result.fields), result.fields)
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
      async function getEpicFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.api.scrum.epic.getFields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(Object.keys(result.fields), result.fields)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getEpicFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.api.scrum.epic.get_fields().response
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
                'tasks.api.scrum.epic.getFields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting epic fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.api.scrum.epic.getFields',
        {},
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    // executing a request to the REST API
    $result = CRest::call(
    'tasks.api.scrum.epic.getFields',
    []
    );

    // Processing the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    }
    else {
        print_r($result['result']);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "tasks.api.scrum.epic.getFields", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.api.scrum.epic.getFields: %w", err)
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
        "fields": {
            "name": {
                "type": "string"
            },
            "description": {
                "type": "string"
            },
            "groupId": {
                "type": "integer"
            },
            "color": {
                "type": "string"
            },
            "files": {
                "type": "array"
            },
            "createdBy": {
                "type": "integer"
            },
            "modifiedBy": {
                "type": "integer"
            }
        }
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
[`object`](../../../data-types.md) | Object with the `fields` key [(Detailed Description)](#fields) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### fields Object {#fields}

The key is the epic field name in the [tasks.api.scrum.epic.add](./tasks-api-scrum-epic-add.md) and [tasks.api.scrum.epic.update](./tasks-api-scrum-epic-update.md) methods, and the value is an object with the field type `type`. In the [tasks.api.scrum.epic.list](./tasks-api-scrum-epic-list.md) method, the same fields are passed in uppercase.

#|
|| **Field**
`type` | **Description** ||
|| **name**
`string` | Epic name ||
|| **description**
`string` | Epic description ||
|| **groupId**
`integer` | Identifier of the Scrum to which the epic belongs ||
|| **color**
`string` | Epic color ||
|| **files**
`array` | Drive file identifiers with the `n` prefix ||
|| **createdBy**
`integer` | Identifier of the user who created the epic ||
|| **modifiedBy**
`integer` | Identifier of the user who last modified the epic ||
|#

## Error Handling

The method has no errors of its own. An example of a general error is an application token without the `task` scope:

HTTP status: **401**

```json
{
    "error": "insufficient_scope",
    "error_description": "The request requires higher privileges than provided by the access token"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-epic-add.md)
- [{#T}](./tasks-api-scrum-epic-update.md)
- [{#T}](./tasks-api-scrum-epic-get.md)
- [{#T}](./tasks-api-scrum-epic-list.md)
- [{#T}](./tasks-api-scrum-epic-delete.md)
