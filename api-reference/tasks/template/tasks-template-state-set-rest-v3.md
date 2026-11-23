# Save Default Values for the Template Form v 3.0 tasks.template.state.set

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to create templates

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `tasks.template.state.set` method saves default values for the task template creation form.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **needsControl**
[`boolean`](../../data-types.md) | Task review by the creator.

Possible values:

- `true` — enable the option in a new template
- `false` — do not enable the option ||
|| **matchesWorkTime**
[`boolean`](../../data-types.md) | Consider working hours when calculating dates.

Possible values:

- `true` — enable the option in a new template
- `false` — do not enable the option ||
|| **defaultRequireResult**
[`boolean`](../../data-types.md) | Require a task result.

Possible values:

- `true` — enable the option in a new template
- `false` — do not enable the option ||
|| **allowsTimeTracking**
[`boolean`](../../data-types.md) | Task time tracking.

Possible values:

- `true` — enable the option in a new template
- `false` — do not enable the option ||
|#

The values are saved in the personal settings of the user on whose behalf the method is called: the webhook owner or the application user. These are form settings in the Bitrix24 interface, not template properties. The method does not change existing templates and does not affect templates created by the [tasks.template.add](./tasks-template-add-rest-v3.md) method.

Flags that are not passed in the request keep their previous values. A request without parameters changes nothing and returns `true`. The saved values cannot be read via REST.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.state.set`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"needsControl":true,"defaultRequireResult":true}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.state.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"needsControl":true,"defaultRequireResult":true,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.state.set
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
    type TemplateStateSetResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateStateSetResult>(
        'tasks.template.state.set',
        {
          needsControl: true,
          defaultRequireResult: true,
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Defaults saved:', result.result)
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
      async function setTemplateFormDefaults() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.state.set',
            {
              needsControl: true,
              defaultRequireResult: true,
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Defaults saved:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setTemplateFormDefaults)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet included in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.state.set"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "needsControl": True,
                "defaultRequireResult": True,
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
                'tasks.template.state.set',
                [
                    'needsControl' => true,
                    'defaultRequireResult' => true,
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
        'tasks.template.state.set',
        {
            needsControl: true,
            defaultRequireResult: true,
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
        'tasks.template.state.set',
        [
            'needsControl' => true,
            'defaultRequireResult' => true,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.state.set", b24.Params{
    	"needsControl":         true,
    	"defaultRequireResult": true,
    })
    if err != nil {
    	return fmt.Errorf("tasks.template.state.set: %w", err)
    }

    var out struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(out.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": true
    },
    "time": {
        "start": 1795429173,
        "finish": 1795429173.522884,
        "duration": 0.5228838920593262,
        "processing": 0,
        "date_start": "2026-11-23T11:19:33+01:00",
        "date_finish": "2026-11-23T11:19:33+01:00",
        "operating_reset_at": 1795429740,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response.

Contains an object with the `result` key and the `true` value if the values were saved ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION",
        "message": "Access denied"
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to create templates: the `create` field in the [tasks.template.access.get](./tasks-template-access-get-rest-v3.md) response. Also make sure the new task card is available on Bitrix24 ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-add-rest-v3.md)
- [{#T}](./tasks-template-access-get-rest-v3.md)
- [{#T}](./fields-rest-v3.md)
