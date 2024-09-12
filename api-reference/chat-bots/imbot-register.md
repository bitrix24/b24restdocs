# Create Chat-Bot imbot.register

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- examples are missing for some parameters in the table
- examples are absent
- success response is missing
- error response is missing
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.register` registers a chat-bot.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CODE^*^**
[`unknown`](../data-types.md) | `'newbot'` | String identifier of the bot, unique within your application (required) | ||
|| **TYPE**
[`unknown`](../data-types.md) | `'B'` | Type, B – chat-bot, responses are immediate, O – chat-bot for Open Lines, S – chat-bot with elevated privileges (supervisor) | ||
|| **EVENT_...^*^**
[`unknown`](../data-types.md) | `'http://www.hazz/chatApi/event.php'` | Link to the event handler for events received from the server. Event handlers:
- `EVENT_HANDLER` – link to the handler for the event of sending a message to the chat-bot.
or
- `EVENT_MESSAGE_ADD` – link to the handler for the event of sending a message to the chat-bot.
- `EVENT_WELCOME_MESSAGE` – link to the handler for the event of opening a dialogue with the chat-bot or inviting it to a group chat.
- `EVENT_BOT_DELETE` – link to the handler for the event of deleting the chat-bot from the client side. | ||
|| **OPENLINE**
[`unknown`](../data-types.md) | `'Y'` | Enable Open Lines support mode, can be omitted if TYPE = 'O' | ||
|| **CLIENT_ID**
[`unknown`](../data-types.md) | `''` | String identifier, used only in Webhook mode | ||
|| **PROPERTIES^*^**
[`unknown`](../data-types.md) | | Personal data of the chat-bot (required) | ||
|| **NAME**
[`unknown`](../data-types.md) | `'NewBot'` | Name of the chat-bot (one of NAME or LAST_NAME is required) | ||
|| **LAST_NAME**
[`unknown`](../data-types.md) | `''` | Last name (one of NAME or LAST_NAME is required) | ||
|| **COLOR**
[`unknown`](../data-types.md) | `'GREEN'` | Color for the mobile application RED, GREEN, MINT, LIGHT_BLUE, DARK_BLUE, PURPLE, AQUA, PINK, LIME, BROWN, AZURE, KHAKI, SAND, MARENGO, GRAY, GRAPHITE | ||
|| **EMAIL**
[`unknown`](../data-types.md) | `'test@test.com'` | E-mail for contact. CANNOT use an e-mail that duplicates the e-mail of real users | ||
|| **PERSONAL_BIRTHDAY**
[`unknown`](../data-types.md) | `'2016-03-11'` | Birthday in the format YYYY-mm-dd | ||
|| **WORK_POSITION**
[`unknown`](../data-types.md) | `'Best Employee'` | Job title, used as a description of the chat-bot | ||
|| **PERSONAL_WWW**
[`unknown`](../data-types.md) | `'http://test.com'` | Link to the website | ||
|| **PERSONAL_GENDER**
[`unknown`](../data-types.md) | `'F'` | Gender, acceptable values M – male, F – female, empty if not required | ||
|| **PERSONAL_PHOTO**
[`unknown`](../data-types.md) | `'/* base64 image */'` | Avatar - base64 | ||
|#

{% include [Footnote on parameters](../../_includes/required.md) %}


**Please note**, if it is not required by your application's logic, it is recommended to respond to the bot's messages only when this chat-bot is mentioned. This can be checked by the `TO_USER_ID` field, which will be passed in the event.

{% note info %}

**Chat-bot with elevated privileges (supervisor)** has access to all messages in chats where it is present (to all if invited with access to history, and to new ones if access to history is not available).

{% endnote %}

To create it, specify the type `S` in the `TYPE` field of the **imbot.register** method.

## Examples

{% include [Explanation about restCommand](./_includes/rest-command.md) %}

```php
$result = restCommand(
    'imbot.register',
    Array(
        'CODE' => 'newbot',
        'TYPE' => 'B',
        'EVENT_HANDLER' => 'http://www.hazz/chatApi/event.php',
        'OPENLINE' => 'Y',
        'CLIENT_ID' => '',
        'PROPERTIES' => Array(
            'NAME' => 'NewBot',
            'LAST_NAME' => '',
            'COLOR' => 'GREEN',
            'EMAIL' => 'test@test.com',
            'PERSONAL_BIRTHDAY' => '2016-03-11',
            'WORK_POSITION' => 'Best Employee',
            'PERSONAL_WWW' => 'http://test.com',
            'PERSONAL_GENDER' => 'F',
            'PERSONAL_PHOTO' => '/* base64 image */',
        )
    ),
    $_REQUEST[
        "auth"
    ]
);
```

{% include [Footnote on examples](../../_includes/examples.md) %}

## Success Response

Numeric identifier of the created bot `BOT_ID`.

## Error Response

error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **EVENT_MESSAGE_ADD_ERROR** | Event handler link is invalid or not specified. ||
|| **EVENT_WELCOME_MESSAGE_ERROR** | Event handler link is invalid or not specified. ||
|| **EVENT_BOT_DELETE_ERROR** | Event handler link is invalid or not specified. ||
|| **CODE_ERROR** | String identifier is not specified. ||
|| **NAME_ERROR** | One of the required fields **NAME** or **LAST_NAME** is not specified. ||
|| **WRONG_REQUEST** | Something went wrong. ||
|| **MAX_COUNT_ERROR** | Maximum number of registered bots for one application has been reached. ||
|#

## Event Handlers

If you need to handle events with different handlers, you can specify each handler individually instead of `EVENT_HANDLER`:

```php
'EVENT_MESSAGE_ADD' => 'http://www.hazz/chatApi/event.php', // Link to the handler for the event of sending a message to the chat-bot
'EVENT_WELCOME_MESSAGE' => 'http://www.hazz/chatApi/event.php', // Link to the handler for the event of opening a dialogue with the chat-bot or inviting it to a group chat
'EVENT_BOT_DELETE' => 'http://www.hazz/chatApi/event.php', // Link to the handler for the event of deleting the chat-bot from the client side
'EVENT_MESSAGE_UPDATE' => 'http://www.hazz/chatApi/event.php', // Link to the handler for the event of subscribing to change events
'EVENT_MESSAGE_DELETE' => 'http://www.hazz/chatApi/event.php', // Link to the handler for the event of subscribing to message deletion events
```

{% note warning %}

If you plan to install more than one chat-bot within one application: *Bitrix24 Rest* imposes a restriction on working with event handlers – there can only be one handler per application. Therefore, when registering a second chat-bot, the links to the handlers **EVENT_MESSAGE_ADD**, **EVENT_WELCOME_MESSAGE**, and **EVENT_BOT_DELETE** must be the same as those of the first.

If it is necessary to handle multiple bots within one application, this must be accounted for within the event handler. For this, when an event occurs, an array of bots is passed to allow for correct processing.

The maximum number of bots for one application: 5.

{% endnote %}

## Related Links

[Rest API - Events of Installation and Update](./events/index.md)