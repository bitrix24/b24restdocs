# Get Chats for CRM Object imopenlines.crm.chat.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to the CRM object

The method `imopenlines.crm.chat.get` retrieves a list of chats associated with a CRM object.

## Method Parameters

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CRM_ENTITY_TYPE***
[`string`](../../../data-types.md) | Type of the CRM object. Possible values:
- `lead` — lead
- `deal` — deal
- `company` — company
- `contact` — contact
||
|| **CRM_ENTITY***
[`integer`](../../../data-types.md) | Identifier of the CRM object.

You can obtain the identifier using the universal method [to get a list of CRM entities](../../../crm/universal/crm-item-list.md) ||
|| **ACTIVE_ONLY**
[`char`](../../../data-types.md) | Flag to return only active chats. Possible values:
- `Y` — return only active chats
- `N` — return all chats

Default is `Y`
||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CRM_ENTITY_TYPE":"lead","CRM_ENTITY":1205,"ACTIVE_ONLY":"N"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.crm.chat.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CRM_ENTITY_TYPE":"lead","CRM_ENTITY":1205,"ACTIVE_ONLY":"N","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imopenlines.crm.chat.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each Chat returned in result[]
    type Chat = {
      CHAT_ID: string
      CONNECTOR_ID: string
      CONNECTOR_TITLE: string
    }

    try {
      const response = await $b24.actions.v2.call.make<Chat[]>({
        method: 'imopenlines.crm.chat.get',
        params: {
          CRM_ENTITY_TYPE: 'lead',
          CRM_ENTITY: 1205,
          ACTIVE_ONLY: 'N',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Chats count:', result.length, 'First chat ID:', result[0]?.CHAT_ID)
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
      async function getCrmChats() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imopenlines.crm.chat.get',
            params: {
              CRM_ENTITY_TYPE: 'lead',
              CRM_ENTITY: 1205,
              ACTIVE_ONLY: 'N',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Chats count:', result.length, 'First chat ID:', result[0]?.CHAT_ID)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCrmChats)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.crm.chat.get',
                [
                    'CRM_ENTITY_TYPE' => 'lead',
                    'CRM_ENTITY' => 1205,
                    'ACTIVE_ONLY' => 'N',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.crm.chat.get',
        {
            CRM_ENTITY_TYPE: 'lead',
            CRM_ENTITY: 1205,
            ACTIVE_ONLY: 'N'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
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
        'imopenlines.crm.chat.get',
        [
            'CRM_ENTITY_TYPE' => 'lead',
            'CRM_ENTITY' => 1205,
            'ACTIVE_ONLY' => 'N',
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "CHAT_ID": "1763",
            "CONNECTOR_ID": "livechat",
            "CONNECTOR_TITLE": "Live Chat"
        }
    ],
    "time": {
        "start": 1773758908,
        "finish": 1773758908.592458,
        "duration": 0.5924580097198486,
        "processing": 0,
        "date_start": "2026-03-17T17:48:28+01:00",
        "date_finish": "2026-03-17T17:48:28+01:00",
        "operating_reset_at": 1773759508,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md) | List of chat objects [(detailed description)](#result-item).

Returns an empty array `"result":[]` if no object with the specified `CRM_ENTITY` exists ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result-item}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID**
[`string`](../../../data-types.md) | Identifier of the chat ||
|| **CONNECTOR_ID**
[`string`](../../../data-types.md) | Identifier of the connector ||
|| **CONNECTOR_TITLE**
[`string`](../../../data-types.md) | Title of the connector ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument CRM_ENTITY is null or empty"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `ACCESS_DENIED` | Access denied! You don't have access to this action | User does not have access to the CRM object ||
|| `400` | `ERROR_ARGUMENT` | Argument CRM_ENTITY_TYPE is null or empty | Required parameter `CRM_ENTITY_TYPE` is not provided or is empty ||
|| `400` | `ERROR_ARGUMENT` | Argument CRM_ENTITY is null or empty | Required parameter `CRM_ENTITY` is not provided or is empty ||
|| `400` | `ERROR_ARGUMENT` | The value of an argument CRM_ENTITY has an invalid type | Parameter `CRM_ENTITY` is provided in an incorrect format ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./imopenlines-crm-chat-get-last-id.md)
- [{#T}](./imopenlines-crm-chat-user-add.md)
- [{#T}](./imopenlines-crm-chat-user-delete.md)