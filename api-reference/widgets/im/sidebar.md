# Widget for the IM_SIDEBAR

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)

The widget adds its item to the chat sidebar.

The embedding code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The embedding will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the widget is embedded

#| 
|| **Embedding Code** | **Location** ||
|| `IM_SIDEBAR` | Item in the chat sidebar ||
|#

### Where it is located in the interface

Open the chat and click the sidebar button on the right side of the top chat panel. In the opened sidebar, there is an *Applications* block at the bottom, which displays the application item with `PLACEMENT=IM_SIDEBAR`.

## What the handler receives

Data is transmitted as a POST request {.b24-info}

```php
Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => de
    [APP_SID] => 99c80eff6378726287350416ee5fef0
    [AUTH_ID] => 6061e72600631fcd00005a4b00000001f0f1076700000000f69dd5fc643d9ce2fdbc1
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 50e00aa340631fcd00005a4b00000001f0f1071111116580a5b83c2de639ef28c12
    [member_id] => da45a03b265ed12127f8a258d793cc5d
    [status] => F
    [PLACEMENT] => IM_SIDEBAR
    [PLACEMENT_OPTIONS] => {"dialogId":"chat1489"}
)
```

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [description of standard data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the context of the call.

For `IM_SIDEBAR`, the following key is passed in the context:

- `dialogId` — identifier of the current chat

## OPTIONS when registering via placement.bind

For `IM_SIDEBAR`, the `placement.bind` method supports `OPTIONS` parameters.

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Parameter** `type` | **Description** ||
|| **iconName*** [`string`](../../data-types.md) | Label of the item in the interface. Up to 50 characters, Latin letters, space, and `-` are allowed ||
|| **extranet** [`string`](../../data-types.md) | Access in the extranet, default is `N`.

Possible values:
- `N` — application is not available for extranet users
- `Y` — application is available for extranet users
||
|| **context** [`string`](../../data-types.md) | Display context, default is `ALL`. Multiple values can be passed using `;`.

Possible values:
- `ALL` — all chats
- `USER` — personal chats of users, excluding chats with bots
- `CHAT` — group chats, excluding `LINES` and `CRM`
- `LINES` — chats of open channels
- `CRM` — chats created within CRM

If `ALL` is passed along with other values, only `ALL` is used. An invalid value will cause a registration error
||
|| **role** [`string`](../../data-types.md) | User role, default is `USER`.

Possible values:
- `USER` — application is available to all users
- `ADMIN` — application is available only to portal administrators
||
|| **color** [`string`](../../data-types.md) | Icon color from the IM palette.

Possible values:
- `RED` — red
- `GREEN` — green
- `MINT` — mint
- `LIGHT_BLUE` — light blue
- `DARK_BLUE` — dark blue
- `PURPLE` — purple
- `AQUA` — aqua
- `PINK` — pink
- `LIME` — lime
- `BROWN` — brown
- `AZURE` — azure
- `KHAKI` — khaki
- `SAND` — sand
- `ORANGE` — orange
- `MARENGO` — marengo
- `GRAY` — gray
- `GRAPHITE` — graphite
||
|#

### Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PLACEMENT": "IM_SIDEBAR",
        "HANDLER": "https://your-domain.com/widgets/im-sidebar-handler.php",
        "TITLE": "My sidebar item",
        "LANG_ALL": {
          "de": {
            "TITLE": "Mein Sidebar-Element"
          },
          "en": {
            "TITLE": "My sidebar item"
          }
        },
        "OPTIONS": {
          "iconName": "chat-tools",
          "context": "ALL",
          "role": "USER",
          "extranet": "N",
          "color": "LIGHT_BLUE"
        },
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/placement.bind
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'placement.bind',
            {
                PLACEMENT: 'IM_SIDEBAR',
                HANDLER: 'https://your-domain.com/widgets/im-sidebar-handler.php',
                TITLE: 'My sidebar item',
                LANG_ALL: {
                    de: {
                        TITLE: 'Mein Sidebar-Element',
                    },
                    en: {
                        TITLE: 'My sidebar item',
                    }
                },
                OPTIONS: {
                    iconName: 'chat-tools',
                    context: 'ALL',
                    role: 'USER',
                    extranet: 'N',
                    color: 'LIGHT_BLUE',
                }
            }
        );

        const result = response.getData().result;
        if (result.error())
            console.error(result.error());
        else
            console.info(result.data());
    }
    catch (error)
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'placement.bind',
                [
                    'PLACEMENT' => 'IM_SIDEBAR',
                    'HANDLER' => 'https://your-domain.com/widgets/im-sidebar-handler.php',
                    'TITLE' => 'My sidebar item',
                    'LANG_ALL' => [
                        'de' => [
                            'TITLE' => 'Mein Sidebar-Element',
                        ],
                        'en' => [
                            'TITLE' => 'My sidebar item',
                        ],
                    ],
                    'OPTIONS' => [
                        'iconName' => 'chat-tools',
                        'context' => 'ALL',
                        'role' => 'USER',
                        'extranet' => 'N',
                        'color' => 'LIGHT_BLUE',
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
            PLACEMENT: 'IM_SIDEBAR',
            HANDLER: 'https://your-domain.com/widgets/im-sidebar-handler.php',
            TITLE: 'My sidebar item',
            LANG_ALL: {
                de: { TITLE: 'Mein Sidebar-Element' },
                en: { TITLE: 'My sidebar item' }
            },
            OPTIONS: {
                iconName: 'chat-tools',
                context: 'ALL',
                role: 'USER',
                extranet: 'N',
                color: 'LIGHT_BLUE'
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
            'PLACEMENT' => 'IM_SIDEBAR',
            'HANDLER' => 'https://your-domain.com/widgets/im-sidebar-handler.php',
            'TITLE' => 'My sidebar item',
            'LANG_ALL' => [
                'de' => [
                    'TITLE' => 'Mein Sidebar-Element',
                ],
                'en' => [
                    'TITLE' => 'My sidebar item',
                ],
            ],
            'OPTIONS' => [
                'iconName' => 'chat-tools',
                'context' => 'ALL',
                'role' => 'USER',
                'extranet' => 'N',
                'color' => 'LIGHT_BLUE',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../../../settings/interactivity/index.md)