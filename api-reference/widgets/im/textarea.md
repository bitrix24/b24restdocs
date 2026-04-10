# Widget Above the IM_TEXTAREA Input Field

> Scope: [`im`](../../scopes/permissions.md)

The widget adds its item to the panel above the message input field.

The embedding code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The embedding will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#| 
|| **Widget Code** | **Location** ||
|| `IM_TEXTAREA` | Item in the panel above the input field ||
|#

### Where to Find It in the Interface

Open the chat and scroll to the bottom of the window where the message input field is located. In the bottom right corner of the input field, open the application panel. The application item with `PLACEMENT=IM_TEXTAREA` can be found in this panel.

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
    [PLACEMENT] => IM_TEXTAREA
    [PLACEMENT_OPTIONS] => {"dialogId":"chat1489"}
)
```

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the context of the call.

For `IM_TEXTAREA`, the following key is passed in the context:

- `dialogId` — the identifier of the current chat

## OPTIONS When Registering via placement.bind

For `IM_TEXTAREA`, the `placement.bind` method supports `OPTIONS` parameters.

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#| 
|| **Parameter** `type` | **Description** ||
|| **iconName*** [`string`](../../data-types.md) | Label of the item in the interface. Up to 50 characters, Latin letters, spaces, and `-` are allowed ||
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
- `LINES` — open lines chats
- `CRM` — chats created within CRM

If `ALL` is passed along with other values, only `ALL` is used. An incorrect value will cause a registration error
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
|| **width** [`integer`](../../data-types.md) | Width of the block in percentage, default is `100`, value must be greater than or equal to `0` ||
|| **height** [`integer`](../../data-types.md) | Height of the block in percentage, default is `100`, value must be greater than or equal to `0` ||
|#

### Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PLACEMENT": "IM_TEXTAREA",
        "HANDLER": "https://your-domain.com/widgets/im-textarea-handler.php",
        "TITLE": "My toolbar item",
        "LANG_ALL": {
          "de": {
            "TITLE": "Mein Toolbar-Element"
          },
          "en": {
            "TITLE": "My toolbar item"
          }
        },
        "OPTIONS": {
          "iconName": "chat-compose",
          "context": "ALL",
          "role": "USER",
          "extranet": "N",
          "color": "LIGHT_BLUE",
          "width": 100,
          "height": 100
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
                PLACEMENT: 'IM_TEXTAREA',
                HANDLER: 'https://your-domain.com/widgets/im-textarea-handler.php',
                TITLE: 'My toolbar item',
                LANG_ALL: {
                    de: {
                        TITLE: 'Mein Toolbar-Element',
                    },
                    en: {
                        TITLE: 'My toolbar item',
                    }
                },
                OPTIONS: {
                    iconName: 'chat-compose',
                    context: 'ALL',
                    role: 'USER',
                    extranet: 'N',
                    color: 'LIGHT_BLUE',
                    width: 100,
                    height: 100,
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
                    'PLACEMENT' => 'IM_TEXTAREA',
                    'HANDLER' => 'https://your-domain.com/widgets/im-textarea-handler.php',
                    'TITLE' => 'My toolbar item',
                    'LANG_ALL' => [
                        'de' => [
                            'TITLE' => 'Mein Toolbar-Element',
                        ],
                        'en' => [
                            'TITLE' => 'My toolbar item',
                        ],
                    ],
                    'OPTIONS' => [
                        'iconName' => 'chat-compose',
                        'context' => 'ALL',
                        'role' => 'USER',
                        'extranet' => 'N',
                        'color' => 'LIGHT_BLUE',
                        'width' => 100,
                        'height' => 100,
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
            PLACEMENT: 'IM_TEXTAREA',
            HANDLER: 'https://your-domain.com/widgets/im-textarea-handler.php',
            TITLE: 'My toolbar item',
            LANG_ALL: {
                de: { TITLE: 'Mein Toolbar-Element' },
                en: { TITLE: 'My toolbar item' }
            },
            OPTIONS: {
                iconName: 'chat-compose',
                context: 'ALL',
                role: 'USER',
                extranet: 'N',
                color: 'LIGHT_BLUE',
                width: 100,
                height: 100
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
            'PLACEMENT' => 'IM_TEXTAREA',
            'HANDLER' => 'https://your-domain.com/widgets/im-textarea-handler.php',
            'TITLE' => 'My toolbar item',
            'LANG_ALL' => [
                'de' => [
                    'TITLE' => 'Mein Toolbar-Element',
                ],
                'en' => [
                    'TITLE' => 'My toolbar item',
                ],
            ],
            'OPTIONS' => [
                'iconName' => 'chat-compose',
                'context' => 'ALL',
                'role' => 'USER',
                'extranet' => 'N',
                'color' => 'LIGHT_BLUE',
                'width' => 100,
                'height' => 100,
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