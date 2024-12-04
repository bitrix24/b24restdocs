# Update Chat Bot imbot.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- not all parameters have examples in the table
- examples are missing
- no response in case of success
- no response in case of error
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.update` updates the bot's data.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **BOT_ID^*^**
[`unknown`](../data-types.md) | `39` | Identifier of the chat bot to be updated | ||
|| **CLIENT_ID**
[`unknown`](../data-types.md) | `''` | String identifier of the chat bot, used only in Webhook mode | ||
|| **FIELDS^*^**
[`unknown`](../data-types.md) | | Data for updating | ||
|| **CODE**
[`unknown`](../data-types.md) | `'newbot'` | String identifier of the chat bot, unique within your application | ||
|| **EVENT_...^*^**
[`unknown`](../data-types.md) | `'http://www.hazz/chatApi/event.php'` | Link to the event handler for events received from the server. Event handlers:
- `EVENT_HANDLER` - link to the event handler for sending messages to the chat bot.
or
- `EVENT_MESSAGE_ADD` - link to the event handler for sending messages to the chat bot.
- `EVENT_WELCOME_MESSAGE` - link to the event handler for opening a dialogue with the chat bot or inviting it to a group chat.
- `EVENT_BOT_DELETE` - link to the event handler for deleting the chat bot from the client side.
 | ||
|| **PROPERTIES^*^**
[`unknown`](../data-types.md) | | Required when updating the bot's data | ||
|| **NAME**
[`unknown`](../data-types.md) | `'UpdatedBot'` | Name of the chat bot | ||
|| **LAST_NAME**
[`unknown`](../data-types.md) | `''` | Last name of the chat bot | ||
|| **COLOR**
[`unknown`](../data-types.md) | `'MINT'` | Color of the chat bot for the mobile application: RED, GREEN, MINT, LIGHT_BLUE, DARK_BLUE, PURPLE, AQUA, PINK, LIME, BROWN, AZURE, KHAKI, SAND, MARENGO, GRAY, GRAPHITE | ||
|| **EMAIL**
[`unknown`](../data-types.md) | `'test2@test.com'` | E-mail for contact | ||
|| **PERSONAL_BIRTHDAY**
[`unknown`](../data-types.md) | `'2016-03-12'` | Birthday in YYYY-mm-dd format | ||
|| **WORK_POSITION**
[`unknown`](../data-types.md) | `'My second bot'` | Job title, used as a description of the chat bot | ||
|| **PERSONAL_WWW**
[`unknown`](../data-types.md) | `'http://test2.com'` | Link to the website | ||
|| **PERSONAL_GENDER**
[`unknown`](../data-types.md) | `'M'` | Gender of the bot, acceptable values are M - male, F - female, empty if not required | ||
|| **PERSONAL_PHOTO**
[`unknown`](../data-types.md) | `'/* base64 image */'` | Avatar of the chat bot - base64 | ||
|#

{% include [Note on parameters](../../_includes/required.md) %}

{% note warning "" %}

The method **imbot.update** no longer supports changing the fields `TYPE` and `OPENLINE` (im 17.5.10).

{% endnote %}

## Examples

{% include [Explanation of restCommand](./_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.update',
        Array(
            'BOT_ID' => 39,
            'CLIENT_ID' => '',
            'FIELDS' => Array(
                'CODE' => 'newbot',
                'EVENT_HANDLER' => 'http://www.hazz/chatApi/event.php',
                'PROPERTIES' => Array(
                    'NAME' => 'UpdatedBot',
                    'LAST_NAME' => '',
                    'COLOR' => 'MINT',
                    'EMAIL' => 'test2@test.com',
                    'PERSONAL_BIRTHDAY' => '2016-03-12',
                    'WORK_POSITION' => 'My second bot',
                    'PERSONAL_WWW' => 'http://test2.com',
                    'PERSONAL_GENDER' => 'M',
                    'PERSONAL_PHOTO' => '/* base64 image */',
                )
            )
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Note on examples](../../_includes/examples.md) %}

## Response in case of success

`true`

## Response in case of error

error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **BOT_ID_ERROR** | Chat bot not found. ||
|| **APP_ID_ERROR** | The chat bot does not belong to this application; you can only work with chat bots installed within the application. ||
|| **EVENT_MESSAGE_ADD_ERROR** | Event handler link is invalid or not specified. ||
|| **EVENT_WELCOME_MESSAGE_ERROR** | Event handler link is invalid or not specified. ||
|| **EVENT_BOT_DELETE_ERROR** | Event handler link is invalid or not specified. ||
|| **NAME_ERROR** | One of the required fields **NAME** or **LAST_NAME** of the chat bot is not specified. ||
|| **WRONG_REQUEST** | Something went wrong. ||
|#

## Event Handlers

If you need to handle events with different handlers, you can specify each handler individually instead of `EVENT_HANDLER`:

```php
'EVENT_MESSAGE_ADD' => 'http://www.hazz/chatApi/event.php', // Link to the event handler for sending messages to the chat bot
'EVENT_WELCOME_MESSAGE' => 'http://www.hazz/chatApi/event.php', // Link to the event handler for opening a dialogue with the chat bot or inviting it to a group chat
'EVENT_BOT_DELETE' => 'http://www.hazz/chatApi/event.php', // Link to the event handler for deleting the chat bot from the client side
'EVENT_MESSAGE_UPDATE' => 'http://www.hazz/chatApi/event.php', // Link to the event handler for subscribing to change events
'EVENT_MESSAGE_DELETE' => 'http://www.hazz/chatApi/event.php', // Link to the event handler for subscribing to message deletion events
```

## Related Links

[Rest API - Events of installation and update](./events/index.md)