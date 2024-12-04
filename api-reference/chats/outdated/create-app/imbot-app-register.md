# Register a Chat Application imbot.app.register

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.app.register` registers a chat application.

Applications are divided into two types: [JS command and IFRAME application](../chat-apps.md).

## Method Parameters for JS Command

#|
|| **Name** | **Example** | **Description** ||
|| **BOT_ID** | `62` | Identifier of the bot that owns the chat application ||
|| **CODE** | `'echo'` | Code of the chat application ||
|| **JS_METHOD** | `'SEND'` | Options for `JS_METHOD`:
- `PUT` — inserts a command to the chat bot in the textarea, `JS_PARAM` can only contain the command text and its parameters (allowed characters: a-z 0-9 - + _ / )
- `SEND` — sends a command to the chat bot, `JS_PARAM` can only contain the command text and its parameters (allowed characters a-z 0-9 - + _ / )
- `CALL` — calls a phone number
- `SUPPORT` — opens the support chat bot working through Open Channels, `JS_PARAM` must specify the open channel code (32 characters) ||
|| **JS_PARAM** | `'/help'` | ||
|| **ICON_FILE** | `'/* base64 image */'` | Icon in base64.
The format of the icon `ICON_FILE`:
- This field must contain the base64 of your icon, the icon format is detailed [in the documentation](../icon.md)
- Note that if this field is not specified, the application will be available in the system dialog "Chat Applications" ||
|| **CONTEXT** | `'BOT'` | Context of the application.
The `CONTEXT` indicates where the application will be available:
- `ALL` — in all chats
- `USER` — only in one-on-one chats
- `CHAT` — only in group chats
- `BOT` — only with the chat bot that installed the application
- `LINES` — only in Open Channels chats
- `CALL` — only in chats created within Telephony

Each context can have the postfix `-admin`, then the application will be available in the specified context only to Bitrix24 administrators ||
|| **EXTRANET_SUPPORT** | `'N'` | Is the command available to extranet users, default is N ||
|| **LIVECHAT_SUPPORT** | `'N'` | Online chat support ||
|| **IFRAME_POPUP** | `'N'` | The iframe will open with the ability to move within the messenger, switching between dialogs will not close such a window. ||
|| **LANG** | `Array(...)` | Array of translations, it is advisable to specify at least for DE and EN ||
|| **LANGUAGE_ID (LANG)** | `'en'` | Language identifier for translation ||
|| **TITLE (LANG)** | `'Echobot BUTTON'` | Title for the button or command in the specified language ||
|| **DESCRIPTION (LANG)** | `'Send help command'` | Description of the command in the specified language ||
|#

### Code Example

{% include [Explanation about restCommand](../../_includes/rest-command.md) %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.app.register',
        Array(
            'BOT_ID' => 62,
            'CODE' => 'echo',
            'JS_METHOD' => 'SEND',
            'JS_PARAM' => '/help',
            'ICON_FILE' => '/* base64 image */',
            'CONTEXT' => 'BOT',
            'EXTRANET_SUPPORT' => 'N',
            'LIVECHAT_SUPPORT' => 'N',
            'IFRAME_POPUP' => 'N',
            'LANG' => Array(
                Array(
                    'LANGUAGE_ID' => 'en',
                    'TITLE' => 'Echobot BUTTON',
                    'DESCRIPTION' => 'Send help command'
                ),
            )
        ),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

## Method Parameters for IFRAME Application

#|
|| **Name** | **Example** | **Description** ||
|| **BOT_ID** | `62` | Identifier of the bot that owns the chat application ||
|| **CODE** | `'echo'` | Code of the chat application ||
|| **IFRAME** | `'https://marta.bitrix.com/iframe/echo.php'` | URL of the iframe ||
|| **IFRAME_WIDTH** | `'350'` | Desired width of the iframe. Minimum value - 250px ||
|| **IFRAME_HEIGHT** | `'150'` | Desired height of the iframe. Minimum value - 50px ||
|| **HASH** | `'d1ab17949a572b0979d8db0d5b349cd2'` | Token for accessing your iframe for signature verification, 32 characters ||
|| **ICON_FILE** | `'/* base64 image */'` | Icon in base64 ||
|| **CONTEXT** | `'BOT'` | Context ||
|| **HIDDEN** | `'N'` | Is the application hidden or not ||
|| **EXTRANET_SUPPORT** | `'N'` | Is the command available to extranet users, default is N ||
|| **LIVECHAT_SUPPORT** | `'N'` | Online chat support ||
|| **IFRAME_POPUP** | `'N'` | The iframe will open with the ability to move within the messenger, switching between dialogs will not close such a window ||
|| **LANG** | `Array(...)` | Array of translations, it is advisable to specify at least for DE and EN ||
|| **LANGUAGE_ID (LANG)** | `'en'` | Language identifier for translation ||
|| **TITLE (LANG)** | `'Echobot IFRAME'` | Title for the button or command in the specified language ||
|| **DESCRIPTION (LANG)** | `'Open Echobot IFRAME app'` | Description of the command in the specified language ||
|| **COPYRIGHT (LANG)** | `'Bitrix24'` | Copyright ||
|#

**Please note:**
1. Although you specify the window sizes when registering the IFRAME application, it may be reduced by the application to the actually available sizes. The ideal option is to make it fit within the minimum sizes and be able to freely adapt to increase and decrease, which will be very important for mobile devices.
2. The link to `IFRAME` must lead to a site with an active HTTPS certificate. You can read the rules for developing IFRAME handlers and limitations [in the documentation](../iframe.md).
3. Always use hash checks before performing any operations.
4. Applications are only available after reloading the page or after a service hit to the server (this varies by installation, from 15 to 26 minutes). The application will appear instantly only when interacting [with the context button](../context.md).
5. In the values of `IFRAME_WIDTH` and `IFRAME_HEIGHT`, you specify the desired width and height of the application window. The application window will be reduced to the size of the messenger window if the messenger window is smaller. Conversely, if it is larger, the desired sizes for the application will be used.
6. If the application is called from the context (keyboard or menu), it will be invoked in context mode. In this mode, the value of `IFRAME_POPUP` will be ignored.

### Code Example

{% include [Explanation about restCommand](../../_includes/rest-command.md) %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.app.register',
        Array(
            'BOT_ID' => 62,
            'CODE' => 'echo',
            'IFRAME' => 'https://marta.bitrix.com/iframe/echo.php',
            'IFRAME_WIDTH' => '350',
            'IFRAME_HEIGHT' => '150',
            'HASH' => 'd1ab17949a572b0979d8db0d5b349cd2',
            'ICON_FILE' => '/* base64 image */',
            'CONTEXT' => 'BOT',
            'HIDDEN' => 'N',
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
        ),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

## Returned Data

Numeric identifier of the created application `APP_ID`.

## Possible Error Codes

#|
|| **Code** | **Description** ||
|| `BOT_ID_ERROR` | Chat bot not found ||
|| `APP_ID_ERROR` | The chat application does not belong to this rest application. You can only work with chat applications installed within the current rest application ||
|| `IFRAME_HTTPS` | The link to `IFRAME` must lead to a site with an active HTTPS certificate ||
|| `HASH_ERROR` | You did not specify a hash for the application. We recommend using unique hashes for each IFRAME and each installation ||
|| `PARAMS_ERROR` | Error specifying JS command or IFRAME parameters ||
|| `LANG_ERROR` | Language phrases for the visible command were not provided ||
|| `WRONG_REQUEST` | Something went wrong ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imbot-app-update.md)
- [{#T}](./imbot-app-unregister.md)