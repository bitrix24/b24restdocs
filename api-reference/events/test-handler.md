# How to Test Your Handler for Processing Bitrix24 Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A test handler helps verify that Bitrix24 can send an event to your server and that the server receives and retains the event data. To test, register a `ONAPPTEST` event handler using the [event.bind](./event-bind.md) method, then call the `event.test` method.

The scenario consists of four steps:

1. Create a `handler.php` file that saves the inbound request to a file
2. Register a `ONAPPTEST` event handler using the [event.bind](./event-bind.md) method
3. Call a test event using the `event.test` method
4. Verify that a file containing the event data appears in the `log` folder

## Prepare the Handler

To execute the scenario, you need:

- An application with OAuth authorization
- A public URL for the handler, accessible from an external network for GET and POST requests
- A `handler.php` file on your server
- A writable `log` folder located next to the `handler.php` file
- An OAuth access token to call the `event.bind` and `event.test` methods

Create a `handler.php` file on your server. Ensure the file is accessible from the internet. Create a `log` folder next to the file.

The code in the `handler.php` file saves the inbound request to a separate file:

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- PHP

    ```php
    <?
    file_put_contents(
        __DIR__ . '/log/' . time() . '.txt',
        var_export($_REQUEST, true)
    );
    ```

{% endlist %}

## Register a Test Event

Register the `ONAPPTEST` event using the [event.bind](./event-bind.md) method. Pass the public URL of the `handler.php` file in the `handler` parameter.

Replace the values in the examples:

- `https://example.com/handler.php` with your handler URL
- `**put_access_token_here**` with your OAuth access token
- `**put_your_bitrix24_address**` with your Bitrix24 address

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

- PHP

    ```php
    <?
    $eventBind = CRest::call(
        'event.bind',
        [
            'event' => 'ONAPPTEST',
            'handler' => 'https://example.com/handler.php'
        ]
    );
    if($eventBind['result'])
    {
        echo 'event bind successful';
    }
    ?>
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

The `event.test` method works only with application OAuth authorization. It is not suitable for incoming webhooks: with a different authorization type, the method returns an authorization type error.

In the example, the `any` parameter is used as a test value. After the call, it should appear in the saved request within the `data.QUERY` block.

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

- PHP

    ```php
    <?
    $result = CRest::call(
        'event.test',
        [
            'any' => 'data'
        ]
    );
    if($result['result'])
    {
        echo 'successful';
    }
    ?>
    ```

{% endlist %}

A successful call to the `event.test` method returns `1`.

```json
{
    "result": 1
}
```

## Verify the Result

Open the `log` folder located next to the `handler.php` file. If the scenario is executed successfully, a file containing the event data will appear in it.

The file should contain:

- `event` with the value `ONAPPTEST`
- `event_handler_id` with the identifier of the registered handler
- `data.QUERY.any` with the value `data`
- `auth` with the event authorization data

{% list tabs %}

- PHP

    ```php
    array (
        'event' => 'ONAPPTEST',
        'event_handler_id' => 1,
        'data' => 
        array (
            'QUERY' => 
            array (
                'any' => 'data',
            ),
            'LANGUAGE_ID' => 'en',
        ),
        'ts' => '1573120286',
        'auth' => array (...)
    )
    ```

{% endlist %}

If the file is created and contains `data.QUERY.any`, the handler is accessible from Bitrix24 and accepts event data.

## Errors and Diagnostics

If the `event.bind` method returns an error, check the registration parameters:

- `ERROR_EVENT_NOT_FOUND` — an incorrect event is specified in the `event` parameter. To test the handler, use `ONAPPTEST`
- `HANDLER` was not passed — specify a public URL for the `handler.php` file
- `Unable to set event handler` — check if a handler with the same URL is already registered
- Access error — check the OAuth token and the application context

If the `event.bind` method returns `false` or the `event.test` method returns a successful response, but the file does not appear in the `log` folder, check the handler:

- The `handler` URL is accessible from the internet and does not point to `localhost`
- The server accepts POST requests to the `handler.php` file
- The `log` folder exists alongside the `handler.php` file
- The web server has write permissions for the `log` folder
- There are no PHP errors in the handler code

## Key Considerations

The `event.test` method only verifies the delivery of a test event `ONAPPTEST`. For production events, use the event codes from the list returned by the [events](./events.md) method and register them using the [event.bind](./event-bind.md) method.

Do not save authorization tokens from the `auth` block into public logs. In the example, the handler records the entire request only for quick verification of event reception.

After verification, delete the files from the `log` folder or close external access to it. If the test handler is no longer needed, remove the subscription using the [event.unbind](./event-unbind.md) method.

## Continue Learning

- [{#T}](./event-bind.md)
- [{#T}](./events.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
