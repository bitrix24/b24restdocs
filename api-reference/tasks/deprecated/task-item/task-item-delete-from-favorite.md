# Remove Task from Favorites task.item.deletefromfavorite

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method removes a task from Favorites.

{% note warning "DEPRECATED" %}

Development of this method has been halted. Use [tasks.task.favorite.remove](../../tasks-task-favorite-remove.md).

{% endnote %}

## Method Parameters

#|
|| **Name** | **Description** ||
|| **auth** | Authorization token ||
|| **TASK_ID** | Task identifier ||
|| **PARAMS** | This parameter contains the key `AFFECT_CHILDREN`. It indicates whether to add the subtasks of this task to Favorites ||
|#

It is essential to maintain the order of parameters in the request. If this order is violated, the request will be executed with errors.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASK_ID":10,"PARAMS":{"AFFECT_CHILDREN":"Y"}}' \
    https://your-domain.com/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.deletefromfavorite
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASK_ID":10,"PARAMS":{"AFFECT_CHILDREN":"Y"},"auth":"mqa17fnd5cth4rpwtizyl49tbnzp7omf"}' \
    https://your-domain.com/rest/task.item.deletefromfavorite
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // TODO: verify API version
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DeleteFromFavoriteResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<DeleteFromFavoriteResult>({
        method: 'task.item.deletefromfavorite',
        params: {
          TASK_ID: 10,
          PARAMS: {
            AFFECT_CHILDREN: 'Y',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Task removed from favorites:', result)
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
      async function deleteFromFavorite() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.item.deletefromfavorite',
            params: {
              TASK_ID: 10,
              PARAMS: {
                AFFECT_CHILDREN: 'Y',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Task removed from favorites:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteFromFavorite)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.item.deletefromfavorite',
                [
                    'TASK_ID' => 10,
                    'PARAMS' => [
                        'AFFECT_CHILDREN' => "Y"
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting task from favorites: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.item.deletefromfavorite',
        {
            TASK_ID: 10,
            PARAMS: {
                AFFECT_CHILDREN: "Y"
            }
        },
        function(result) {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.item.deletefromfavorite',
        [
            'TASK_ID' => 10,
            'PARAMS' => [
                'AFFECT_CHILDREN' => 'Y'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}