# Get a List of Actions for the Task task.item.getallowedactions

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns an array of identifiers for the allowed actions on the task.

{% note warning "DEPRECATED" %}

The development of this method has been halted. Use [tasks.task.getAccess](../../tasks-task-get-access.md).

{% endnote %}

## Method Parameters

#| 
|| **Name** | **Description** ||
|| **TASKID** | Task identifier ||
|#

## Mapping Table of Identifiers and Allowed Actions for the Task

#| 
|| **Identifier** | **Description** ||
|| `1` | ACTION_ACCEPT ||
|| `2` | ACTION_DECLINE ||
|| `3` | ACTION_COMPLETE ||
|| `4` | ACTION_APPROVE ||
|| `5` | ACTION_DISAPPROVE ||
|| `6` | ACTION_START ||
|| `7` | ACTION_DELEGATE ||
|| `8` | ACTION_REMOVE ||
|| `9` | ACTION_EDIT ||
|| `10` | ACTION_DEFER ||
|| `11` | ACTION_RENEW ||
|| `12` | ACTION_CREATE ||
|| `13` | ACTION_CHANGE_DEADLINE ||
|| `14` | ACTION_CHECKLIST_ADD_ITEMS ||
|| `15` | ACTION_ELAPSED_TIME_ADD ||
|| `16` | ACTION_CHANGE_DIRECTOR ||
|| `17` | ACTION_PAUSE ||
|| `18` | ACTION_START_TIME_TRACKING ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.getallowedactions
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.item.getallowedactions
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // The method returns an array of allowed action IDs for the task
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AllowedActionsResult = number[]

    try {
      const response = await $b24.actions.v2.call.make<AllowedActionsResult>({
        method: 'task.item.getallowedactions',
        params: {
          TASKID: 13,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Allowed action IDs:', result)
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
      async function getAllowedActions() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.item.getallowedactions',
            params: {
              TASKID: 13,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Allowed action IDs:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getAllowedActions)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.item.getallowedactions',
                [13]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting allowed actions: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.item.getallowedactions',
        [13],
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.item.getallowedactions',
        [
            'TASKID' => 13
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}