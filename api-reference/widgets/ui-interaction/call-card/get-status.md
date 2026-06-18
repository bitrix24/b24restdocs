# Get Call Status getStatus

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `getStatus` method returns the current data of the call card.

{% note info "" %}

The method operates within the application context in the `CALL_CARD` placement.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT***
[`string`](../../../data-types.md) | The name of the interface command.

For this method — `getStatus` ||
|| **PARAMS***
[`object`](../../../data-types.md) | The parameters object for the command.

For this method, an empty object is passed: `{}` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"getStatus","PARAMS":{}}' \
    "https://**put_your_bitrix24_address**/rest/placement.call?auth=**put_access_token_here**"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type GetStatusResult = {
      CALL_ID: string
      PHONE_NUMBER: string
      LINE_NUMBER: string
      LINE_NAME: string
      CRM_ENTITY_TYPE: string
      CRM_ENTITY_ID: number
      CRM_ACTIVITY_ID: string
      CRM_BINDINGS: Array<{
        ENTITY_TYPE: string
        ENTITY_ID: number
      }>
      CALL_DIRECTION: string
      CALL_STATE: string
      CALL_LIST_MODE: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<GetStatusResult>({
        method: 'placement.call',
        params: {
          PLACEMENT: 'getStatus',
          PARAMS: {},
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.CALL_ID, result.CALL_STATE, result.PHONE_NUMBER)
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
      async function getCallStatus() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.call',
            params: {
              PLACEMENT: 'getStatus',
              PARAMS: {},
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.CALL_ID, result.CALL_STATE, result.PHONE_NUMBER)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCallStatus)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'placement.call',
                [
                    'PLACEMENT' => 'getStatus',
                    'PARAMS' => []
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'placement.call',
        {
            PLACEMENT: 'getStatus',
            PARAMS: {}
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'placement.call',
        [
            'PLACEMENT' => 'getStatus',
            'PARAMS' => (object)[]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

```json
{
    "CALL_ID": "E45D40253D1C2D2F.1774588815.822533",
    "PHONE_NUMBER": "+19999999666",
    "LINE_NUMBER": "reg151083",
    "LINE_NAME": "",
    "CRM_ENTITY_TYPE": "CONTACT",
    "CRM_ENTITY_ID": 797,
    "CRM_ACTIVITY_ID": "",
    "CRM_BINDINGS": [
        {
        "ENTITY_TYPE": "DEAL",
        "ENTITY_ID": 4615
        },
        {
        "ENTITY_TYPE": "COMPANY",
        "ENTITY_ID": 643
        }
    ],
    "CALL_DIRECTION": "outgoing",
    "CALL_STATE": "idle",
    "CALL_LIST_MODE": false
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **CALL_ID**
[`string`](../../../data-types.md) | The identifier of the call ||
|| **PHONE_NUMBER**
[`string`](../../../data-types.md) | The client's number ||
|| **LINE_NUMBER**
[`string`](../../../data-types.md) | The line number ||
|| **LINE_NAME**
[`string`](../../../data-types.md) | The name of the line ||
|| **CRM_ENTITY_TYPE**
[`string`](../../../data-types.md) | The type of the current CRM object ||
|| **CRM_ENTITY_ID**
[`integer`](../../../data-types.md) | The identifier of the current CRM object ||
|| **CRM_ACTIVITY_ID**
[`integer`](../../../data-types.md) | The identifier of the CRM activity ||
|| **CRM_BINDINGS**
[`object[]`](../../../data-types.md) | The bindings of the call to CRM objects [(detailed description)](#crm_bindings) ||
|| **CALL_DIRECTION**
[`string`](../../../data-types.md) | The direction of the call.

Possible values:

- `incoming` — incoming call
- `outgoing` — outgoing call
- `callback` — callback ||
|| **CALL_STATE**
[`string`](../../../data-types.md) | The state of the call.

Possible values:

- `idle` — no connection
- `connecting` — establishing connection
- `connected` — connection established ||
|| **CALL_LIST_MODE**
[`boolean`](../../../data-types.md) | Indicator of the dialing mode ||
|#

### CRM_BINDINGS Parameter {#crm_bindings}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE**
[`string`](../../../data-types.md) | The type of the CRM object ||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | The identifier of the CRM object ||
|#

## Error Handling

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Application context required"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Application context required | The method was called outside the application context in the `CALL_CARD` placement ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disable-auto-close.md)
- [{#T}](./enable-auto-close.md)
- [{#T}](./call-card-entity-changed.md)
- [{#T}](./call-card-before-close.md)
- [{#T}](./call-card-call-state-changed.md)
