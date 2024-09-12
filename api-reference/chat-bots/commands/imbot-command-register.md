# Add the imbot.command.register Team

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- not all parameters have examples in the table
- examples are missing
- success response is absent
- error response is absent
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
[`unknown`](../../data-types.md) | `62` | Identifier of the chat bot that owns the team | ||
|| **COMMAND**
[`unknown`](../../data-types.md) | `'echo'` | The text of the command that the user will enter in chats | ||
|| **COMMON**
[`unknown`](../../data-types.md) | `'Y'` | If specified as Y, the command is available in all chats; if N, it is only available in those where the chat bot is present | ||
|| **HIDDEN**
[`unknown`](../../data-types.md) | `'N'` | Whether the command is hidden or not - default is N | ||
|| **EXTRANET_SUPPORT**
[`unknown`](../../data-types.md) | `'N'` | Whether the command is available to Extranet users, default is N | ||
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
 | Array of translations, at least for RU and EN must be specified | ||
|| **EVENT_COMMAND_ADD**
[`unknown`](../../data-types.md) | `'http://www.hazz/chatApi/bot.php'` | Link to the handler for commands | ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

{% note warning %}

To process the command, the application must handle the command addition event [ONIMCOMMANDADD](./events/index.md).

{% endnote %}

{% note warning %}

Attention! If you plan to install more than one command for the chat bot: Bitrix24 Rest imposes a restriction on working with event handlers - there can only be one handler per application. Therefore, when registering a second command, the links to the handlers `EVENT_COMMAND_ADD` must be the same as for the first command.

If it is necessary to handle multiple commands within one application, this must be accounted for within the event handler. For this, an array of commands is passed when the event occurs, so they can be processed correctly.

{% endnote %}

{% note warning %}

It is mandatory to specify the array of translations `LANG` for at least RU and EN. If there is no phrase for BY, UA, KZ, the RU phrases will be shown by default; if there is no phrase in RU, the command will be hidden. The same applies to other languages - if there are no phrases, the EN phrases will be shown by default; if there is no phrase in EN, the command will be hidden in the public part.

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

{% include [Example Notes](../../../_includes/examples.md) %}

## Success Response

The command identifier `COMMAND_ID`.

## Error Response

error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **EVENT_COMMAND_ADD** | The event handler link is invalid or not specified. ||
|| **COMMAND_ERROR** | The command text that the chat bot should respond to is not specified. ||
|| **BOT_ID_ERROR** | The chat bot was not found. ||
|| **APP_ID_ERROR** | The chat bot does not belong to this application. It can only work with chat bots installed within the application. ||
|| **LANG_ERROR** | Language phrases for the visible command were not provided. ||
|| **WRONG_REQUEST** | Something went wrong. ||
|#

## Related Links

- [Event for the chat bot to receive the command ONIMCOMMANDADD](./events/index.md)