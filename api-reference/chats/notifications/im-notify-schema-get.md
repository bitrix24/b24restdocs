# Get Notification Type Schema im.notify.schema.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.schema.get` returns the notification types of the Bitrix24 account, grouped by module.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify.schema.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.notify.schema.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    // Keys of result are module codes: blog, calendar, tasks and so on
    type NotificationSchemaResult = Record<string, ModuleSchema>

    type ModuleSchema = {
      NAME: string
      MODULE_ID: string
      LIST: Array<{
        ID: string
        NAME: string
      }>
    }

    try {
      const response = await $b24.actions.v2.call.make<NotificationSchemaResult>({
        method: 'im.notify.schema.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        for (const module of Object.values(result)) {
          console.info(module.NAME, module.LIST.map((item) => item.ID))
        }
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
      async function getNotificationSchema() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.notify.schema.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          for (const module of Object.values(result)) {
            console.info(module.NAME, module.LIST.map((item) => item.ID))
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getNotificationSchema)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.notify.schema.get().response
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
        $response = $b24Service->core->call('im.notify.schema.get', []);
        $result = $response->getResponseData()->getResult();

        print_r($result);
    } catch (Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('im.notify.schema.get', {}, function(result) {
        if (result.error()) {
            console.error(result.error().ex);
        } else {
            console.log(result.data());
        }
    });
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call('im.notify.schema.get', []);

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        var_dump($result['result']);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.notify.schema.get", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("im.notify.schema.get: %w", err)
    }

    // The keys of result are module codes: blog, calendar, tasks, and others
    var modules map[string]struct {
    	Name     string `json:"NAME"`
    	ModuleID string `json:"MODULE_ID"`
    	List     []struct {
    		ID   string `json:"ID"`
    		Name string `json:"NAME"`
    	} `json:"LIST"`
    }
    if err := json.Unmarshal(res.Result, &modules); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for code, module := range modules {
    	fmt.Println(code, module.Name, len(module.List))
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "tasks": {
            "NAME": "Tasks",
            "MODULE_ID": "tasks",
            "LIST": [
                {
                    "ID": "tasks|comment",
                    "NAME": "New comment added to a task"
                },
                {
                    "ID": "tasks|reminder",
                    "NAME": "Task reminder"
                },
                {
                    "ID": "tasks|manage",
                    "NAME": "Task created or updated"
                },
                {
                    "ID": "tasks|task_assigned",
                    "NAME": "Task assigned to you"
                },
                {
                    "ID": "tasks|task_expired_soon",
                    "NAME": "Task almost overdue"
                }
            ]
        },
        "rest": {
            "NAME": "Market",
            "MODULE_ID": "rest",
            "LIST": [
                {
                    "ID": "rest|app_install",
                    "NAME": "New app installed"
                },
                {
                    "ID": "rest|app_install_request",
                    "NAME": "App installation request"
                },
                {
                    "ID": "rest|vi_user_data_request",
                    "NAME": "App requested data access"
                }
            ]
        }
    },
    "time": {
        "start": 1794317532,
        "finish": 1794317532.176408,
        "duration": 0.176408052444458,
        "processing": 0,
        "date_start": "2026-11-10T14:32:12+01:00",
        "date_finish": "2026-11-10T14:32:12+01:00",
        "operating_reset_at": 1794318120,
        "operating": 0
    }
}
```

The example shows two modules from the response. The full response contains all modules of the Bitrix24 account that send notifications, for example `blog`, `calendar`, `crm`, `disk`, `im`, `tasks`.

### Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object in which the key is a module code, for example `tasks`, and the value is a description of the module and its notification types [(detailed description)](#module-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Module Object {#module-object}

#|
|| **Name**
`Type` | **Description** ||
|| **NAME**
[`string`](../../data-types.md) | The name of the module ||
|| **MODULE_ID**
[`string`](../../data-types.md) | Module code. Matches the object key in `result` and the `notify_module` field of a notification ||
|| **LIST**
[`array`](../../data-types.md) | Array of the module's notification types [(detailed description)](#list-item-object) ||
|#

#### LIST Item {#list-item-object}

#|
|| **Name**
`Type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Notification type code in the format ```MODULE|EVENT```, for example ```tasks|comment```.

Use it in the `SEARCH_TYPE` and `SEARCH_TYPES` parameters of the [im.notify.history.search](./im-notify-history-search.md) method. Matches the `setting_name` field of a notification in the [im.notify.get](./im-notify-get.md) response.

The schema does not contain all codes that occur in notifications: for example, app notifications come with the ```rest|rest_notify``` code, which is not in `LIST` ||
|| **NAME**
[`string`](../../data-types.md) | The name of the notification type ||
|#

## Error Handling

HTTP Status: **401**

```json
{
    "error": "INVALID_CREDENTIALS",
    "error_description": "Invalid request credentials"
}
```

The method has no error codes of its own, only system REST API errors are possible.

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-notify.md)
- [{#T}](./im-notify-personal-add.md)
- [{#T}](./im-notify-system-add.md)
- [{#T}](./im-notify-get.md)
- [{#T}](./im-notify-read-list.md)
- [{#T}](./im-notify-read.md)
- [{#T}](./im-notify-read-all.md)
- [{#T}](./im-notify-answer.md)
- [{#T}](./im-notify-confirm.md)
- [{#T}](./im-notify-delete.md)
- [{#T}](./im-notify-history-search.md)