# Execute the Trigger crm.automation.trigger.execute

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.automation.trigger.execute` notifies CRM automation that an application trigger has fired for an object. If this trigger is linked to a stage or status in the automation settings, it can move the object to that stage or status. For example, a telephony application executes the `call_done` trigger after a call, and the deal moves to the "In Progress" stage.

The method works only in the context of an [application](../../../../settings/app-installation/index.md). The trigger must first be registered using the [crm.automation.trigger.add](./crm-automation-trigger-add.md) method and linked to a stage in the automation settings — the workflow is described in the [triggers overview](./index.md).

{% note warning "" %}

A `true` response does not confirm the stage change. The method returns `true` even if the stage has not changed:

- the trigger is not linked to a stage, or its conditions are not met
- the object is already at the stage the trigger is linked to
- the trigger's stage comes earlier in the pipeline than the object's current stage, and moving to a previous stage is not allowed in the trigger settings
- there is no object with this `OWNER_ID`

To check the result, retrieve the object's stage using the [crm.item.get](../../universal/crm-item-get.md) method.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../../data-types.md) | Trigger code that the application passed during registration, for example `call_done` ||
|| **OWNER_TYPE_ID***
[`integer`](../../../data-types.md) | Type of the CRM object according to the [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) reference, for example `2` — deal.

Triggers are available for leads, deals, estimates, invoices, and SPAs. If you pass a contact or a company, the trigger fires for the objects linked to them, such as deals ||
|| **OWNER_ID***
[`integer`](../../../data-types.md) | Identifier of the CRM object, such as a deal. It is returned by the [crm.item.list](../../universal/crm-item-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"call_done","OWNER_TYPE_ID":2,"OWNER_ID":6,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.automation.trigger.execute
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
        method: 'crm.automation.trigger.execute',
        params: {
          CODE: 'call_done',
          OWNER_TYPE_ID: 2,
          OWNER_ID: 6,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Trigger event sent:', result)
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
      async function executeAutomationTrigger() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.automation.trigger.execute',
            params: {
              CODE: 'call_done',
              OWNER_TYPE_ID: 2,
              OWNER_ID: 6,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Trigger event sent:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', executeAutomationTrigger)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.automation.trigger.execute(
            code="call_done",
            owner_type_id=2,
            owner_id=6,
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
                'crm.automation.trigger.execute',
                [
                    'CODE'          => 'call_done',
                    'OWNER_TYPE_ID' => 2,
                    'OWNER_ID'      => 6,
                ]
            )
            ->getResponseData()
            ->getResult();

        // The SDK wraps the boolean result of the method in an array.
        // true does not confirm the stage change: read the item to check it
        echo $result[0] ? 'Trigger event sent' : 'Trigger event not sent';
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error executing automation trigger: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.automation.trigger.execute',
        {
            CODE: 'call_done',
            OWNER_TYPE_ID: 2,
            OWNER_ID: 6
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
        'crm.automation.trigger.execute',
        [
            'CODE' => 'call_done',
            'OWNER_TYPE_ID' => 2,
            'OWNER_ID' => 6
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.automation.trigger.execute", b24.Params{
    	"CODE":          "call_done",
    	"OWNER_TYPE_ID": 2,
    	"OWNER_ID":      6,
    })
    if err != nil {
    	return fmt.Errorf("crm.automation.trigger.execute: %w", err)
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
        "start": 1790706808,
        "finish": 1790706808.400356,
        "duration": 0.4003560543060303,
        "processing": 0,
        "date_start": "2026-09-29T18:33:28+00:00",
        "date_finish": "2026-09-29T18:33:28+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if Bitrix24 accepted the trigger event ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Incorrect parameter OWNER_TYPE_ID."
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
|| `400` | Empty value | Trigger with code call_done is not registered. | The current application has no trigger with the code from `CODE`. The error text contains the passed code instead of `call_done` ||
|| `400` | Empty value | Incorrect parameter OWNER_TYPE_ID. | CRM has no object type with this `OWNER_TYPE_ID` ||
|| `400` | Empty value | Incorrect parameter OWNER_ID. | `OWNER_ID` is not passed or is not greater than zero ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-automation-trigger-add.md)
- [{#T}](./crm-automation-trigger-list.md)
- [{#T}](./crm-automation-trigger-delete.md)