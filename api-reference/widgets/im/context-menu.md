# IM_CONTEXT_MENU Message Context Menu Item

> Scope: [`im`](../../scopes/permissions.md)

You can add your item to the context menu of messages in the chat.

The widget code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#| 
|| **Widget Code** | **Location** ||
|| `IM_CONTEXT_MENU` | Message context menu item ||
|#

### Where to Find It in the Interface

Open any chat and hover over a message. In the message action bar, click the `...` button to open the context menu. Hover over *More* to reveal additional menu items. The application item with `PLACEMENT=IM_CONTEXT_MENU` will appear at the end of the action list above the message.

## What the Handler Receives

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
    [PLACEMENT] => IM_CONTEXT_MENU
    [PLACEMENT_OPTIONS] => {"messageId":84889, "dialogId":"chat1489"}
)
```

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string containing the context of the call.

For `IM_CONTEXT_MENU`, the context includes the following keys:

- `dialogId` — the identifier of the current chat
- `messageId` — the identifier of the selected message

## OPTIONS When Registering via placement.bind

For `IM_CONTEXT_MENU`, the `placement.bind` method supports `OPTIONS` parameters.

#| 
|| **Parameter**
`type` | **Description** ||
|| **extranet**
[`string`](../../data-types.md) | Access in the extranet, default is `N`.

Possible values:
- `N` — the application is not available to extranet users
- `Y` — the application is available to extranet users
||
|| **context**
[`string`](../../data-types.md) | Display context, default is `ALL`. Multiple values can be passed using `;`.

Possible values:
- `ALL` — all chats
- `USER` — personal chats of users, excluding chats with bots
- `CHAT` — group chats, excluding `LINES` and `CRM`
- `LINES` — open lines chats
- `CRM` — chats created within CRM

If `ALL` is passed along with other values, only `ALL` is used. An invalid value will cause a registration error.
||
|| **role**
[`string`](../../data-types.md) | User role, default is `USER`.

Possible values:
- `USER` — the application is available to all users
- `ADMIN` — the application is available only to portal administrators
||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PLACEMENT": "IM_CONTEXT_MENU",
        "HANDLER": "https://your-domain.com/widgets/im-context-menu-handler.php",
        "TITLE": "My menu item",
        "LANG_ALL": {
          "de": {
            "TITLE": "Mein Menüpunkt"
          },
          "en": {
            "TITLE": "My menu item"
          }
        },
        "OPTIONS": {
          "context": "ALL",
          "role": "USER",
          "extranet": "N"
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
                PLACEMENT: 'IM_CONTEXT_MENU',
                HANDLER: 'https://your-domain.com/widgets/im-context-menu-handler.php',
                TITLE: 'My menu item',
                LANG_ALL: {
                    de: {
                        TITLE: 'Mein Menüpunkt',
                    },
                    en: {
                        TITLE: 'My menu item',
                    }
                },
                OPTIONS: {
                    context: 'ALL',
                    role: 'USER',
                    extranet: 'N',
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
                    'PLACEMENT' => 'IM_CONTEXT_MENU',
                    'HANDLER' => 'https://your-domain.com/widgets/im-context-menu-handler.php',
                    'TITLE' => 'My menu item',
                    'LANG_ALL' => [
                        'de' => [
                            'TITLE' => 'Mein Menüpunkt',
                        ],
                        'en' => [
                            'TITLE' => 'My menu item',
                        ],
                    ],
                    'OPTIONS' => [
                        'context' => 'ALL',
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
            PLACEMENT: 'IM_CONTEXT_MENU',
            HANDLER: 'https://your-domain.com/widgets/im-context-menu-handler.php',
            TITLE: 'My menu item',
            LANG_ALL: {
                de: { TITLE: 'Mein Menüpunkt' },
                en: { TITLE: 'My menu item' }
            },
            OPTIONS: {
                context: 'ALL',
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
            'PLACEMENT' => 'IM_CONTEXT_MENU',
            'HANDLER' => 'https://your-domain.com/widgets/im-context-menu-handler.php',
            'TITLE' => 'My menu item',
            'LANG_ALL' => [
                'de' => [
                    'TITLE' => 'Mein Menüpunkt',
                ],
                'en' => [
                    'TITLE' => 'My menu item',
                ],
            ],
            'OPTIONS' => [
                'context' => 'ALL',
                'role' => 'USER',
                'extranet' => 'N',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)