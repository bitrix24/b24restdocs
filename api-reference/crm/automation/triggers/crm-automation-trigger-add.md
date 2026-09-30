# Add Trigger crm.automation.trigger.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.automation.trigger.add` registers an application trigger — an event that lets CRM move a deal or another object to the desired stage or status. For example, a telephony application registers the "Call completed" trigger with the `call_done` code. Then an administrator links it to a stage in the CRM automation settings, and the application executes the trigger using the [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) method. The workflow is described in the [triggers overview](./index.md).

The method works only in the context of an [application](../../../../settings/app-installation/index.md): the trigger belongs to the application that registered it.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../../data-types.md) | Trigger code, unique within the application, for example `call_done`. Latin letters, digits, and the `.`, `-`, `_` characters are allowed.

If the application already has a trigger with this `CODE`, the method updates its name `NAME` ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the trigger. It is displayed in the CRM automation settings when the trigger is linked to a stage ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"call_done","NAME":"Call completed","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.automation.trigger.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'crm.automation.trigger.add',
        params: {
          CODE: 'call_done',
          NAME: 'Call completed',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Trigger added:', result)
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
      async function addAutomationTrigger() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.automation.trigger.add',
            params: {
              CODE: 'call_done',
              NAME: 'Call completed',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Trigger added:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addAutomationTrigger)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.automation.trigger.add(
            code="call_done",
            name="Call completed",
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
        $result = $b24Service
            ->core
            ->call(
                'crm.automation.trigger.add',
                [
                    'CODE' => 'call_done',
                    'NAME' => 'Call completed',
                ]
            )
            ->getResponseData()
            ->getResult();

        // The SDK wraps the boolean result of the method in an array
        echo $result[0] ? 'Trigger saved' : 'Trigger not saved';
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding automation trigger: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.automation.trigger.add',
        {
            "CODE": 'call_done',
            "NAME": 'Call completed'
        },
        function(result)
        {
            if(result.error())
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
        'crm.automation.trigger.add',
        [
            'CODE' => 'call_done',
            'NAME' => 'Call completed'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.automation.trigger.add", b24.Params{
    	"CODE": "call_done",
    	"NAME": "Call completed",
    })
    if err != nil {
    	return fmt.Errorf("crm.automation.trigger.add: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start":1718884406.366687,
        "finish":1718884406.80718,
        "duration":0.4404928684234619,
        "processing":0.03356289863586426,
        "date_start":"2024-06-20T11:53:26+00:00",
        "date_finish":"2024-06-20T11:53:26+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if the trigger is registered or the name of an existing trigger with the same `CODE` is updated ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied! Application context required"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | Access denied. | The user does not have access to CRM ||
|| `403` | `ACCESS_DENIED` | Access denied! Admin permissions required | The method was called by a user who is not an administrator ||
|| `403` | `ACCESS_DENIED` | Access denied! Application context required | The method was called outside an application, for example via a webhook ||
|| `400` | Empty value | Empty trigger code! | The `CODE` parameter is not passed, is empty, or equals `0` ||
|| `400` | Empty value | Wrong trigger code! | `CODE` contains characters other than Latin letters, digits, and `.`, `-`, `_` ||
|| `400` | Empty value | Empty trigger name! | The `NAME` parameter is not passed, is empty, or equals `0` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-automation-trigger-execute.md)
- [{#T}](./crm-automation-trigger-list.md)
- [{#T}](./crm-automation-trigger-delete.md)