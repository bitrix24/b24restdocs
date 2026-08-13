# Tile in the Contact Center CONTACT_CENTER

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement, contact_center`](../scopes/permissions.md)

The widget adds an application tile to the Contact Center — the same place where the user connects mail, telephony, and messengers. Clicking the tile opens the application interface: usually a form for connecting a communication channel of your own.

The placement code is specified in the `PLACEMENT` parameter of the [placement.bind](./placement-bind.md) method.

{% note info "" %}

The widget is not displayed in the interface until the application installation is complete. [Check the application installation](../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#|
|| **Placement Code** | **Location** ||
|| `CONTACT_CENTER` | Tile in the Contact Center ||
|#

### Where to Find It in the Interface

Open the Contact Center at `/contact_center/`. The application tile is displayed at the bottom of the page, in the *Partner solutions* block.

![Tile in the Contact Center](./_images/CONTACT_CENTER.png "Tile in the Contact Center")

The tile name is set by the `TITLE` parameter during registration. If the parameter is not passed, the application name is displayed. For an active application, the tile is marked with a check, like the connected built-in channels.

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

```php
Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 0123456789abcdef0123456789abcdef
    [AUTH_ID] => 6061e72600631fcd00005a4b00000001f0f1076700000000f69dd5fc643d9ce2fdbc1
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 50e00aa340631fcd00005a4b00000001f0f1071111116580a5b83c2de639ef28c12
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => 5b2f8c1d7e3a9046b8c5d2f1a7e3b904
    [APPLICATION_SCOPE] => contact_center,imopenlines,placement
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => CONTACT_CENTER
    [PLACEMENT_OPTIONS] => {"ID":"717","URI":"\/contact_center\/"}
)
```

{% include [Note on required parameters](../../_includes/required.md) %}

{% include notitle [Description of Standard Data](_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

#|
|| **Key** | **Description** ||
|| **ID**
[`string`](../data-types.md) | The identifier of the handler registration — the same value that comes in the `id` field of the [placement.get](./placement-get.md) method response.

An application can register several tiles with different handlers. By this key, the handler determines which of them the user opened ||
|| **URI**
[`string`](../data-types.md) | The address of the Contact Center page the widget is opened from ||
|#

This placement does not support the `OPTIONS` parameter of the [placement.bind](./placement-bind.md) method: the values passed are not retained, and [placement.get](./placement-get.md) returns an empty array.

## Relationship with Other Objects

**Open channels.** The tile in the Contact Center is the entry point for connecting a channel. The channel itself is registered by the application with the methods of the [{#T}](../imopenlines/imconnector/index.md) section, not through this placement.

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PLACEMENT": "CONTACT_CENTER",
        "HANDLER": "https://your-domain.com/widgets/contact-center-handler.php",
        "TITLE": "My communication channel",
        "LANG_ALL": {
          "en": {
            "TITLE": "My communication channel"
          },
          "de": {
            "TITLE": "Mein Kommunikationskanal"
          }
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
          PLACEMENT: 'CONTACT_CENTER',
          HANDLER: 'https://your-domain.com/widgets/contact-center-handler.php',
          TITLE: 'My communication channel',
          LANG_ALL: {
            en: {
              TITLE: 'My communication channel',
            },
            de: {
              TITLE: 'Mein Kommunikationskanal',
            },
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
      async function bindContactCenter() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.bind',
            params: {
              PLACEMENT: 'CONTACT_CENTER',
              HANDLER: 'https://your-domain.com/widgets/contact-center-handler.php',
              TITLE: 'My communication channel',
              LANG_ALL: {
                en: {
                  TITLE: 'My communication channel',
                },
                de: {
                  TITLE: 'Mein Kommunikationskanal',
                },
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

      document.addEventListener('DOMContentLoaded', bindContactCenter)
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
                    'PLACEMENT' => 'CONTACT_CENTER',
                    'HANDLER' => 'https://your-domain.com/widgets/contact-center-handler.php',
                    'TITLE' => 'My communication channel',
                    'LANG_ALL' => [
                        'en' => [
                            'TITLE' => 'My communication channel',
                        ],
                        'de' => [
                            'TITLE' => 'Mein Kommunikationskanal',
                        ],
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
            PLACEMENT: 'CONTACT_CENTER',
            HANDLER: 'https://your-domain.com/widgets/contact-center-handler.php',
            TITLE: 'My communication channel',
            LANG_ALL: {
                en: {
                    TITLE: 'My communication channel'
                },
                de: {
                    TITLE: 'Mein Kommunikationskanal'
                }
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
            'PLACEMENT' => 'CONTACT_CENTER',
            'HANDLER' => 'https://your-domain.com/widgets/contact-center-handler.php',
            'TITLE' => 'My communication channel',
            'LANG_ALL' => [
                'en' => [
                    'TITLE' => 'My communication channel',
                ],
                'de' => [
                    'TITLE' => 'Mein Kommunikationskanal',
                ],
            ],
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
    	"PLACEMENT": "CONTACT_CENTER",
    	"HANDLER":   "https://your-domain.com/widgets/contact-center-handler.php",
    	"TITLE":     "My communication channel",
    	"LANG_ALL": b24.Params{
    		"ru": b24.Params{
    			"TITLE": "My communication channel",
    		},
    		"en": b24.Params{
    			"TITLE": "My communication channel",
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("placement.bind: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it into the response
    // shape of the placement.bind method, see "Response Handling" on its page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Continue Learning

- [{#T}](./placement-bind.md)
- [{#T}](./placement-list.md)
- [{#T}](./placement-unbind.md)
- [{#T}](./ui-interaction/index.md)
- [{#T}](./bx24-widget-methods.md)
- [{#T}](../imopenlines/imconnector/index.md)
- [{#T}](../../settings/interactivity/index.md)
