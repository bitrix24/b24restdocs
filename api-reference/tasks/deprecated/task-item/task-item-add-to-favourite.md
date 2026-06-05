# Add Task to Favorites task.item.addtofavorite

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- Parameter types are not specified
- Parameter requirements are not indicated
- No response in case of error
- No response in case of success

{% endnote %}

{% endif %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds a task to Favorites.

{% note warning "DEPRECATED" %}

Development of this method has been halted. Use [tasks.task.favorite.add](../../tasks-task-favorite-add.md).

{% endnote %}

## Method Parameters

#| 
|| **Name** | **Description** ||
|| **auth** | Authorization token ||
|| **TASK_ID** | Task identifier ||
|| **PARAMS** | The parameter contains the key `AFFECT_CHILDREN`. It indicates whether to add subtasks of this task to Favorites ||
|#

Maintaining the order of parameters in the request is mandatory. If violated, the request will be executed with errors.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASK_ID":10,"PARAMS":{"AFFECT_CHILDREN":"Y"}}' \
    https://your-domain.com/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.addtofavorite
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASK_ID":10,"PARAMS":{"AFFECT_CHILDREN":"Y"},"auth":"mqa17fnd5cth4rpwtizyl49tbnzp7omf"}' \
    https://your-domain.com/rest/task.item.addtofavorite
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // TODO: verify result shape — no response example is provided on this page
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AddToFavouriteResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<AddToFavouriteResult>({
        method: 'task.item.addtofavorite',
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
        console.info('Task added to favourites:', result)
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
      async function addTaskToFavourite() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.item.addtofavorite',
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
          console.info('Task added to favourites:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addTaskToFavourite)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.item.addtofavorite',
                [
                    'TASK_ID' => 10,
                    'PARAMS'  => [
                        'AFFECT_CHILDREN' => 'Y',
                    ],
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
        echo 'Error adding task to favorites: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "task.item.addtofavorite",
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
        'task.item.addtofavorite',
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