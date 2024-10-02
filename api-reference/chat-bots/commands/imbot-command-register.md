# Add the command imbot.command.register

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter mandatory status is not indicated
- not all parameters have examples in the table
- examples are missing
- success response is missing
- error response is missing
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.command.register` registers a command for processing by the chat bot.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **BOT_ID**
[`unknown`](../../data-types.md) | `62` | Identifier of the chat bot that owns the command | ||
|| **COMMAND**
[`unknown`](../../data-types.md) | `'echo'` | The text of the command that the user will enter in chats.

Only Latin letters and numbers can be used. Spaces and special characters are not accepted | ||
|| **COMMON**
[`unknown`](../../data-types.md) | `'Y'` | If Y is specified, the command is available in all chats; if N, it is only available in those where the chat bot is present | ||
|| **HIDDEN**
[`unknown`](../../data-types.md) | `'N'` | Whether the command is hidden or not - defaults to N | ||
|| **EXTRANET_SUPPORT**
[`unknown`](../../data-types.md) | `'N'` | Whether the command is available to Extranet users, defaults to N | ||
|| **CLIENT_ID**
[`unknown`](../../data-types.md) | `''` | String identifier of the chat bot, used only in Webhook mode | ||
|| **LANG^*^**
[`unknown`](../../data-types.md) | 
```php
Array(
    Array(
        'LANGUAGE_ID' => 'en',
        'TITLE' => 'Get echo message',
        'PARAMS' => 'some text'
    )
)
```
 | Array of translations, at least for DE and EN must be specified | ||
|| **EVENT_COMMAND_ADD**
[`unknown`](../../data-types.md) | `'http://www.hazz/chatApi/bot.php'` | Link to the handler for commands | ||
|#

{% include [Notes on parameters](../../../_includes/required.md) %}

{% note warning %}

To process the command, the application must handle the event of adding a command [ONIMCOMMANDADD](./events/index.md).

{% endnote %}

{% note warning %}

Attention! If you plan to install more than one command for the chat bot: Bitrix24 Rest imposes a limitation on working with event handlers - there can only be one handler per application. Therefore, when registering a second command, the links to the handlers `EVENT_COMMAND_ADD` must be the same as those of the first command.

If it is necessary to handle multiple commands within one application, this must be accounted for within the event handler. For this, when the event occurs, an array of commands is passed to allow for correct processing.

{% endnote %}

{% note warning %}

It is mandatory to specify the array of translations `LANG` for at least DE and EN. If there is no phrase for BY, UA, KZ, the phrases from DE will be shown by default; if there is no phrase in DE, the command will be hidden. The same applies to other languages - if there are no phrases, the phrases from EN will be shown by default; if there is no phrase in EN, the command will be hidden in the public part.

{% endnote %}

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'imbot.command.register',
    Array(
        'BOT_ID' => 62,
        'COMMAND' => 'echo',
        'COMMON' => 'Y',
        'HIDDEN' => 'N',
        'EXTRANET_SUPPORT' => 'N',
        'CLIENT_ID' => '',
        'LANG' => Array(
            Array(
                'LANGUAGE_ID' => 'en',
                'TITLE' => 'Get echo message',
                'PARAMS' => 'some text'
            ),
        ),
        'EVENT_COMMAND_ADD' => 'http://www.hazz/chatApi/bot.php',
    ),
    $_REQUEST[
        "auth"
    ]
);
```

{% include [Notes on examples](../../../_includes/examples.md) %}

## Success response

The command identifier `COMMAND_ID`.

## Error response

error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **EVENT_COMMAND_ADD** | The event handler link is invalid or not specified. ||
|| **COMMAND_ERROR** | The text of the command that the chat bot should respond to is not specified. ||
|| **BOT_ID_ERROR** | The chat bot was not found. ||
|| **APP_ID_ERROR** | The chat bot does not belong to this application. Only chat bots installed within the application can be used. ||
|| **LANG_ERROR** | Language phrases for the visible command were not provided. ||
|| **WRONG_REQUEST** | Something went wrong. ||
|#

## Related links

- [Event for receiving a command by the chat bot ONIMCOMMANDADD](./events/index.md)