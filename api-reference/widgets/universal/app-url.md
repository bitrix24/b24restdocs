# Opening the Application via a Link REST_APP_URI

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../scopes/permissions.md)

The placement has no button of its own in the interface. The handler is called when the user follows a link of the form `/marketplace/view/#APP_CODE#/` that the application itself placed in the content: in a chat message, a feed comment, a task description. The application opens in a slider over the page the user came from.

Custom parameters can be added directly to the link: they reach the handler in `PLACEMENT_OPTIONS`. This way a single application opens different screens: a document card, a report, an approval form.

The placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget is not displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#|
|| **Placement Code** | **Location** ||
|| `REST_APP_URI` | A slider that opens when the user follows the link `/marketplace/view/#APP_CODE#/` ||
|#

### How to Build the Link

`#APP_CODE#` is the application code, not the handler registration identifier:

- for a mass-market application — the symbolic code from the application card in the Developer's area
- for a local application — the `client_id` from the application settings in the *Developer resources* section, for example `local.66ba434d853c87.18550109`

Pass your own parameters in the `params` key: `/marketplace/view/#APP_CODE#/?params[docId]=42`. The application defines the key names itself, and the values reach the handler as strings.

The link works everywhere Bitrix24 renders an internal address as a link and opens it in a slider. The handler receives the address of the original page in the `URI` key, so it shows which section the user came from.

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

```php
Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 9ecab44f06b9efb6c37d7b02180422b2
    [AUTH_ID] => 913374660070f28d001e30ba00000001f0f1073c8a5e2b7d94f16c0a3e58d271
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 81b29b660070f28d001e30ba00000001f0f107e4d1a9b3f508c72e6d95af3b04
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => ec1b2074a9d3f5c81b6e40d27a95cf38
    [APPLICATION_SCOPE] => placement
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => REST_APP_URI
    [PLACEMENT_OPTIONS] => {"test":"y","docId":"42","URI":"\/company\/personal\/user\/1\/blog\/"}
)
```

The example was captured for the link `/marketplace/view/#APP_CODE#/?params[test]=y&params[docId]=42` followed from the news feed.

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string. It contains the keys from the `params` of your link and the universal `URI` key.

#|
|| **Parameter** | **Description** ||
|| **Keys from `params`**
[`string`](../../data-types.md) | The values the application set in the link. Key names are arbitrary, and the values arrive as strings ||
|| **URI***
[`string`](../../data-types.md) | The path with the query string of the Bitrix24 page the user followed the link from ||
|#

Bitrix24 adds the `URI` key itself, but it never overwrites a value of its own. If the application passes its own `URI` key in `params`, the handler receives exactly that value.

## OPTIONS when registering via placement.bind

The placement does not support the `OPTIONS` parameter of the [placement.bind](../placement-bind.md) method: the values passed are not retained, and [placement.get](../placement-get.md) returns an empty array. Pass your settings in the handler address or in the `params` key of the link.

The placement does not support the `USER_ID` parameter either: an attempt to register a handler for a single user returns the error `ERROR_PLACEMENT_USER_MODE`. The handler is always registered for all Bitrix24 users.

An application registers only one handler for this placement. A repeated `placement.bind` call returns the error `ERROR_PLACEMENT_MAX_COUNT`. To change the handler address, first remove the registration with the [placement.unbind](../placement-unbind.md) method.

### Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PLACEMENT": "REST_APP_URI",
        "HANDLER": "https://your-domain.com/widgets/app-uri-handler.php",
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
          PLACEMENT: 'REST_APP_URI',
          HANDLER: 'https://your-domain.com/widgets/app-uri-handler.php',
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
      async function bindAppUri() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.bind',
            params: {
              PLACEMENT: 'REST_APP_URI',
              HANDLER: 'https://your-domain.com/widgets/app-uri-handler.php',
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

      document.addEventListener('DOMContentLoaded', bindAppUri)
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
                    'PLACEMENT' => 'REST_APP_URI',
                    'HANDLER' => 'https://your-domain.com/widgets/app-uri-handler.php',
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
            PLACEMENT: 'REST_APP_URI',
            HANDLER: 'https://your-domain.com/widgets/app-uri-handler.php'
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
            'PLACEMENT' => 'REST_APP_URI',
            'HANDLER' => 'https://your-domain.com/widgets/app-uri-handler.php',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "placement.bind", b24.Params{
    	"PLACEMENT": "REST_APP_URI",
    	"HANDLER":   "https://your-domain.com/widgets/app-uri-handler.php",
    })
    if err != nil {
    	return fmt.Errorf("placement.bind: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it into the response
    // shape of the placement.bind method, see "Response Handling" on its page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Relationship With Other Objects

**The content that holds the link.** The application places the link with the same methods it uses for any other text: in a chat message with the methods of the [{#T}](../../chats/index.md) section, in a feed post with the methods of the [{#T}](../../log/index.md) section, in a task description with the methods of the [{#T}](../../tasks/index.md) section.

**Application interface.** The slider is controlled by the [JavaScript methods for widgets](../bx24-widget-methods.md): `closeApplication` closes the widget, `openApplication` opens it again with different parameters.

## Common Mistakes

#|
|| **Mistake** | **Solution** ||
|| The link opens an empty slider | Check that the application is installed and active: the handler is substituted only for an installed application ||
|| The slider opens, but the parameters do not arrive | Parameters are passed only in the `params` key: `?params[docId]=42`. Keys passed directly in the query string do not reach `PLACEMENT_OPTIONS` ||
|| `placement.bind` returns `ERROR_PLACEMENT_MAX_COUNT` | The handler is already registered. Remove the old registration with the [placement.unbind](../placement-unbind.md) method ||
|| The link contains the handler registration identifier | The address needs the application code: the symbolic code of a mass-market application or the `client_id` of a local one ||
|#

{% note tip "Typical Use-Cases and Scenarios" %}

- [View external documents via link](https://helpdesk.bitrix24.com/courses/index.php?COURSE_ID=268&LESSON_ID=26030)

{% endnote %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./background-worker.md)
- [{#T}](../placement-bind.md)
- [{#T}](../placement-unbind.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../../../settings/interactivity/index.md)
