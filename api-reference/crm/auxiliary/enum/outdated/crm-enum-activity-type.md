# Get Enumeration Elements "Activity Types" crm.enum.activitytype

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.activity.todo.*](../../../timeline/activities/todo/index.md).

{% endnote %}

The method `crm.enum.activitytype` returns a list of types for the `TYPE_ID` field of [activities](../../../timeline/activities/index.md).

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.enum.activitytype
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/crm.enum.activitytype
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each ActivityTypeItem returned in result[]
    type ActivityTypeItem = {
      ID: number
      NAME: string
      SYMBOL_CODE: string | null
      SYMBOL_CODE_SHORT: string | null
    }

    try {
      const response = await $b24.actions.v2.call.make<ActivityTypeItem[]>({
        method: 'crm.enum.activitytype',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Activity types count:', result.length, 'first item:', result[0])
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
      async function fetchActivityTypes() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.enum.activitytype',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Activity types count:', result.length, 'first item:', result[0])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchActivityTypes)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.enum.activitytype',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling crm.enum.activitytype: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.enum.activitytype",
        {},
        function(result) {
            if (result.error())
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
        'crm.enum.activitytype',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
"result": [
    {
     "ID": 0,
     "NAME": "",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 1,
     "NAME": "Meeting",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 2,
     "NAME": "Call",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 3,
     "NAME": "Task",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 4,
     "NAME": "E-mail",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 5,
     "NAME": "Action",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 6,
     "NAME": "User Action",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    }
],
"time": {
    "start": 1750152062.819392,
    "finish": 1750152062.85849,
    "duration": 0.03909802436828613,
    "processing": 0.0004849433898925781,
    "date_start": "2025-06-17T12:21:02+02:00",
    "date_finish": "2025-06-17T12:21:02+02:00",
    "operating_reset_at": 1750152662,
    "operating": 0
}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../../data-types.md) | Array of activity types [(detailed description)](#result) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result array {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the activity type ||
|| **NAME**
[`string`](../../../../data-types.md) | Name of the activity type ||
|| **SYMBOL_CODE**
[`string`](../../../../data-types.md) | Symbolic code ||
|| **SYMBOL_CODE_SHORT**
[`string`](../../../../data-types.md) | Short symbolic code ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./../index.md)