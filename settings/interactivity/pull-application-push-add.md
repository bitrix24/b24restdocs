# Send a Push Notification to the Application Users pull.application.push.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`pull`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: only a Bitrix24 administrator

The method `pull.application.push.add` sends a push notification to the application users. The notification arrives in the Bitrix24 mobile app on behalf of your application, so the application has to have its name filled in.

A push notification does not go to a Push&Pull channel: it is delivered by the mobile app, and the recipient will see the notification only if the app is installed. To update the interface of an open application, use [pull.application.event.add](./pull-application-event-add.md) — that method puts an event into the channel, where the client reads it from.

{% note info "" %}

The method works only in the context of an [application](../app-installation/index.md). The request is executed with the application OAuth token and the `pull` scope, and a webhook does not create such a context.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID**
[`integer`](../../api-reference/data-types.md) \| [`string`](../../api-reference/data-types.md) \| [`integer[]`](../../api-reference/data-types.md) | A user identifier or an array of user identifiers the push notification is sent to. Always pass the parameter: there is no validation on the REST side, and without `USER_ID` the method returns a successful response but no notification goes out.

The method parses a string as JSON, so `"577"` and `"[1, 2, 3]"` are accepted as well.

`USER_ID` can be retrieved:
- with the [user.get](../../api-reference/user/user-get.md) method
- with the [user.current](../../api-reference/user/user-current.md) method for the current user

The method does not limit the number of recipients and discards duplicate identifiers ||
|| **TEXT**^*^
[`string`](../../api-reference/data-types.md) | The text of the push notification.

The size of a push notification is limited to 4 KB. Bitrix24 truncates a longer text.

The method returns an error if `TEXT` is not passed, is empty, or equals `0` ||
|| **AVATAR**
[`string`](../../api-reference/data-types.md) | The absolute URL of an image for the push notification.

The image is downloaded by the Bitrix24 mobile app when it receives the notification. There is no check that the URL is reachable: if the image fails to load, the notification arrives without it and the method returns a successful response.

Bitrix24 parses the value as a URL and reassembles it, so a string that could not be parsed will not reach the device ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

An example of sending a push notification to the application users, where:
- `USER_ID` — a user identifier or an array of user identifiers
- `TEXT` — the text of the push notification
- `AVATAR` — the URL of an image for the push notification

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "USER_ID": [1, 2, 3],
        "TEXT": "Hello, world!",
        "AVATAR": "https://example.com/images/avatar.png",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/pull.application.push.add.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'pull.application.push.add',
        params: {
          USER_ID: [1, 2, 3],
          TEXT: 'Hello, world!',
          AVATAR: 'https://example.com/images/avatar.png',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Push accepted:', result)
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
      async function sendApplicationPush() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'pull.application.push.add',
            params: {
              USER_ID: [1, 2, 3],
              TEXT: 'Hello, world!',
              AVATAR: 'https://example.com/images/avatar.png',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Push accepted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', sendApplicationPush)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.pull.application.push.add(
            [1, 2, 3],
            text="Hello, world!",
            avatar="https://example.com/images/avatar.png",
        ).response
        print(bitrix_response.result)
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
                'pull.application.push.add',
                [
                    'USER_ID' => [1, 2, 3],
                    'TEXT' => 'Hello, world!',
                    'AVATAR' => 'https://example.com/images/avatar.png',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sending push notification: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'pull.application.push.add',
        {
            USER_ID: [1, 2, 3],
            TEXT: 'Hello, world!',
            AVATAR: 'https://example.com/images/avatar.png'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'pull.application.push.add',
        [
            'USER_ID' => [1, 2, 3],
            'TEXT' => 'Hello, world!',
            'AVATAR' => 'https://example.com/images/avatar.png',
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1743495945,
        "finish": 1743495945.285066,
        "duration": 0.2850658893585205,
        "processing": 0.008597135543823242,
        "date_start": "2025-04-01T11:52:25+02:00",
        "date_finish": "2025-04-01T11:52:25+02:00",
        "operating_reset_at": 1743496545,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../api-reference/data-types.md) | The indicator that the notification has been accepted for sending. The method returns `true` regardless of whether the notification reached the device.

There is no other payload in the response: the method does not return a notification identifier, and the delivery status cannot be learned from the response ||
|| **time**
[`time`](../../api-reference/data-types.md#time) | Information about the request execution time. The composition of the fields — [Time Object](../../api-reference/data-types.md#time) ||
|#

### Why the Notification Did Not Arrive

If the notification did not arrive, check the delivery conditions:

- at least one positive identifier is left in `USER_ID`: the method discards zeros and negative values
- the Bitrix24 mobile app is installed on the recipient's device and they are signed in to it
- the application has its name filled in for the current language, otherwise the method would have returned `EMPTY_APP_NAME`
- push notifications are enabled in Bitrix24 itself

## Error Handling

HTTP Status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Send push notifications available only for application authorization."
}
```

HTTP Status: **400**

```json
{
    "error": "ACCESS_ERROR",
    "error_description": "You do not have access to send push notifications"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Send push notifications available only for application authorization. | The method was called outside the context of an application, for example, through a webhook ||
|| `400` | `ACCESS_ERROR` | You do not have access to send push notifications | A user without administrator permissions is trying to send a push notification ||
|| `400` | `TEXT_ERROR` | Text can't be empty | The `TEXT` parameter is not passed, is empty, or equals `0` ||
|| `400` | `EMPTY_APP_NAME` | For send push-notification application name can't be empty | The application has no name filled in. A name in the default language does not clear the error — Bitrix24 checks only the name for the current language ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./push-and-pull-in-browser.md)
- [{#T}](./custom-push-and-pull-client.md)
- [{#T}](./pull-application-config-get.md)
- [{#T}](./pull-application-event-add.md)
