# Update Application Data in Chat imbot.app.update

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.app.update` updates application data in the chat.

{% note warning %}

The required fields are the application ID and one of the necessary fields for editing. If both JS and IFRAME methods are specified in one command, only JS will be used.

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name** | **Example** | **Description** ||
|| **APP_ID*** | `13` | Chat identifier ||
|| **IFRAME** | `'https://marta.bitrix.com/iframe/echo.php'` | URL of the frame ||
|| **IFRAME_WIDTH** | `'350'` | Desired width of the frame. Minimum value - 250px ||
|| **IFRAME_HEIGHT** | `'150'` | Desired height of the frame. Minimum value - 50px ||
|| **JS_METHOD** | `'SEND'` | ||
|| **JS_PARAM** | `'/help'` | ||
|| **HASH** | `'register'` | Token for accessing your frame, 32 characters ||
|| **ICON_FILE** | `'/* base64 image */'` | Icon of your application - base64 ||
|| **CONTEXT** | `'BOT'` | Application context ||
|| **EXTRANET_SUPPORT** | `'N'` | Is the command available to extranet users, default N ||
|| **LIVECHAT_SUPPORT** | `'N'` | Online chat support ||
|| **IFRAME_POPUP** | `'N'` | iframe will open with the ability to move within the messenger, switching between dialogs will not close such a window ||
|| **LANG** | `Array(...)` | Array of translations, it is advisable to specify at least for DE and EN ||
|| **LANGUAGE_ID (LANG)** | `'en'` | Language identifier for translation ||
|| **TITLE (LANG)** | `'Echobot IFRAME'` | Title for the button or command in the specified language ||
|| **DESCRIPTION (LANG)** | `'Open Echobot IFRAME app'` | Command description in the specified language ||
|| **COPYRIGHT (LANG)** | `'Bitrix24'` | Copyright ||
|#

## Code Example

{% include [Explanation on restCommand](../../_includes/rest-command.md) %}

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.app.update',
        Array(
            'APP_ID' => 13,
            'FIELDS' => Array(
                'IFRAME' => 'https://marta.bitrix.com/iframe/echo.php',
                'IFRAME_WIDTH' => '350',
                'IFRAME_HEIGHT' => '150',
                'JS_METHOD' => 'SEND',
                'JS_PARAM' => '/help',
                'HASH' => 'register',
                'ICON_FILE' => '/* base64 image */',
                'CONTEXT' => 'BOT',
                'EXTRANET_SUPPORT' => 'N',
                'LIVECHAT_SUPPORT' => 'N',
                'IFRAME_POPUP' => 'N',
                'LANG' => Array(
                    Array(
                        'LANGUAGE_ID' => 'en',
                        'TITLE' => 'Echobot IFRAME',
                        'DESCRIPTION' => 'Open Echobot IFRAME app',
                        'COPYRIGHT' => 'Bitrix24'
                    ),
                )
            )
        ),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

## Successful Response

`true`

## Possible Error Codes

#|
|| **Code** | **Description** ||
|| `CHAT_APP_ID_ERROR` | Application not found ||
|| `APP_ID_ERROR` | The chat application does not belong to this rest application. You can only work with chat applications installed within the current rest application ||
|| `IFRAME_HTTPS` | The link to `IFRAME` must be on a site with an active HTTPS certificate ||
|| `WRONG_REQUEST` | Something went wrong ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imbot-app-register.md)
- [{#T}](./imbot-app-unregister.md)