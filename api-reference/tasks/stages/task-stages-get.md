# Get the List of Kanban Stages or "My Plan" task.stages.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: 
> - any user for "My Plan" stages
> - any user with group access for Kanban stages

This method retrieves the Kanban stages or "My Plan".

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **entityId*** 
[`integer`](../../data-types.md) | Identifier of the object.

Possible values:
- `ID` of the group — the method will retrieve the Kanban stages of the group. An access error will be returned if the permission level is insufficient.
- `0` — the method will retrieve the stages of "My Plan" for the current user. ||
|| **isAdmin** 
[`boolean`](../../data-types.md) | If set to `true`, permission checks will not occur, provided that the requester is an administrator of the account. ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "entityId": 0
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/task.stages.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "entityId": 0
    }' \
    https://your-domain.bitrix24.com/rest/task.stages.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each stage in the result map
    type StageInfo = {
      ID: string
      TITLE: string
      SORT: string
      COLOR: string
      SYSTEM_TYPE: string | null
      ENTITY_ID: string
      ENTITY_TYPE: string
      ADDITIONAL_FILTER: unknown[]
      TO_UPDATE: unknown[]
      TO_UPDATE_ACCESS: null
    }

    // result is a map of stage ID → StageInfo
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type StagesGetResult = Record<string, StageInfo>

    try {
      const response = await $b24.actions.v2.call.make<StagesGetResult>({
        method: 'task.stages.get',
        params: {
          entityId: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Stages:', Object.values(result).map(s => `${s.ID}: ${s.TITLE}`))
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
      async function getTaskStages() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.stages.get',
            params: {
              entityId: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Stages:', Object.values(result).map(s => `${s.ID}: ${s.TITLE}`))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTaskStages)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.stages.get',
                [
                    'entityId' => $entityId,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting task stages: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const entityId = 0;
    BX24.callMethod(
        'task.stages.get',
        {
            entityId: entityId,
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $entityId = 0;

    // executing the request to the REST API
    $result = CRest::call(
        'task.stages.get',
        [
            'entityId' => $entityId
        ]
    );

    // Processing the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "5": {
         "ID": "5",
         "TITLE": "Not Planned",
         "SORT": "100",
         "COLOR": "00C4FB",
         "SYSTEM_TYPE": "NEW",
         "ENTITY_ID": "1",
         "ENTITY_TYPE": "U",
         "ADDITIONAL_FILTER": [],
         "TO_UPDATE": [],
         "TO_UPDATE_ACCESS": null
        },
        "6": {
         "ID": "6",
         "TITLE": "I Will Do It This Week",
         "SORT": "200",
         "COLOR": "47D1E2",
         "SYSTEM_TYPE": null,
         "ENTITY_ID": "1",
         "ENTITY_TYPE": "U",
         "ADDITIONAL_FILTER": [],
         "TO_UPDATE": [],
         "TO_UPDATE_ACCESS": null
        }
    }
}
```

## Returned Data

#| 
|| **Field**
`type` | **Description** ||
|| **result** 
`object` | An object containing data about the Kanban / My Plan stages, with stage IDs as keys. ||
|| **ID** 
`integer` | Identifier of the stage. ||
|| **TITLE** 
`string` | Title. ||
|| **SORT** 
`integer` | Sorting. ||
|| **COLOR** 
`string` | Color in RGB format. ||
|| **SYSTEM_TYPE** 
`string` | System type. Possible values: `NEW`, `PROGRESS`, `WORK`, `REVIEW`, `FINISH`. ||
|| **ENTITY_ID** 
`integer` | Identifier of the object, i.e., group or user. ||
|| **ENTITY_TYPE** 
`string` | Type of the object. `U` for user, `G` for group. ||
|| **ADDITIONAL_FILTER** 
`array` | Additional filters. 

System parameter. Always has the value of an empty array. ||
|| **TO_UPDATE** 
`array` | Array of elements to update.

System parameter. Always has the value of an empty array. ||
|| **TO_UPDATE_ACCESS** 
`null` | Functions applied to the task when moving to this stage.

System parameter. Always has the value of `null`. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "You cannot view stages in this group."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | You cannot view stages in this group. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-stages-add.md)
- [{#T}](./task-stages-update.md)
- [{#T}](./task-stages-can-move-task.md)
- [{#T}](./task-stages-move-task.md)
- [{#T}](./task-stages-delete.md)