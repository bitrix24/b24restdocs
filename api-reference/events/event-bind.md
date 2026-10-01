# Register a New Event Handler event.bind

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `event.bind` method registers a new event handler.

The method works only within the context of [application](../../settings/app-installation/index.md) authorization. When called through a webhook, the method returns the `WRONG_AUTH_TYPE` error.

The following restrictions apply to a user without administrator rights:

- offline events are unavailable: a subscription with `event_type=offline` returns the `ACCESS_DENIED` error
- only the user's own identifier can be specified in `auth_type`; for another user, the method returns the `ACCESS_DENIED` error

{% note info %}

Bitrix24 sends the event data in a POST request to the handler URL, so the address must be accessible from the internet. How to test a handler is described in the article [{#T}](./test-handler.md).

{% endnote %}

The method can be called via [BX24.callBind](../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-bind.md).

{% note info %}

When an application is deleted, its event handlers are removed; when it is updated, they are retained. If the installer of a new version registers the same handler again, the method returns the `ERROR_CORE` error. Before registering, check the current handlers with the [event.get](./event-get.md) method.

{% endnote %}

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../settings/app-installation/installation-finish.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../data-types.md) | Event code, such as `ONCRMLEADADD`. The event must belong to the application's scope or be a basic event; otherwise, the method returns the `ERROR_EVENT_NOT_FOUND` error. The list of available events is returned by the [events](./events.md) method ||
|| **handler***
[`string`](../data-types.md) | Handler URL with the `http` or `https` scheme. The host name must contain a dot, so `localhost` is not accepted. Required for online events; ignored when `event_type=offline` ||
|| **auth_type**
[`integer`](../data-types.md) | Identifier of the user under whom the event handler is authorized. By default, for an administrator, it is the user whose action triggered the event; for a user without administrator rights, it is that user. Ignored when `event_type=offline` ||
|| **event_type**
[`string`](../data-types.md) | Subscription type: `online` or `offline`. Default is `online`. With `offline`, the event is placed in the [offline event queue](./offline-events.md) ||
|| **auth_connector**
[`string`](../data-types.md) | Source key for [offline events](./offline-events.md). This key creates a separate queue that does not receive changes made by requests of the application itself with the same `auth_connector`. The same value is passed to the `event.offline.*` methods. The parameter is not available on all plans: check it with the [feature.get](../common/system/feature-get.md) method using the `rest_auth_connector` code; otherwise, the method returns the `WRONG_LICENSE` error ||
|| **options**
[`object`](../data-types.md) | Additional settings for the registered event. The set of fields depends on the event.

For the `ONOFFLINEEVENT` event, the `minTimeout` field is supported — the minimum interval between notifications in seconds. Default is 1. More details in the article [{#T}](./on-offline-event.md#min-timeout) ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "event": "ONCRMLEADADD",
        "handler": "https://www.my-domain.com/handler/",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/event.bind
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
        method: 'event.bind',
        params: {
          event: 'ONCRMLEADADD',
          handler: 'https://www.my-domain.com/handler/',
          auth_type: 15,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Handler registered:', result)
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
      async function bindEvent() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'event.bind',
            params: {
              event: 'ONCRMLEADADD',
              handler: 'https://www.my-domain.com/handler/',
              auth_type: 15,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Handler registered:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', bindEvent)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.event.bind(
            event="ONCRMLEADADD",
            handler="https://www.my-domain.com/handler/",
            auth_type=15,
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'event.bind',
        [
            'event' => 'ONCRMLEADADD',
            'handler' => 'https://www.my-domain.com/handler/',
            'auth_type' => 15
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "event.bind", b24.Params{
    	"event":   "ONCRMLEADADD",
    	"handler": "https://www.my-domain.com/handler/",
    })
    if err != nil {
    	return fmt.Errorf("event.bind: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1721296536.908506,
        "finish": 1721296537.007365,
        "duration": 0.09885907173156738,
        "processing": 0.03251290321350098,
        "date_start": "2024-07-18T11:55:36+02:00",
        "date_finish": "2024-07-18T11:55:37+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Success of execution ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error":"ERROR_EVENT_NOT_FOUND",
    "error_description":"Event not found"
}
```

{% include notitle [Error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_EVENT_NOT_FOUND` | Event not found | The event was not found or does not belong to the application's scope ||
|| `400` | `ERROR_ARGUMENT` | Argument 'EVENT' is null or empty | The `event` parameter is not passed ||
|| `400` | `ERROR_ARGUMENT` | Argument 'HANDLER' is null or empty | The `handler` parameter is not passed for an online event ||
|| `400` | `ERROR_ARGUMENT` | ```Value must be one of {online|offline}``` | An invalid `event_type` value is passed ||
|| `400` | `ERROR_ARGUMENT` | Offline event cannot be registered for this event. | The event cannot be received offline, such as `ONOFFLINEEVENT` ||
|| `400` | `ERROR_WRONG_HANDLER_URL` | Wrong handler URL | The handler URL has no host, or the host name has no dot ||
|| `400` | `ERROR_UNSUPPORTED_PROTOCOL` | Unsupported handler protocol | The handler URL scheme is neither `http` nor `https` ||
|| `400` | `ERROR_CORE` | Unable to set event handler: Handler already binded | This handler is already registered ||
|| `400` | `ERROR_CORE` | Unable to set event handler: Process of binding the handler has already started | The same handler is being registered by a parallel request ||
|| `403` | `ACCESS_DENIED` | Access denied! Offline events binding requires administrator access rights | An offline event handler is being registered by a user without administrator rights ||
|| `403` | `ACCESS_DENIED` | Access denied! Event binding with AUTH_TYPE requires administrator access rights | A user without administrator rights specified another user in `auth_type` ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | The method was called outside an application, for example, through a webhook ||
|| `403` | `WRONG_LICENSE` | This feature is not enabled for the current license: auth_connector | `auth_connector` is passed, but the plan does not support source keys ||
|#

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./events.md)
- [{#T}](./test-handler.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)
