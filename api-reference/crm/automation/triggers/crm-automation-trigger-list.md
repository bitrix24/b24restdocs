# Get the list of triggers crm.automation.trigger.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.automation.trigger.list` returns the triggers that the current application registered using the [crm.automation.trigger.add](./crm-automation-trigger-add.md) method. Pass the trigger codes from the response to the [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) and [crm.automation.trigger.delete](./crm-automation-trigger-delete.md) methods. For example, before deleting a trigger, the application finds its `CODE` in the list.

The method works only in the context of an [application](../../../../settings/app-installation/index.md).

## Method Parameters

No parameters. The method returns the whole list at once, without pagination: it ignores `start`, `filter`, and `order`.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.automation.trigger.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each trigger returned in result[]
    type TriggerItem = {
      NAME: string
      CODE: string
    }

    // crm.automation.trigger.list returns all triggers of the current application at once
    try {
      const response = await $b24.actions.v2.call.make<TriggerItem[]>({
        method: 'crm.automation.trigger.list',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Triggers:', result.length, result)
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
      async function loadTriggerList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.automation.trigger.list returns all triggers of the current application at once
          const response = await $b24.actions.v2.call.make({
            method: 'crm.automation.trigger.list',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Triggers:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', loadTriggerList)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.automation.trigger.list().response
        for trigger in bitrix_response.result:
            print(trigger["CODE"], trigger["NAME"])
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
            ->getCRMScope()
            ->trigger()
            ->list();

        foreach ($result->getTriggers() as $trigger) {
            echo $trigger->CODE . ' — ' . $trigger->NAME . PHP_EOL;
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching automation triggers: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.automation.trigger.list',
        {},
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
        'crm.automation.trigger.list',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.automation.trigger.list", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.automation.trigger.list: %w", err)
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
    "result": [
        {
            "NAME": "Payment received",
            "CODE": "payment_received"
        },
        {
            "NAME": "Call completed",
            "CODE": "call_done"
        }
    ],
    "time": {
        "start": 1790705980,
        "finish": 1790705980.904287,
        "duration": 0.9042870998382568,
        "processing": 0,
        "date_start": "2026-09-29T18:19:40+00:00",
        "date_finish": "2026-09-29T18:19:40+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | Array of the current application's triggers [(detailed description)](#trigger). If the application has not registered any triggers, the method returns an empty array `[]` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### result Array Element {#trigger}

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the trigger, for example `Call completed`. In the CRM automation settings, it is preceded by the application name in square brackets, or by the application ID if the application has no name ||
|| **CODE**
[`string`](../../../data-types.md) | Trigger code within the application. Pass it to the [crm.automation.trigger.execute](./crm-automation-trigger-execute.md) and [crm.automation.trigger.delete](./crm-automation-trigger-delete.md) methods ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied! Admin permissions required"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | Access denied. | The user does not have access to CRM ||
|| `403` | `ACCESS_DENIED` | Access denied! Admin permissions required | The method was called by a user who is not an administrator ||
|| `403` | `ACCESS_DENIED` | Access denied! Application context required | The method was called outside an application, for example via a webhook ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-automation-trigger-add.md)
- [{#T}](./crm-automation-trigger-execute.md)
- [{#T}](./crm-automation-trigger-delete.md)
