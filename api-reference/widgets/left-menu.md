# Item in the Main Menu LEFT_MENU

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../scopes/permissions.md)

The widget adds an item to the main menu of Bitrix24. The user selects the item, and the application interface opens as a separate page — across the entire working area, not in a slider. The placement suits applications with a section of their own: a dashboard, a report, a catalog.

The placement code is specified in the `PLACEMENT` parameter of the [placement.bind](./placement-bind.md) method.

{% note info "" %}

The widget is not displayed in the interface until the application installation is complete. [Check the application installation](../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#|
|| **Widget Code** | **Location** ||
|| `LEFT_MENU` | Item in the main menu of Bitrix24 ||
|#

### Where to Find It in the Interface

The application item goes to the *Applications* group of the main menu, together with the items of other installed applications. The group is collapsed by default — expand it to see the item.

![Item in the main menu of Bitrix24](./_images/LEFT_MENU.png "Item in the main menu of Bitrix24")

The item name is set by the `TITLE` parameter during registration. If the parameter is not passed, the application name is displayed.

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

```php
Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => fea0d7bc24669fcb8807e88ee394c7ca
    [AUTH_ID] => 63d39f6600631fcd00005a4b00000001f0f1071905299b72b307a6c223d43877697546
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 5352c76600631fcd00005a4b00000001f0f107d262f083bb53a16948269371e327d1d9
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => 5b2f8c1d7e3a9046b8c5d2f1a7e3b904
    [APPLICATION_SCOPE] => placement
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => LEFT_MENU
    [PLACEMENT_OPTIONS] => {"URI":"\/crm\/lead\/list\/"}
)
```

{% include [Note on Required Parameters](../../_includes/required.md) %}

{% include notitle [Description of Standard Data](_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

This placement has no own keys. The call context carries only the universal `URI` key — the address of the page the user came from when selecting the menu item. The menu is shown on every page of Bitrix24, so the value of `URI` differs every time and does not depend on the application itself.

This placement does not support the `OPTIONS` parameter of the [placement.bind](./placement-bind.md) method: the values passed are not retained, and [placement.get](./placement-get.md) returns an empty array.

{% note tip "Typical use-cases and scenarios" %}

- [Application with its own page in the left menu](https://helpdesk.bitrix24.com/courses/index.php?COURSE_ID=268&LESSON_ID=26022)

{% endnote %}

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PLACEMENT": "LEFT_MENU",
        "HANDLER": "https://your-domain.com/widgets/left-menu-handler.php",
        "TITLE": "Supplier dashboard",
        "LANG_ALL": {
          "en": {
            "TITLE": "Supplier dashboard"
          },
          "de": {
            "TITLE": "Lieferanten-Dashboard"
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
          PLACEMENT: 'LEFT_MENU',
          HANDLER: 'https://your-domain.com/widgets/left-menu-handler.php',
          TITLE: 'Supplier dashboard',
          LANG_ALL: {
            en: {
              TITLE: 'Supplier dashboard',
            },
            de: {
              TITLE: 'Lieferanten-Dashboard',
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
      async function bindLeftMenu() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.bind',
            params: {
              PLACEMENT: 'LEFT_MENU',
              HANDLER: 'https://your-domain.com/widgets/left-menu-handler.php',
              TITLE: 'Supplier dashboard',
              LANG_ALL: {
                en: {
                  TITLE: 'Supplier dashboard',
                },
                de: {
                  TITLE: 'Lieferanten-Dashboard',
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

      document.addEventListener('DOMContentLoaded', bindLeftMenu)
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
                    'PLACEMENT' => 'LEFT_MENU',
                    'HANDLER' => 'https://your-domain.com/widgets/left-menu-handler.php',
                    'TITLE' => 'Supplier dashboard',
                    'LANG_ALL' => [
                        'en' => [
                            'TITLE' => 'Supplier dashboard',
                        ],
                        'de' => [
                            'TITLE' => 'Lieferanten-Dashboard',
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
            PLACEMENT: 'LEFT_MENU',
            HANDLER: 'https://your-domain.com/widgets/left-menu-handler.php',
            TITLE: 'Supplier dashboard',
            LANG_ALL: {
                en: {
                    TITLE: 'Supplier dashboard'
                },
                de: {
                    TITLE: 'Lieferanten-Dashboard'
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
            'PLACEMENT' => 'LEFT_MENU',
            'HANDLER' => 'https://your-domain.com/widgets/left-menu-handler.php',
            'TITLE' => 'Supplier dashboard',
            'LANG_ALL' => [
                'en' => [
                    'TITLE' => 'Supplier dashboard',
                ],
                'de' => [
                    'TITLE' => 'Lieferanten-Dashboard',
                ],
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](./placement-bind.md)
- [{#T}](./placement-list.md)
- [{#T}](./placement-unbind.md)
- [{#T}](./universal/index.md)
- [{#T}](./ui-interaction/index.md)
- [{#T}](./bx24-widget-methods.md)
- [{#T}](../../settings/interactivity/index.md)
