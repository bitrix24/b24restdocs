# Send a Message to a CRM Item Chat imopenlines.crm.message.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with read access to the CRM item linked to the chat

The method `imopenlines.crm.message.add` sends a message on behalf of an employee or bot to an open channel chat linked to a CRM item. The message is marked "Auto generated message".

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CRM_ENTITY_TYPE***
[`string`](../../../data-types.md) | Type of the CRM entity:
- `lead` — lead
- `deal` — deal
- `company` — company
- `contact` — contact

The value is case-insensitive ||
|| **CRM_ENTITY***
[`integer`](../../../data-types.md) | Identifier of the CRM entity linked to the chat.

You can obtain a list of entities of a specific CRM entity type using the method [crm.item.list](../../../crm/universal/crm-item-list.md) ||
|| **USER_ID***
[`integer`](../../../data-types.md) | Message sender identifier — the user or bot that must be a chat participant and have access to the CRM item.

The user identifier can be obtained using the [user.get](../../../user/user-get.md) or [user.search](../../../user/user-search.md) method.

The list of chatbots can be obtained using the [imbot.bot.list](../../../chat-bots/outdated/bots/imbot-bot-list.md) method ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | Identifier of the open channel chat linked to the CRM entity. The chat must have an ongoing dialog: an operator has taken it, and the dialog is neither closed nor waiting for the client's reply.

You can obtain the chat ID using the method [imopenlines.crm.chat.get](../chats/imopenlines-crm-chat-get.md) or [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md) ||
|| **MESSAGE***
[`string`](../../../data-types.md) | The text of the message that will be displayed in the chat ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CRM_ENTITY_TYPE":"lead","CRM_ENTITY":1195,"USER_ID":27,"CHAT_ID":1341,"MESSAGE":"Message text"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.crm.message.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CRM_ENTITY_TYPE":"lead","CRM_ENTITY":1195,"USER_ID":27,"CHAT_ID":1341,"MESSAGE":"Message text","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imopenlines.crm.message.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'imopenlines.crm.message.add',
        params: {
          CRM_ENTITY_TYPE: 'lead',
          CRM_ENTITY: 1195,
          USER_ID: 27,
          CHAT_ID: 1341,
          MESSAGE: 'Message text',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created message ID:', result)
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
      async function sendCrmMessage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imopenlines.crm.message.add',
            params: {
              CRM_ENTITY_TYPE: 'lead',
              CRM_ENTITY: 1195,
              USER_ID: 27,
              CHAT_ID: 1341,
              MESSAGE: 'Message text',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created message ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', sendCrmMessage)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imopenlines.crm.message.add(
            crm_entity_type="lead",
            crm_entity=1195,
            user_id=27,
            chat_id=1341,
            message="Message text",
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
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.crm.message.add',
                [
                    'CRM_ENTITY_TYPE' => 'lead',
                    'CRM_ENTITY' => 1195,
                    'USER_ID' => 27,
                    'CHAT_ID' => 1341,
                    'MESSAGE' => 'Message text',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.crm.message.add',
        {
            CRM_ENTITY_TYPE: 'lead',
            CRM_ENTITY: 1195,
            USER_ID: 27,
            CHAT_ID: 1341,
            MESSAGE: 'Message text',
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imopenlines.crm.message.add',
        [
            'CRM_ENTITY_TYPE' => 'lead',
            'CRM_ENTITY' => 1195,
            'USER_ID' => 27,
            'CHAT_ID' => 1341,
            'MESSAGE' => 'Message text',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Created message ID: ' . $result['result'];
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imopenlines.crm.message.add", b24.Params{
    	"CRM_ENTITY_TYPE": "lead",
    	"CRM_ENTITY":      1195,
    	"USER_ID":         27,
    	"CHAT_ID":         1341,
    	"MESSAGE":         "Message text",
    })
    if err != nil {
    	return fmt.Errorf("imopenlines.crm.message.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 19880117,
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+01:00",
        "date_finish": "2024-10-11T10:00:00+01:00",
        "operating_reset_at":1762349466,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created message in the chat ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "CHAT_NOT_IN_CRM",
    "error_description": "Chat does not belong to the CRM entity being checked"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_NOT_IN_CRM` | Chat does not belong to the CRM entity being checked | The chat is not linked to the CRM object or has no active dialog ||
|| `400` | `ERROR_ARGUMENT` | Argument 'CRM_ENTITY_TYPE' is null or empty | The required parameter `CRM_ENTITY_TYPE` is not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'CRM_ENTITY' is null or empty | The required parameter `CRM_ENTITY` is not provided ||
|| `400` | `ERROR_ARGUMENT` | The value of an argument 'CRM_ENTITY' has an invalid type | The `CRM_ENTITY` parameter is provided in an invalid format ||
|| `400` | `ERROR_ARGUMENT` | Argument 'USER_ID' is null or empty | The required parameter `USER_ID` is not provided ||
|| `400` | `ERROR_ARGUMENT` | The value of an argument 'USER_ID' has an invalid type | The `USER_ID` parameter is provided in an invalid format ||
|| `400` | `ERROR_ARGUMENT` | Argument 'CHAT_ID' is null or empty | The required parameter `CHAT_ID` is not provided ||
|| `400` | `ERROR_ARGUMENT` | The value of an argument 'CHAT_ID' has an invalid type | The `CHAT_ID` parameter is provided in an invalid format ||
|| `400` | `ERROR_ARGUMENT` | Argument 'MESSAGE' is null or empty | The required parameter `MESSAGE` is not provided ||
|| `400` | `MESSAGE_ADD_ERROR` | Message isn't added | The message was not sent, for example, the author `USER_ID` cannot write to this chat ||
|| `403` | `ACCESS_DENIED` | Access denied! You dont have access to this action | The current user does not have read permission for the CRM object, or an unknown type is passed in `CRM_ENTITY_TYPE` ||
|| `403` | `ACCESS_DENIED` | Access denied! User dont have access to this entity | The user `USER_ID` does not have read permission for the CRM object ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-message-quick-save.md)
