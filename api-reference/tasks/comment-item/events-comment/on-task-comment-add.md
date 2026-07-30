# Event on Adding a Comment OnTaskCommentAdd

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONTASKCOMMENTADD` event is triggered after a new comment is added to a task.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

When working with the old task detail form before module version `tasks 25.700.0`:

```json
{
    "event": "ONTASKCOMMENTADD",
    "data": {
        "FIELDS_BEFORE": "undefined",
        "FIELDS_AFTER": {
            "ID": 123,
            "TASK_ID": 555
        },
        "IS_ACCESSIBLE_BEFORE": "undefined",
        "IS_ACCESSIBLE_AFTER": "undefined"
    },
    "ts": "1466439714",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "task",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "F",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "a223c6b3710f85df22e9377d6c4f7553",
        "refresh_token": "4s386p3q0tr8dy89xvmt96234v3dljg8",
        "application_token": "51856fefc120afa4b628cc82d3935cce"
    }
}
```

When working with the new task detail form with chat from module version `tasks 25.700.0`:

```json
{
    "event": "ONTASKCOMMENTADD",
    "data": {
        "FIELDS_BEFORE": "undefined",
        "FIELDS_AFTER": {
            "ID": 0,
            "TASK_ID": 555,
            "MESSAGE_ID": 1458
        },
        "IS_ACCESSIBLE_BEFORE": "undefined",
        "IS_ACCESSIBLE_AFTER": "undefined"
    },
    "ts": "1466439714",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "task",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "F",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "a223c6b3710f85df22e9377d6c4f7553",
        "refresh_token": "4s386p3q0tr8dy89xvmt96234v3dljg8",
        "application_token": "51856fefc120afa4b628cc82d3935cce"
    }
}
```

{% include notitle [Note on parameters](../../../../_includes/required.md) %}

#|
|| **parameter**
`type` | **Description** ||
|| **event***
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONTASKCOMMENTADD` ||
|| **data***
[`object`](../../../data-types.md) | An object containing data about the comment addition event.

The structure is described [below](#data) ||
|| **ts***
[`timestamp`](../../../data-types.md) | Date and time of the event sent from the [event queue](../../../events/index.md) ||
|| **auth***
[`object`](../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter Data[] {#data}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS_BEFORE***
[`undefined`\|`object`](../../../data-types.md) | Fields of the comment and task before the event (detailed description provided [below](#fields_before)). If no task fields are available, this field will contain the value `undefined` ||
|| **FIELDS_AFTER***
[`undefined`\|`object`](../../../data-types.md) | Fields of the comment and task after the event (detailed description provided [below](#fields_after)). If no task fields are available, this field will contain the value `undefined` ||
|| **IS_ACCESSIBLE_BEFORE***
[`string`](../../../data-types.md) | Whether the task was accessible for reading before the event (detailed description provided [below](#is_accessible_before)) ||
|| **IS_ACCESSIBLE_AFTER***
[`string`](../../../data-types.md) | Whether the task became accessible for reading after the event (detailed description provided [below](#is_accessible_after)) ||
|#

### Field FIELDS_BEFORE {#fields_before}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../../../data-types.md) | Identifier of the created comment. `'ID' => 0` is returned when using the [new task detail form](../../tasks-new.md) with module version `tasks 25.700.0` ||
|| **TASK_ID***
[`integer`](../../../data-types.md) | Identifier of the task to which the comment was added ||
|| **MESSAGE_ID**
[`integer`](../../../data-types.md) | Identifier of the sent message in the task chat, returned when using the [new task detail form](../../tasks-new.md) with module version `tasks 25.700.0` ||
|#

### Field FIELDS_AFTER {#fields_after}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../../../data-types.md) | Identifier of the created comment. `'ID' => 0` is returned when using the [new task detail form](../../tasks-new.md) with module version `tasks 25.700.0` ||
|| **TASK_ID***
[`integer`](../../../data-types.md) | Identifier of the task to which the comment was added ||
|| **MESSAGE_ID**
[`integer`](../../../data-types.md) | Identifier of the sent message in the task chat, returned when using the [new task detail form](../../tasks-new.md) with module version `tasks 25.700.0` ||
|#

### Field IS_ACCESSIBLE_BEFORE {#is_accessible_before}

#|
|| **Name**
`type` | **Description** ||
|| **IS_ACCESSIBLE_BEFORE***
[`string`](../../../data-types.md) | Possible values:
- `Y` (Yes) — yes
- `N` (No) — no
- `undefined` — not defined or check was not performed ||
|#

### Field IS_ACCESSIBLE_AFTER {#is_accessible_after}

#|
|| **Name**
`type` | **Description** ||
|| **IS_ACCESSIBLE_AFTER***
[`string`](../../../data-types.md) | Possible values:
- `Y` (Yes) — yes
- `N` (No) — no
- `undefined` — not defined or check was not performed ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../_includes/auth-params-in-events.md) %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (event.bind returns true on success)
    type EventBindResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<EventBindResult>({
        method: 'event.bind',
        params: {
          event: 'OnTaskCommentAdd',
          handler: 'https://example.com/handler.php',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Event bound successfully:', result)
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
      async function bindTaskCommentAddEvent() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'event.bind',
            params: {
              event: 'OnTaskCommentAdd',
              handler: 'https://example.com/handler.php',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Event bound successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', bindTaskCommentAddEvent)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'event.bind',
                [
                    'event'   => 'OnTaskCommentAdd',
                    'handler' => 'https://example.com/handler.php',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // The data processing logic you need
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error binding event: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'event.bind',
        {
            "event": "OnTaskCommentAdd",
            "handler": "https://example.com/handler.php"
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'event.bind',
        [
            'event' => 'OnTaskCommentAdd',
            'handler' => 'https://example.com/handler.php'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](./on-task-comment-update.md)
- [{#T}](./on-task-comment-delete.md)
