# Event on Task Addition OnTaskAdd

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The event is triggered after a task is created.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
array(
    'event' => 'ONTASKADD',
    'data' => array(
        'FIELDS_BEFORE' => 'undefined',
        'FIELDS_AFTER' => array('ID' => 123),
        'IS_ACCESSIBLE_BEFORE' => 'N',
        'IS_ACCESSIBLE_AFTER' => 'undefined',
    ),
    'ts' => '1466439714',
    'auth' => array(
        'access_token' => 's6p6eclrvim6da22ft9ch94ekreb52lv',
        'expires_in' => '3600',
        'scope' => 'crm',
        'domain' => 'some-domain.bitrix24.com',
        'server_endpoint' => 'https://oauth.bitrix.info/rest/',
        'status' => 'F',
        'client_endpoint' => 'https://some-domain.bitrix24.com/rest/',
        'member_id' => 'a223c6b3710f85df22e9377d6c4f7553',
        'refresh_token' => '4s386p3q0tr8dy89xvmt96234v3dljg8',
        'application_token' => '51856fefc120afa4b628cc82d3935cce',
        ),
)
```

{% include notitle [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Symbolic event code, in this case `OnTaskAdd`||
|| **data***
[`array`](../../data-types.md) | Array with data of the added task ||
|| **ts***
[`timestamp`](../../data-types.md) | Date and time of the event sent from the [event queue](../../events/index.md) ||
|| **auth***
[`array`](../../data-types.md) | Authorization parameters and data about the account where the event occurred ||
|#

### Parameter data[]

{% include notitle [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS_BEFORE***
[`undefined`\|`object`](../../data-types.md) | Fields of the task before the event (detailed description provided [below](#fields_before)). If no task fields are available, this field will contain the value `undefined` ||
|| **FIELDS_AFTER***
[`undefined`\|`object`](../../data-types.md) | Fields of the task after the event (detailed description provided [below](#fields_after)). If no task fields are available, this field will contain the value `undefined` ||
|| **IS_ACCESSIBLE_BEFORE***
[`string`](../../data-types.md) | Was the task readable before the event (detailed description provided [below](#is_accessible_before)) ||
|| **IS_ACCESSIBLE_AFTER***
[`string`](../../data-types.md) | Is the task readable after the event (detailed description provided [below](#is_accessible_after)) ||
|#

### Field FIELDS_BEFORE {#fields_before}

{% include notitle [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../../data-types.md) | Identifier of the created task ||
|#

### Field FIELDS_AFTER {#fields_after}

{% include notitle [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../../data-types.md) | Identifier of the created task ||
|#

### Field IS_ACCESSIBLE_BEFORE {#is_accessible_before}

{% include notitle [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IS_ACCESSIBLE_BEFORE***
[`string`](../../data-types.md) | Possible values:
- `Y` (Yes) — yes
- `N` (No) — no
- `undefined` — not defined or check not performed ||
|#

### Field IS_ACCESSIBLE_AFTER {#is_accessible_after}

{% include notitle [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IS_ACCESSIBLE_AFTER***
[`string`](../../data-types.md) | Possible values:
- `Y` (Yes) — yes
- `N` (No) — no
- `undefined` — not defined or check not performed ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (event.bind returns true on success)
    type BindEventResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<BindEventResult>({
        method: 'event.bind',
        params: {
          event: 'onTaskAdd',
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
      async function bindOnTaskAddEvent() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'event.bind',
            params: {
              event: 'onTaskAdd',
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

      document.addEventListener('DOMContentLoaded', bindOnTaskAddEvent)
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
                    'event'   => 'onTaskAdd',
                    'handler' => 'https://example.com/handler.php',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
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
            "event": "onTaskAdd",
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
            'event' => 'onTaskAdd',
            'handler' => 'https://example.com/handler.php'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./on-task-update.md)
- [{#T}](./on-task-delete.md)