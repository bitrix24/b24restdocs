# How to Test Your Handler for Processing Bitrix24 Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A test handler helps verify that Bitrix24 can send an event to your server and that the server receives and retains the event data. You do not need to wait for a real action in Bitrix24: the `ONAPPTEST` event is triggered manually with the `event.test` method.

The result of the scenario is a file in the `log` folder on your server holding the body of the POST request from Bitrix24: the event code, the transmitted data, and the `auth` block.

## Methods of the Scenario

#|
|| **Method** | **Role in the scenario** ||
|| [event.bind](./event-bind.md) | Registers a handler for the `ONAPPTEST` event and links the event code to the URL of your file ||
|| `event.test` | Sends the `ONAPPTEST` event to the registered URL. Every parameter passed to the method arrives in the handler within the `data.QUERY` block ||
|| [event.unbind](./event-unbind.md) | Removes the test handler after the check ||
|#

The scenario consists of five steps:

1. Create a `handler.php` file that saves the inbound request to a file
2. Register a `ONAPPTEST` event handler using the `event.bind` method
3. Call a test event using the `event.test` method
4. Verify that a file containing the event data appears in the `log` folder
5. Remove the test handler using the `event.unbind` method

## What You Need

- An application with [OAuth 2.0 authorization](../../settings/oauth/index.md). The `event.bind` and `event.test` methods work only with this authorization type, an [inbound webhook](../../local-integrations/local-webhooks.md) cannot call them
- An OAuth access token to call the methods. No additional [scope](../scopes/permissions.md) is required: `event.bind`, `event.test`, and the `ONAPPTEST` event are basic
- A public handler URL over the `http` or `https` protocol, accessible from an external network. Bitrix24 validates the URL on registration: the address must contain a domain name with a dot, so `localhost` and a bare IP address will not work
- A `handler.php` file on your server and a `log` folder next to it, writable by the web server
- A completed application installation. Until the installation is complete, events are not sent to the application even though `event.bind` and `event.test` return success. [Check the application installation](../../settings/app-installation/installation-finish.md)

## Prepare the Handler

Create a `handler.php` file on your server and make sure it opens at a public URL. The code saves the inbound request to a separate file and creates the `log` folder itself on the first run.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- PHP

    ```php
    <?php
    $logDir = __DIR__ . '/log';

    if (!is_dir($logDir)) {
        mkdir($logDir, 0775, true);
    }

    file_put_contents(
        $logDir . '/' . uniqid((string)time() . '-') . '.txt',
        var_export($_REQUEST, true)
    );
    ```

{% endlist %}

Bitrix24 sends the event as a POST request, so the data ends up in `$_REQUEST`. The file name is built from the request time plus a random suffix, so each event lands in its own file even when several are triggered within the same second.

{% note warning "" %}

The handler records the entire request, including the `auth` block with the application tokens, and the `log` folder sits in the web directory next to `handler.php`. Close the folder to external access before the first `event.test` call — otherwise the tokens are publicly available from the very first event. Use this code only for a one-time check: delete the files from the `log` folder right after it. Do not write tokens to the log in a production handler.

{% endnote %}

## Register a Test Event

Register the `ONAPPTEST` event using the [event.bind](./event-bind.md) method. Pass the public URL of the `handler.php` file in the `handler` parameter.

Replace the values in the examples:

- `https://example.com/handler.php` with your handler URL
- `**put_access_token_here**` with your OAuth access token
- `**put_your_bitrix24_address**` with your Bitrix24 address

The SDK tabs assume an already-created client. Read how to initialize it in the [{#T}](../../sdk/index.md) section.

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"event":"ONAPPTEST","handler":"https://example.com/handler.php","auth":"**put_access_token_here**"}' \
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
          event: 'ONAPPTEST',
          handler: 'https://example.com/handler.php',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('event.bind result:', result)
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
              event: 'ONAPPTEST',
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
          console.info('event.bind result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', bindEvent)
    </script>
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $eventBind = CRest::call(
        'event.bind',
        [
            'event' => 'ONAPPTEST',
            'handler' => 'https://example.com/handler.php'
        ]
    );

    if ($eventBind['result']) {
        echo 'event bind successful';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "event.bind", b24.Params{
    	"event":   "ONAPPTEST",
    	"handler": "https://example.com/handler.php",
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

A successful registration returns `true`.

```json
{
    "result": true
}
```

## Call the Test Event

Call the `event.test` method with arbitrary data. Bitrix24 will send the `ONAPPTEST` event to the URL you specified when registering the handler.

In the example, the `any` parameter with the value `data` is used as a test value. The method does not validate the set of parameters: any key-value pairs you pass are returned to the handler inside `data.QUERY`. They are what confirms that your call is the one that reached the server.

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"any":"data","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/event.test
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
        method: 'event.test',
        params: {
          any: 'data',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('event.test result:', result)
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
      async function testEvent() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'event.test',
            params: {
              any: 'data',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('event.test result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', testEvent)
    </script>
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'event.test',
        [
            'any' => 'data'
        ]
    );

    if ($result['result']) {
        echo 'successful';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "event.test", b24.Params{
    	"any": "data",
    })
    if err != nil {
    	return fmt.Errorf("event.test: %w", err)
    }

    var value int
    if err := json.Unmarshal(res.Result, &value); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("result:", value)
    ```

{% endlist %}

A successful call to the `event.test` method returns `1`. The response confirms only that Bitrix24 accepted the call and queued the event — verify the delivery by the file in the `log` folder.

```json
{
    "result": 1
}
```

## Verify the Result

Open the `log` folder located next to the `handler.php` file. If the scenario is executed successfully, a file containing the event data will appear in it.

The file content is the output of `var_export`, not executable code. The `auth` block is collapsed in the example, its keys are covered in the article [{#T}](./index.md#auth).

```
array (
    'event' => 'ONAPPTEST',
    'event_handler_id' => '1',
    'data' =>
    array (
        'QUERY' =>
        array (
            'any' => 'data',
        ),
        'LANGUAGE_ID' => 'en',
    ),
    'ts' => '1573120286',
    'auth' => array (...),
)
```

Compare the keys in the file against what you sent:

#|
|| **Key** | **What to check** ||
|| `event` | The value `ONAPPTEST` — the test event is the one that arrived ||
|| `event_handler_id` | Identifier of the subscription that Bitrix24 assigned when the handler was registered. It is not returned over REST — use it as a label in the logs to tell subscriptions apart ||
|| `data.QUERY` | Parameters of the `event.test` call. In the example this is `any` with the value `data` ||
|| `data.LANGUAGE_ID` | Interface language of the Bitrix24 account the event came from ||
|| `ts` | Date and time the event was sent from the queue, in Unix format ||
|| `auth` | Event authorization data. The structure is described in the article [{#T}](./index.md#auth) ||
|#

If the file is created and contains `data.QUERY.any`, the handler is accessible from Bitrix24 and accepts event data. You can test handlers for real events the same way: replace `ONAPPTEST` with the event code you need and perform the action in Bitrix24 that triggers it.

## Errors and Diagnostics

If any method of the scenario returns an error, look up its code in the table. The `WRONG_AUTH_TYPE` and `expired_token` codes apply to all three calls, the rest apply to `event.bind`.

#|
|| **Code** | **Cause and what to do** ||
|| `WRONG_AUTH_TYPE` | The method was called by a webhook. Repeat the call with an application OAuth token ||
|| `expired_token` | The token has expired. Refresh it with the `refresh_token` and repeat the call — read how to do this in the article [{#T}](../../settings/oauth/auto-renewal.md) ||
|| `ERROR_EVENT_NOT_FOUND` | The `event` parameter specifies an event that is not in the list available to the application. To test the handler, use `ONAPPTEST`; the full list of events is returned by the [events](./events.md) method ||
|| `ERROR_WRONG_HANDLER_URL` | The `handler` parameter received an address without a domain name — for example, `localhost` or an empty string. Specify a public URL with a dot in the domain name ||
|| `ERROR_UNSUPPORTED_PROTOCOL` | The handler address does not use the `http` or `https` protocol ||
|| `ERROR_ARGUMENT` with the text `Argument 'EVENT' is null or empty` or `Argument 'HANDLER' is null or empty` | The `event` or `handler` parameter was not passed ||
|| `ERROR_CORE` with the text `Handler already binded` | A handler with the same event-URL pair is already registered. Check the subscriptions with the [event.get](./event-get.md) method and remove the extra one with [event.unbind](./event-unbind.md) if needed ||
|#

If the `event.bind` method returned `true` and the `event.test` method returned `1`, but the file does not appear in the `log` folder, check the following in order:

- Wait a few seconds and refresh the folder. The event travels through a queue, so it does not arrive instantly
- The application installation is complete. Until it is, both methods return success but events are not sent to the application
- The `handler` URL opens from an external network, not only from your local network
- The server accepts POST requests to the `handler.php` file and does not respond with a redirect
- The `log` folder exists alongside the `handler.php` file and the web server has write permissions for it
- There are no PHP errors in the handler code — check the web server error log

After each fix, repeat the scenario from the `event.test` call — there is no need to register the handler again.

If Bitrix24 is deployed in a closed network, a network policy may block the handler. Check the access rules in the article [{#T}](../../settings/cloud-and-on-premise/network-access.md).

## Remove the Test Handler

After the check, remove the subscription so that the test event stops reaching your server. Pass the same `event` and `handler` to [event.unbind](./event-unbind.md) that you used for the registration.

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"event":"ONAPPTEST","handler":"https://example.com/handler.php","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/event.unbind
    ```

{% endlist %}

In the response, `count` is the number of removed handlers. A value of `0` means no subscription with that event-URL pair was found.

```json
{
    "result": {
        "count": 1
    }
}
```

## Key Considerations

The `event.test` method only verifies the delivery of a test event `ONAPPTEST`. For production events, use the event codes from the list returned by the [events](./events.md) method and register them using the [event.bind](./event-bind.md) method.

In a production handler, verify the `application_token` instead of recording the entire request — this is how you confirm that the event came from Bitrix24. Read how to do this in the article [{#T}](./safe-event-handlers.md).

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./event-bind.md)
- [{#T}](./events.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
