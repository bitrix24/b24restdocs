# Widget in a Mobile App Chat IMMOBILE_CONTEXT_MENU

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement, im`](../scopes/permissions.md)

The widget adds its own item to the *Apps* menu above the input field in a chat of the Bitrix24 mobile application. The handler receives the identifier of the chat it was opened from, so the placement suits applications that work with the current conversation: reply templates, conversation hints, or a client card from an external system.

The placement is declared in the global `placement` scope — it has no mobile scope of its own. The `im` scope is required for the handler to work with the chat by the received `dialogId`.

The placement code is passed in the `PLACEMENT` parameter of the [placement.bind](./placement-bind.md) method.

This is the only placement of the mobile application. All other widgets are displayed in the Bitrix24 web interface only.

{% note info "" %}

The widget is not displayed in the interface until the application installation is complete. [Check the application installation](../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#|
|| **Placement Code** | **Location** ||
|| `IMMOBILE_CONTEXT_MENU` | Item of the *Apps* menu above the input field in a chat of the mobile application ||
|#

### Where to Find It in the Interface

Open a chat in the Bitrix24 mobile application and look at the row of buttons above the input field. Tap the *Apps* button — a menu opens with all applications that registered this placement. The item is named after the `TITLE` parameter, and if it is empty, after the application name.

The button is displayed in a regular chat, in a CoPilot chat, and in an AI assistant chat. The widget opens as a separate page on top of the conversation.

If no handler is registered, the menu does not open — instead, the mobile application shows a notification that there are no applications.

The button is not present in every Bitrix24: its display is enabled on the Bitrix24 side and depends neither on the application settings nor on the registration parameters. While the button is missing, a registered widget is not displayed, although the registration itself works.

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

```php

Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 52df2309b7ca57fa93c8545c3650fa57
    [AUTH_ID] => 83fd7166007e9c94001e30ba00000001f0f107a52e6ad7ef9a1b8c3d5e2f4061
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 737c9966007e9c94001e30ba00000001f0f1072b8d4e6f01a3c57d9e8b2f4160
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => ec1b2074a9d3f5c81b6e40d27a95cf38
    [APPLICATION_SCOPE] => im,placement
    [member_id] => d897063e1ce7c5eb9f04b9751eef5915
    [status] => L
    [PLACEMENT] => IMMOBILE_CONTEXT_MENU
    [PLACEMENT_OPTIONS] => {"dialogId":"chat1"}
)

```

{% include [Note on Required Parameters](../../_includes/required.md) %}

{% include notitle [Description of Standard Data](_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The `PLACEMENT_OPTIONS` value is passed as a JSON string with the call context.

#|
|| **Key**
`type` | **Description** ||
|| **dialogId***
[`string`](../data-types.md) | Identifier of the chat the widget is opened from: `chatNNN` for a group chat, the user identifier for a private conversation. The chat can be retrieved by it with the [im.dialog.get](../chats/im-dialog-get.md) method ||
|#

The universal `URI` key does not arrive to this placement. Bitrix24 substitutes it from the address of the page the widget is opened on, while in the mobile application the widget opens as a separate page rather than as a frame inside the interface.

## OPTIONS When Registering via placement.bind

The `OPTIONS` parameters are passed by the developer when registering the handler — these are not the data that Bitrix24 sends to the handler when it calls the placement.

For `IMMOBILE_CONTEXT_MENU`, the `placement.bind` method supports three parameters, all of them optional.

#|
|| **Parameter**
`type` | **Description** ||
|| **context**
[`string`](../data-types.md) | Chat types the item is displayed in, default is `ALL`. Several values are listed with `;`, for example `CHAT;LINES`.

Possible values:
- `ALL` — all chats
- `USER` — private conversation
- `CHAT` — group chat
- `LINES` — open channel chat
- `CRM` — chat linked to a CRM item

Any other value results in the `INVALID_ERROR_CONTEXT` error
||
|| **extranet**
[`string`](../data-types.md) | Access in the extranet, default is `N`.

Possible values:
- `N` — application is not available for extranet users
- `Y` — application is available for extranet users

Any other value results in the `INVALID_ERROR_EXTRANET` error
||
|| **role**
[`string`](../data-types.md) | User role, default is `USER`.

Possible values:
- `USER` — application is available to all users
- `ADMIN` — application is available only to portal administrators

Any other value results in the `INVALID_ERROR_ROLE` error
||
|#

The restrictions set by these parameters are applied by the web version of the messenger. The mobile application requests the list of widgets without filtering by `context`, `role`, and `extranet`, so check the rights and the chat type in the handler itself instead of relying on the registration parameters.

The placement supports no other parameters. The icon name cannot be set: it is the same for all menu items, and a passed `iconName` value is not retained by Bitrix24. A personal handler binding is not supported either — with the `USER_ID` parameter the method returns the `ERROR_PLACEMENT_USER_MODE` error.

### Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PLACEMENT": "IMMOBILE_CONTEXT_MENU",
        "HANDLER": "https://your-domain.com/widgets/immobile-context-menu-handler.php",
        "TITLE": "Reply templates",
        "LANG_ALL": {
          "en": {
            "TITLE": "Reply templates"
          },
          "de": {
            "TITLE": "Antwortvorlagen"
          }
        },
        "OPTIONS": {
          "context": "CHAT;LINES",
          "role": "USER",
          "extranet": "N"
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
          PLACEMENT: 'IMMOBILE_CONTEXT_MENU',
          HANDLER: 'https://your-domain.com/widgets/immobile-context-menu-handler.php',
          TITLE: 'Reply templates',
          LANG_ALL: {
            en: {
              TITLE: 'Reply templates',
            },
            de: {
              TITLE: 'Antwortvorlagen',
            },
          },
          OPTIONS: {
            context: 'CHAT;LINES',
            role: 'USER',
            extranet: 'N',
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
      async function bindImmobileContextMenu() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.bind',
            params: {
              PLACEMENT: 'IMMOBILE_CONTEXT_MENU',
              HANDLER: 'https://your-domain.com/widgets/immobile-context-menu-handler.php',
              TITLE: 'Reply templates',
              LANG_ALL: {
                en: {
                  TITLE: 'Reply templates',
                },
                de: {
                  TITLE: 'Antwortvorlagen',
                },
              },
              OPTIONS: {
                context: 'CHAT;LINES',
                role: 'USER',
                extranet: 'N',
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

      document.addEventListener('DOMContentLoaded', bindImmobileContextMenu)
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
                    'PLACEMENT' => 'IMMOBILE_CONTEXT_MENU',
                    'HANDLER' => 'https://your-domain.com/widgets/immobile-context-menu-handler.php',
                    'TITLE' => 'Reply templates',
                    'LANG_ALL' => [
                        'en' => [
                            'TITLE' => 'Reply templates',
                        ],
                        'de' => [
                            'TITLE' => 'Antwortvorlagen',
                        ],
                    ],
                    'OPTIONS' => [
                        'context' => 'CHAT;LINES',
                        'role' => 'USER',
                        'extranet' => 'N',
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
            PLACEMENT: 'IMMOBILE_CONTEXT_MENU',
            HANDLER: 'https://your-domain.com/widgets/immobile-context-menu-handler.php',
            TITLE: 'Reply templates',
            LANG_ALL: {
                en: { TITLE: 'Reply templates' },
                de: { TITLE: 'Antwortvorlagen' }
            },
            OPTIONS: {
                context: 'CHAT;LINES',
                role: 'USER',
                extranet: 'N'
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
            'PLACEMENT' => 'IMMOBILE_CONTEXT_MENU',
            'HANDLER' => 'https://your-domain.com/widgets/immobile-context-menu-handler.php',
            'TITLE' => 'Reply templates',
            'LANG_ALL' => [
                'en' => [
                    'TITLE' => 'Reply templates',
                ],
                'de' => [
                    'TITLE' => 'Antwortvorlagen',
                ],
            ],
            'OPTIONS' => [
                'context' => 'CHAT;LINES',
                'role' => 'USER',
                'extranet' => 'N',
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
    	"PLACEMENT": "IMMOBILE_CONTEXT_MENU",
    	"HANDLER":   "https://your-domain.com/widgets/immobile-context-menu-handler.php",
    	"TITLE":     "Reply templates",
    	"LANG_ALL": b24.Params{
    		"ru": b24.Params{
    			"TITLE": "Reply templates",
    		},
    		"en": b24.Params{
    			"TITLE": "Reply templates",
    		},
    	},
    	"OPTIONS": b24.Params{
    		"context":  "CHAT;LINES",
    		"role":     "USER",
    		"extranet": "N",
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

## Relationship with Other Objects

**Chat.** The `dialogId` key indicates which chat the widget is opened from. Chat data is returned by the [im.dialog.get](../chats/im-dialog-get.md) method, the message history by [im.dialog.messages.get](../chats/messages/im-dialog-messages-get.md), and a message can be sent to the same chat with the [im.message.add](../chats/messages/im-message-add.md) method.

**User.** For a private conversation, `dialogId` equals the identifier of the interlocutor, so their data is returned by the [user.get](../user/user-get.md) method.

## Common Mistakes

#|
|| **Mistake** | **Solution** ||
|| The *Apps* button is missing in the chat | Update the mobile application: older versions have no row of buttons above the input field at all. If the button is missing in the current version as well, its display is not yet enabled for this Bitrix24 — in this case the widget cannot be displayed ||
|| The menu opens, but the application item is not there | Complete the [application installation](../../settings/app-installation/installation-finish.md). Until the installation is complete, the widget is not displayed ||
|| The item is looked for in the web version of the messenger | The placement works in the mobile application only. The web interface has its own placements: [IM_CONTEXT_MENU](./im/context-menu.md), [IM_SIDEBAR](./im/sidebar.md), [IM_TEXTAREA](./im/textarea.md) ||
|| The handler expects the `URI` key | This placement does not receive `URI`. The only context key is `dialogId` ||
|| Access is limited with the `role` and `context` parameters | The mobile application does not apply them. Check the rights and the chat type in the handler ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./im/index.md)
- [{#T}](./placement-bind.md)
- [{#T}](./placement-unbind.md)
- [{#T}](./bx24-widget-methods.md)
- [{#T}](../chats/im-dialog-get.md)
- [{#T}](../../settings/app-installation/installation-finish.md)
