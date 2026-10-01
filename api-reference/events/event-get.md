# Retrieve a List of Registered Event Handlers event.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `event.get` method returns a list of event handlers that the application registered with the [event.bind](./event-bind.md) method.

The method works only in the context of authorizing the [application](../../settings/app-installation/index.md). When called through a webhook, it returns the `WRONG_AUTH_TYPE` error.

An administrator receives all handlers of the application, including those registered by other users. For a user without administrator permissions, the method returns only the handlers whose `auth_type` contains that user's ID.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/event.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each event handler returned in result[]
    type EventHandlerItem = {
      event: string,
      handler?: string,
      auth_type?: string,
      connector_id?: string,
      offline: number,
    }

    try {
      const response = await $b24.actions.v2.call.make<EventHandlerItem[]>({
        method: 'event.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(
          'Registered event handlers:',
          result.map(h => `${h.event} → ${h.handler}`)
        )
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
      async function getEventHandlers() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'event.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(
            'Registered event handlers:',
            result.map(h => `${h.event} → ${h.handler}`)
          )
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getEventHandlers)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.event.get().response
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
        $eventService = $serviceBuilder->getMainScope()->event();
        $result = $eventService->get();
        $eventHandlers = $result->getEventHandlers();
        foreach ($eventHandlers as $handler) {
            print("Event: " . $handler->event . "\n");
            print("Handler: " . $handler->handler . "\n");
            print("Auth Type: " . $handler->auth_type . "\n");
            print("Offline: " . $handler->offline . "\n");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "event.get",
        {},
        function(result)
        {
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
        'event.get',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "event.get", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("event.get: %w", err)
    }

    var items []struct {
    	Event       string `json:"event"`
    	Handler     string `json:"handler"`
    	AuthType    string `json:"auth_type"`
    	ConnectorID string `json:"connector_id"`
    	Offline     int    `json:"offline"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.Event, it.Handler)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "event": "ONCRMLEADADD",
            "handler": "https:\/\/www.my-domain.com\/handler\/",
            "auth_type": "0",
            "offline": 0
        },
        {
            "event": "ONCRMLEADADD",
            "handler": "https:\/\/www.my-domain.com\/handler\/",
            "auth_type": "15",
            "offline": 0
        },
        {
            "event": "ONCRMDEALUPDATE",
            "connector_id": "",
            "offline": 1
        }
    ],
    "time": {
        "start": 1721297941.536696,
        "finish": 1721297941.661148,
        "duration": 0.12445211410522461,
        "processing": 0.0029609203338623047,
        "date_start": "2024-07-18T12:19:01+02:00",
        "date_finish": "2024-07-18T12:19:01+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | Event handlers of the application in the order of registration [(detailed description)](#handler). The method returns all handlers in a single response, without pagination. If there are no handlers, the method returns an empty array ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

#### Handler List Item {#handler}

The set of fields depends on the subscription type.

#|
|| **Name**
`type` | **Description** ||
|| **event**
[`string`](../data-types.md) | Event code in uppercase, for example `ONCRMLEADADD` ||
|| **handler**
[`string`](../data-types.md) | Handler URL. Online subscriptions only ||
|| **auth_type**
[`string`](../data-types.md) | ID of the user on whose behalf the handler receives authorization. `0` means the user whose action triggered the event. Online subscriptions only ||
|| **connector_id**
[`string`](../data-types.md) | The `auth_connector` source key specified when subscribing. An empty string if no source is specified. [Offline subscriptions](./offline-events.md) only ||
|| **offline**
[`integer`](../data-types.md) | Subscription type: `0` — online subscription, `1` — offline subscription ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method"
}
```

{% include notitle [Error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | The method was called outside an application, for example, through a webhook ||
|#

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./events.md)
- [{#T}](./event-bind.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)
