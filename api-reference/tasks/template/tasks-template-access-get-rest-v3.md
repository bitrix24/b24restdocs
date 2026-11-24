# Get Task Template Permissions v 3.0 tasks.template.access.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `tasks.template.access.get` method returns which actions with a task template are available to the user.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Task template ID, greater than `0`.

The ID can be retrieved with the [tasks.template.add](./tasks-template-add-rest-v3.md) method when creating a template or with the [tasks.template.list](./tasks-template-list-rest-v3.md) method ||
|#

The method checks the permissions of the user on whose behalf it is called: the webhook owner or the application user. You cannot retrieve another user's permissions with this method.

The method does not check whether the template exists: there is no error for a nonexistent or deleted `id`. If the template is unavailable, the permissions are `false`. An administrator receives `true` in all fields, even if the template does not exist. You can check that the template exists with the [tasks.template.get](./tasks-template-get-rest-v3.md) method.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.access.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":125}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.access.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":125,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.access.get
    ```

- JS (TS)

    REST 3.0 task template methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TemplateAccessResult = {
      read: boolean
      edit: boolean
      remove: boolean
      create: boolean
      changeRights: boolean
      detachParent: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateAccessResult>(
        'tasks.template.access.get',
        {
          id: 125,
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Template access rights — read:', result.read, 'edit:', result.edit, 'remove:', result.remove)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    REST 3.0 task template methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getTemplateAccess() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.access.get',
            {
              id: 125,
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Template access rights — read:', result.read, 'edit:', result.edit, 'remove:', result.remove)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTemplateAccess)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet included in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.access.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 125,
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.template.access.get',
                [
                    'id' => 125,
                ]
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'tasks.template.access.get',
        {
            id: 125,
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.template.access.get',
        [
            'id' => 125,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.access.get", b24.Params{
    	"id": 125,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.template.access.get: %w", err)
    }

    var rights struct {
    	Read         bool `json:"read"`
    	Edit         bool `json:"edit"`
    	Remove       bool `json:"remove"`
    	Create       bool `json:"create"`
    	ChangeRights bool `json:"changeRights"`
    	DetachParent bool `json:"detachParent"`
    }
    if err := json.Unmarshal(res.Result, &rights); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(rights.Read, rights.Edit)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "read": true,
        "edit": true,
        "remove": true,
        "create": true,
        "changeRights": true,
        "detachParent": true
    },
    "time": {
        "start": 1795429013,
        "finish": 1795429013.590555,
        "duration": 0.59055495262146,
        "processing": 0,
        "date_start": "2026-11-23T11:16:53+01:00",
        "date_finish": "2026-11-23T11:16:53+01:00",
        "operating_reset_at": 1795429560,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Permissions of the current user for the template [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

Each field is a permission for an action: `true` — allowed, `false` — denied.

#|
|| **Name**
`type` | **Description** ||
|| **read**
[`boolean`](../../data-types.md) | View the template and create tasks from it with the [tasks.template.task.add](./tasks-template-task-add-rest-v3.md) method ||
|| **edit**
[`boolean`](../../data-types.md) | Edit the template with the [tasks.template.update](./tasks-template-update-rest-v3.md) method ||
|| **remove**
[`boolean`](../../data-types.md) | Delete the template ||
|| **create**
[`boolean`](../../data-types.md) | Create templates ||
|| **changeRights**
[`boolean`](../../data-types.md) | Change template access permissions in the [permissions](./fields-rest-v3.md#permissions) field ||
|| **detachParent**
[`boolean`](../../data-types.md) | Detach the template from the parent template ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Required field \"id\" is missing.",
                "field": "id"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Add `id` to the request body ||
|| `id` | This request requires data type `int` in field `id` | Pass the template ID as a number, not a string ||
|| `id` | This request requires data type `integer` in field `id` | Pass an ID greater than `0` ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the new task card is available on Bitrix24 ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-get-rest-v3.md)
- [{#T}](./tasks-template-update-rest-v3.md)
- [{#T}](./tasks-template-task-add-rest-v3.md)
- [{#T}](./fields-rest-v3.md)
