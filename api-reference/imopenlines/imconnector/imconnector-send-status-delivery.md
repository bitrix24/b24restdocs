# Update Status "Delivered" imconnector.send.status.delivery

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imconnector.send.status.delivery` confirms in Bitrix24 that an outgoing message from an open line has been delivered to an external system.

Call the method after the application has received the [OnImConnectorMessageAdd](./events/on-im-connector-message-add.md) event and delivered the message to the external channel. The message is not resent — the method only records the delivery result.

When processing the "delivered" status, the message is also marked as read — on behalf of the interlocutor from the external channel.

The method is not applicable for incoming messages from an external system to an open line.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONNECTOR***
[`string`](../../data-types.md) | String code of the connector specified in the `ID` parameter when calling [imconnector.register](./imconnector-register.md) ||
|| **LINE***
[`integer`](../../data-types.md) | Identifier of the open line.

The identifier can be obtained using the methods [imopenlines.config.get](../openlines/imopenlines-config-get.md) and [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md) ||
|| **MESSAGES***
[`array`](../../data-types.md) | Array of delivery statuses. Each element of the array is an object with blocks `im`, `message`, `chat`.

The structure of the object is described in detail [below](#messages) ||
|#

### MESSAGES Parameter {#messages}

#|
|| **Name**
`type` | **Description** ||
|| **im***
[`object`](../../data-types.md) | Internal identifiers of the message in Bitrix24 [(detailed description)](#messages-im) ||
|| **message**
[`object`](../../data-types.md) | Message data in the external system [(detailed description)](#messages-message) ||
|| **chat**
[`object`](../../data-types.md) | Chat data in the external system [(detailed description)](#messages-chat) ||
|#

#### im Object {#messages-im}

#|
|| **Name**
`type` | **Description** ||
|| **message_id***
[`integer`](../../data-types.md) | Identifier of the message in Bitrix24 for which the delivery status needs to be set. This is the only value Bitrix24 uses to find the message ||
|| **chat_id**
[`integer`](../../data-types.md) | Identifier of the open line chat in Bitrix24 for the outgoing message. It does not affect the status processing ||
|#

The application receives the fields `im.message_id` and `im.chat_id` in the [OnImConnectorMessageAdd](./events/on-im-connector-message-add.md) event and retains them to pass in the delivery status later. The external identifiers `message.id` and `chat.id` do not replace them.

#### message Object {#messages-message}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`array`](../../data-types.md) | Array of external message identifiers for which the delivery status is being sent. Even for a single message, pass an array, e.g., `["ext-msg-1007"]`.

Bitrix24 retains the entire array passed in the parameters of the open line message — it can later be used to match the Bitrix24 message with the external system message ||
|| **date**
[`integer`](../../data-types.md) | Delivery time of the message in Unix Timestamp in seconds. It does not affect the status processing ||
|#

#### chat Object {#messages-chat}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Identifier of the chat or channel in the external system. It does not affect the status processing.

Pass the same value as in `chat.id` of the method [imconnector.send.messages](./imconnector-send-messages.md) so that the data in the two systems matches ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CONNECTOR": "myconnector",
        "LINE": 107,
        "MESSAGES": [
          {
            "im": {
              "chat_id": 323,
              "message_id": 85911
            },
            "message": {
              "id": ["ext-msg-1007"],
              "date": 1738065600
            },
            "chat": {
              "id": "channel-123"
            }
          }
        ],
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imconnector.send.status.delivery
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SendStatusDeliveryResult = {
      SUCCESS: boolean
      DATA: unknown[]
    }

    try {
      const response = await $b24.actions.v2.call.make<SendStatusDeliveryResult>({
        method: 'imconnector.send.status.delivery',
        params: {
          CONNECTOR: 'myconnector',
          LINE: 107,
          MESSAGES: [
            {
              im: { chat_id: 323, message_id: 85911 },
              message: { id: ['ext-msg-1007'], date: 1738065600 },
              chat: { id: 'channel-123' },
            },
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Delivery status accepted:', result.SUCCESS, 'data:', result.DATA)
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
      async function sendStatusDelivery() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imconnector.send.status.delivery',
            params: {
              CONNECTOR: 'myconnector',
              LINE: 107,
              MESSAGES: [
                {
                  im: { chat_id: 323, message_id: 85911 },
                  message: { id: ['ext-msg-1007'], date: 1738065600 },
                  chat: { id: 'channel-123' },
                },
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Delivery status accepted:', result.SUCCESS, 'data:', result.DATA)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', sendStatusDelivery)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imconnector.send.status.delivery(
            connector="myconnector",
            line=107,
            messages=[
                {
                    "im": {
                        "chat_id": 323,
                        "message_id": 85911,
                    },
                    "message": {
                        "id": [
                            "ext-msg-1007",
                        ],
                        "date": 1738065600,
                    },
                    "chat": {
                        "id": "channel-123",
                    },
                },
            ],
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
    $result = $b24Service->core->call(
        'imconnector.send.status.delivery',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'MESSAGES' => [
                [
                    'im' => ['chat_id' => 323, 'message_id' => 85911],
                    'message' => ['id' => ['ext-msg-1007'], 'date' => 1738065600],
                    'chat' => ['id' => 'channel-123'],
                ],
            ],
        ]
    );
    ```

- BX24.js

    ```js
    BX24.callMethod(
      'imconnector.send.status.delivery',
      {
        CONNECTOR: 'myconnector',
        LINE: 107,
        MESSAGES: [
          {
            im: { chat_id: 323, message_id: 85911 },
            message: { id: ['ext-msg-1007'], date: 1738065600 },
            chat: { id: 'channel-123' },
          },
        ],
      },
      function(result) {
        console.log(result.data());
      }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'imconnector.send.status.delivery',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'MESSAGES' => [
                [
                    'im' => ['chat_id' => 323, 'message_id' => 85911],
                    'message' => ['id' => ['ext-msg-1007'], 'date' => 1738065600],
                    'chat' => ['id' => 'channel-123'],
                ],
            ],
        ]
    );
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imconnector.send.status.delivery", b24.Params{
    	"CONNECTOR": "myconnector",
    	"LINE":      107,
    	"MESSAGES": []b24.Params{
    		{
    			"im": b24.Params{
    				"chat_id":    323,
    				"message_id": 85911,
    			},
    			"message": b24.Params{
    				"id":   []string{"ext-msg-1007"},
    				"date": 1738065600,
    			},
    			"chat": b24.Params{
    				"id": "channel-123",
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("imconnector.send.status.delivery: %w", err)
    }

    var item struct {
    	Success bool `json:"SUCCESS"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Success)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "SUCCESS": true,
        "DATA": []
    },
    "time": {
        "start": 1773267900.126,
        "finish": 1773267900.489,
        "duration": 0.3630001544952393,
        "processing": 0.0884850025177002,
        "date_start": "2026-03-11T14:25:00+03:00",
        "date_finish": "2026-03-11T14:25:00+03:00",
        "operating_reset_at": 1773268500,
        "operating": 0.0884850025177002
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Result of processing the delivery statuses [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **SUCCESS**
[`boolean`](../../data-types.md) | Returns `true` if the required parameters are provided and the connector provider is found.

The value `true` does not confirm that the status has been applied to a specific message: Bitrix24 skips unprocessed `MESSAGES` elements without an error ||
|| **DATA**
[`array`](../../data-types.md) | Always an empty array: the method does not return data on individual messages ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method Application context required"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | Method called not in the context of the OAuth application ||
|| `400` | `ERROR_ARGUMENT` | Argument 'CONNECTOR' is null or empty | `CONNECTOR` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'LINE' is null or empty | `LINE` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'MESSAGES' is null or empty | The `MESSAGES` key is not provided. An empty array does not cause an error ||
|| `400` | `IMCONNECTOR_NO_CORRECT_PROVIDER` | Unable to find a suitable provider for the connector | A connector with this code is not registered in Bitrix24 ||
|#

The method returns an error only for problems with the entire request: a parameter is missing, the connector provider is not found, or the call was made outside the application context. Bitrix24 skips an individual `MESSAGES` element without an error if:

- the element has no `im.message_id`
- there is no message with this `im.message_id` in Bitrix24
- the message does not belong to an open line chat

The delivery status is applied only while the message is in the sending state: Bitrix24 keeps it in this state until it receives the first delivery confirmation. When the method is called again for the same message, the message is marked as read once more, but the delivery status and the external identifier are no longer retained.

Unlike [imconnector.send.messages](./imconnector-send-messages.md), this method does not check the `LINE` parameter, so it does not return the `NOT_ACTIVE_LINE` error.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imconnector-register.md)
- [{#T}](./imconnector-activate.md)
- [{#T}](./imconnector-status.md)
- [{#T}](./imconnector-connector-data-set.md)
- [{#T}](./imconnector-list.md)
- [{#T}](./imconnector-unregister.md)
- [{#T}](./imconnector-send-messages.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-chat-name-set.md)
- [{#T}](../../../tutorials/openlines/example-connector.md)