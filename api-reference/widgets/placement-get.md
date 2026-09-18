# Get Handlers Registered by the Application placement.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `placement.get` returns the list of widget handlers that the application registered with the method [placement.bind](./placement-bind.md). For each handler, the method returns the embedding point code, the handler address, the user for whom the widget is registered, the settings, and the names in different languages.

Use the method to check the current registrations: before calling [placement.bind](./placement-bind.md) again or before removing a handler with the method [placement.unbind](./placement-unbind.md). The list of embedding points available to the application is returned by another method — [placement.list](./placement-list.md).

{% note info "" %}

The method works only in the context of an [application](../../settings/app-installation/index.md)

{% endnote %}

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/placement.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each PlacementHandler returned in result[]
    type PlacementHandler = {
      id: number
      placement: string
      userId: number
      handler: string
      options: Record<string, string> | unknown[]
      title: string
      description: string
      langAll: Record<string, { TITLE: string; DESCRIPTION: string; GROUP_NAME: string }>
    }

    try {
      const response = await $b24.actions.v2.call.make<PlacementHandler[]>({
        method: 'placement.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Registered handlers count:', result.length, result)
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
      async function getPlacementHandlers() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Registered handlers count:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getPlacementHandlers)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.placement.get().response
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
                'placement.get',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting placements: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "placement.get",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'placement.get',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "placement.get", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("placement.get: %w", err)
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
    "result": [
        {
            "id": 41,
            "placement": "CRM_DEAL_LIST_TOOLBAR",
            "userId": 0,
            "handler": "https://myapp.com/?handler=1",
            "options": [],
            "title": "Add invoice",
            "description": "",
            "langAll": {
                "de": {
                    "TITLE": "Add invoice",
                    "DESCRIPTION": "",
                    "GROUP_NAME": "Documents"
                }
            }
        },
        {
            "id": 42,
            "placement": "CRM_DEAL_LIST_TOOLBAR",
            "userId": 0,
            "handler": "https://myapp.com/?handler=1",
            "options": [],
            "title": "Import invoice",
            "description": "",
            "langAll": {
                "de": {
                    "TITLE": "Import invoice",
                    "DESCRIPTION": "",
                    "GROUP_NAME": "Documents"
                }
            }
        },
        {
            "id": 43,
            "placement": "IM_CONTEXT_MENU",
            "userId": 0,
            "handler": "https://myapp.com/?handler=2",
            "options": {
                "extranet": "N",
                "context": "ALL",
                "role": "USER"
            },
            "title": "My App 1",
            "description": "",
            "langAll": {
                "de": {
                    "TITLE": "My App 1",
                    "DESCRIPTION": "",
                    "GROUP_NAME": ""
                }
            }
        },
        {
            "id": 44,
            "placement": "PAGE_BACKGROUND_WORKER",
            "userId": 1,
            "handler": "https://myapp.com/?handler=3",
            "options": {
                "errorHandlerUrl": "https://myapp.com/?handler=3"
            },
            "title": "My App 2",
            "description": "",
            "langAll": {
                "de": {
                    "TITLE": "My App 2",
                    "DESCRIPTION": "",
                    "GROUP_NAME": ""
                }
            }
        }
    ],
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
[`array`](../data-types.md) | List of registered widget handlers [(detailed description)](#result). If the application has not registered any handlers, the list is empty ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

#### Element of the result Array {#result}

Except for `id`, the field values are set by the application when registering the handler with the method [placement.bind](./placement-bind.md). In the response, field names are written in camelCase rather than in uppercase like the registration parameters: `placement` instead of `PLACEMENT`, `userId` instead of `USER_ID`. The mapping for each field is given in the table.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the registered handler ||
|| **placement**
[`string`](../data-types.md) | Embedding point code. The `PLACEMENT` parameter at registration ||
|| **userId**
[`integer`](../data-types.md) | Identifier of the user for whom the widget is registered. The `USER_ID` parameter at registration. The value `0` means the widget is available to all users ||
|| **handler**
[`string`](../data-types.md) | Widget handler address. The `HANDLER` parameter at registration ||
|| **options**
[`object`](../data-types.md) or [`array`](../data-types.md) | Additional widget display parameters. The `OPTIONS` parameter at registration. An object if the parameters are set, otherwise an empty array `[]` ||
|| **title**
[`string`](../data-types.md) | Widget name for one language: the one in which the registration was performed, or the first one from `LANG_ALL` if there is no such translation. The `TITLE` parameter at registration ||
|| **description**
[`string`](../data-types.md) | Widget description for the same language as `title`. The `DESCRIPTION` parameter at registration ||
|| **langAll**
[`object`](../data-types.md) | Widget name, description, and group for the languages passed at registration. The key is the language code, and the value is an object with the fields `TITLE`, `DESCRIPTION`, and `GROUP_NAME`. The `LANG_ALL` parameter at registration ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method Application context required"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Meaning** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | The method was called outside the application context, for example via a webhook ||
|| `403` | `ACCESS_DENIED` | Access denied! | The method was called by a user without administrator permissions ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./placements.md)
- [{#T}](./placement-list.md)
- [{#T}](./placement-bind.md)
- [{#T}](./placement-unbind.md)
- [{#T}](./ui-interaction/index.md)
