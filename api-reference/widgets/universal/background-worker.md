# Background Handler on Every Page PAGE_BACKGROUND_WORKER

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../scopes/permissions.md)

Bitrix24 loads the handler of this placement on every page — in a hidden frame, without a visible interface element. The user neither opens nor sees the widget: the application code runs in the background on any page the employee has open.

The placement is needed where the application has to react not to a click but to an external event: receive a signal from its own backend through [interactive interaction](../../../settings/interactivity/index.md), show an incoming call in a telephony integration, open the application slider with the [openApplication](../bx24-widget-methods.md) method.

The placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method. Registration requires the `OPTIONS[errorHandlerUrl]` parameter — the address where Bitrix24 reports that the handler has been deactivated.

{% note info "" %}

The embedding is not displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#|
|| **Embedding Code** | **Location** ||
|| `PAGE_BACKGROUND_WORKER` | A hidden frame on every Bitrix24 page ||
|#

### When the Handler is Called

The handler loads on every Bitrix24 page load. A page opened in a slider is a separate document, so the handler loads there once more, already with its own address in the `URI` key.

This leads to the main requirement for the handler: it has to respond quickly. If the response takes longer than five seconds and this happens more than ten times a day on the same Bitrix24, the handler registration is deleted.

Bitrix24 informs the application about the deletion: a request with the error `ERROR_PLACEMENT_LOADING_OVERTIME` and a description of the exceeded loading time arrives at the address from `OPTIONS[errorHandlerUrl]`. The request is sent without authorization tokens. To bring the widget back, the application registers the handler again with the [placement.bind](../placement-bind.md) method.

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

```php
Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 588b8a98e848778a4ffb38fbcf70f2b9
    [AUTH_ID] => 4172bb660070f28d001e30ba00000001f0f107c42ca5bd5f61030c5d9c3e4d60
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 31f1e2660070f28d001e30ba00000001f0f107b1918506d8a2ed9ecf76e8fdac
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => ec1b2074a9d3f5c81b6e40d27a95cf38
    [APPLICATION_SCOPE] => placement
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => PAGE_BACKGROUND_WORKER
    [PLACEMENT_OPTIONS] => {"ID":"PAGE_BACKGROUND_WORKER","URI":"\/company\/personal\/user\/1\/blog\/"}
)
```

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the call context.

#|
|| **Parameter** | **Description** ||
|| **ID***
[`string`](../../data-types.md) | The placement code, always equal to `PAGE_BACKGROUND_WORKER` ||
|| **URI***
[`string`](../../data-types.md) | The path with the query string of the page where the handler loaded. It tells the application where the user currently is ||
|#

## OPTIONS when registering via placement.bind

For `PAGE_BACKGROUND_WORKER`, the `placement.bind` method supports one `OPTIONS` parameter.

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **errorHandlerUrl***
[`string`](../../data-types.md) | The address where Bitrix24 reports that the handler registration has been deleted.

The parameter is mandatory: without it `placement.bind` returns the error `EMPTY_ERROR_HANDLER_URL`. Other `OPTIONS` keys are not retained — [placement.get](../placement-get.md) returns only `errorHandlerUrl`
||
|#

An application registers one handler for this placement. A repeated `placement.bind` call returns the error `ERROR_PLACEMENT_MAX_COUNT` — to change the handler address, first remove the registration with the [placement.unbind](../placement-unbind.md) method.

### Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PLACEMENT": "PAGE_BACKGROUND_WORKER",
        "HANDLER": "https://your-domain.com/widgets/background-handler.php",
        "OPTIONS": {
          "errorHandlerUrl": "https://your-domain.com/widgets/background-error.php"
        },
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/placement.bind
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
        method: 'placement.bind',
        params: {
          PLACEMENT: 'PAGE_BACKGROUND_WORKER',
          HANDLER: 'https://your-domain.com/widgets/background-handler.php',
          OPTIONS: {
            errorHandlerUrl: 'https://your-domain.com/widgets/background-error.php',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Placement bound successfully:', result)
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
      async function bindBackgroundWorker() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.bind',
            params: {
              PLACEMENT: 'PAGE_BACKGROUND_WORKER',
              HANDLER: 'https://your-domain.com/widgets/background-handler.php',
              OPTIONS: {
                errorHandlerUrl: 'https://your-domain.com/widgets/background-error.php',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Placement bound successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', bindBackgroundWorker)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'placement.bind',
                [
                    'PLACEMENT' => 'PAGE_BACKGROUND_WORKER',
                    'HANDLER' => 'https://your-domain.com/widgets/background-handler.php',
                    'OPTIONS' => [
                        'errorHandlerUrl' => 'https://your-domain.com/widgets/background-error.php',
                    ],
                ]
            );

        $result = $response->getResponseData()->getResult();
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error binding placement: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'placement.bind',
        {
            PLACEMENT: 'PAGE_BACKGROUND_WORKER',
            HANDLER: 'https://your-domain.com/widgets/background-handler.php',
            OPTIONS: {
                errorHandlerUrl: 'https://your-domain.com/widgets/background-error.php'
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
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
        'placement.bind',
        [
            'PLACEMENT' => 'PAGE_BACKGROUND_WORKER',
            'HANDLER' => 'https://your-domain.com/widgets/background-handler.php',
            'OPTIONS' => [
                'errorHandlerUrl' => 'https://your-domain.com/widgets/background-error.php',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## A Handler for a Single User

`PAGE_BACKGROUND_WORKER` is the only placement that supports the `USER_ID` parameter of the [placement.bind](../placement-bind.md) method. A handler registered with `USER_ID` loads only on the pages of that user. This is how background code is connected for those who need it — for example, only for telephony operators.

The limit of one handler is counted separately for the general registration and for each user, so a personal handler and a handler for all employees can be registered at the same time.

## Relationship With Other Objects

**Call card.** From the background handler, the application controls the call card: it changes the card state, buttons, and title, and subscribes to the operator actions. The methods and events are in the [{#T}](../ui-interaction/page-background-worker/index.md) section, and the whole scenario is in the [{#T}](../ui-interaction/page-background-worker/webrtc-scenario.md) article.

**Signals from the backend.** The handler receives messages from the server side of the application through the [interactive interaction](../../../settings/interactivity/index.md) mechanism and opens the application interface based on them with the [JavaScript methods for widgets](../bx24-widget-methods.md).

**User.** The identifier for the `USER_ID` parameter used when registering a personal handler is returned by the methods of the [{#T}](../../user/index.md) section.

## Common Mistakes

#|
|| **Mistake** | **Solution** ||
|| `placement.bind` returns `EMPTY_ERROR_HANDLER_URL` | Pass `OPTIONS[errorHandlerUrl]`: without an address for deactivation messages the placement is not registered ||
|| `placement.bind` returns `ERROR_PLACEMENT_MAX_COUNT` | The handler is already registered. Remove the old registration with the [placement.unbind](../placement-unbind.md) method ||
|| The handler stopped being called and is missing from `placement.get` | The registration was deleted because of slow loading. Speed up the handler response and register it again ||
|| The handler is called several times on one screen | Pages in sliders are separate documents, and the handler loads again in each of them. Check `URI` if the scenario has to run only once ||
|#

{% note tip "Typical Use-Cases and Scenarios" %}

- [{#T}](../ui-interaction/page-background-worker/webrtc-scenario.md)

{% endnote %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./app-url.md)
- [{#T}](../placement-bind.md)
- [{#T}](../placement-unbind.md)
- [{#T}](../ui-interaction/page-background-worker/index.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../../../settings/interactivity/index.md)
