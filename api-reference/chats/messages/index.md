# Formatting

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet the writing standard

{% endnote %}

{% endif %}

Messages can be formatted, with options for bold text, strikethrough, and quotes. The lesson provides user commands, appearance, and REST API for formatting chat messages.

> Quick navigation: [all methods](#all-methods)

## Formatting Codes

If you send the following message in chat:

```markdown
[B]bold[/B] text
[U]underlined[/U] text
[I]italic[/I] text
[S]strikethrough[/S] text
```

It will be displayed as follows:

![Formatting Result](./_images/bbcode1.png)

Formatting using the REST API:

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

```php
restCommand(
    'imbot.message.add',
    Array(
        "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
        "MESSAGE" => "bold text
            underlined text
            italic text
            strikethrough text",
    ),
    $_REQUEST["auth"]
);
```

## Line Breaks

Line breaks can be added to the text using the following characters:

```markdown
[BR]
\# BR \# (without spaces)
\n
```

Any of these codes will create a line break:

![Line Break Result](./_images/br1.png)

Line breaks using the REST API:

```php
restCommand(
    'imbot.message.add',
    Array(
        "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
        "MESSAGE" => "First line
            Second line",
    ),
    $_REQUEST["auth"]
);
```

## Quoting

You can quote text in two ways:

```markdown
>>first line of the quote
>>second line of the quote
------------------------------------------------------
Dmitry Vlasov08.04.2016 13:06:49
Hello everyone
------------------------------------------------------
```

The appearance of the quote will differ slightly - in the second case, the author and the time of the quote will be indicated:

![Quote Result](./_images/quote1.png)

Quoting using the REST API:

```php
restCommand(
    'imbot.message.add',
    Array(
        "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
        "MESSAGE" => ">>first line of the quote
            >>second line of the quote
            ------------------------------------------------------
            Dmitry Vlasov08.04.2016 13:06:49
            Hello everyone
            ------------------------------------------------------",
    ),
    $_REQUEST["auth"]
);
```

## Links

Any link in the text will automatically become clickable. If the link address has "rich formatting," the link will automatically pick it up:

![Link Result](./_images/link1.png)

Links using the REST API:

```php
restCommand(
    'imbot.message.add',
    Array(
        "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
        "MESSAGE" => "http://bitrix24.com",
    ),
    $_REQUEST["auth"]
);
```
{% note info %}

"Rich formatting" can be disabled by passing the key `'URL_PREVIEW' => 'N'` when creating the message:

```php
restCommand(
    'imbot.message.add',
    Array(
        "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
        "MESSAGE" => "http://bitrix24.com",
        "URL_PREVIEW" => "N"
    ),
    $_REQUEST["auth"]
);
```

{% endnote %}

If you send a link to an image `https://files.shelenkov.com/bitrix/images/mantis.jpg` (the link must end with .png, .jpg, .gif), it will automatically convert to an image:

![Link Result](./_images/link2.png)

You can create a link manually through the **URL** code - `[URL=http://bitrix24.com]Link to Bitrix24[/URL]`:

![Link Result](./_images/link3.png)

Creating a link using the REST API:
```php
restCommand(
    'imbot.message.add',
    Array(
        "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
        "MESSAGE" => "[URL=http://bitrix24.com]Link to Bitrix24[/URL]",
    ),
    $_REQUEST["auth"]
);
```

Similarly to the **URL** code, there are special codes for links within the messenger.

- `[USER=5]Martha[/USER]` - link to a user.
- `[CALL=84012334455]call[/CALL]` - button to make a call through *Bitrix24*.
- `[CHAT=12]link to chat[/CHAT]` - link to a chat.

![Link Result](./_images/link4.png)

Formatting special codes using the REST API:

```php
restCommand(
    'imbot.message.add',
    Array(
        "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
        "MESSAGE" => "[USER=5]Martha[/USER]
            [CALL=84012334455]call[/CALL]
            [CHAT=12]link to chat[/CHAT]",
    ),
    $_REQUEST["auth"]
);
```

## Indents

To create indents in a message, use the tab character:

[send=text]button name[/send] - instant sending of text to the bot.

Indents using the REST API:
```php
restCommand(
    'imbot.message.add',
    Array(
        "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
        "MESSAGE" => "
            Indent
                Indent
                    Indent",
    ),
    $_REQUEST["auth"]
);
```

## Active Links (Commands)

If you want the user to send some text by clicking on a link, use the **SEND** tag:
```markdown
[send=text]button name[/send] - instant sending of text to the bot.
```

With this tag, you can prompt the user to send a command to your bot, but there is a more preferred way - [Keyboards](.)

![Link Result](./_images/command1.png)

Active links (commands) using the REST API:

```php
restCommand(
    'imbot.message.add',
    Array(
        "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
        "MESSAGE" => "[send=text]button name[/send] - instant sending of text to the bot",
    ),
    $_REQUEST["auth"]
);
```

If you need the user to add something to the command, use the **PUT** code:

```markdown
[put=/search]Enter search string[/put]
```

Sending the bot command using the REST API:

```php
restCommand(
    'imbot.message.add',
    Array(
        "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
        "MESSAGE" => "Enter search string",
    ),
    $_REQUEST["auth"]
);
```

## Icons

Adding your own icon to a message is done by sending the code:

```markdown
[icon=http://files.shelenkov.com/images/unicorn.png size=30 title=Unicorn]
```

The icon will be displayed as follows.


After this, the icon will also be added to the Business Chat emoji set. You can remove the icon from the set by right-clicking on the icon in the set and selecting **Delete**.

A required property is to specify the path to the image (without spaces).

Additional attributes are available:
- **title** - title;
- **height** - height;
- **width** - width;
- **size** - height and width.

For optimal display on all devices, the icon size should be twice as large as specified in the display parameters.

Adding your own icon using the REST API:

```php
restCommand(
    'imbot.message.add',
    Array(
        "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
        "MESSAGE" => "[icon=http://files.shelenkov.com/images/unicorn.png size=30 title=Unicorn]",
    ),
    $_REQUEST["auth"]
);
```

{% note info %}

For more details on how to use advanced format attachments within messages, read [here](./attachments/index.md).

{% endnote %}

## Overview of Methods {#all-methods}

#| 
|| **Method** | **Description** ||
|| [im.dialog.messages.get](./im-dialog-messages-get.md) | Retrieves a list of recent messages ||
|| [im.dialog.read](./im-dialog-read.md) | Marks messages as "read" ||
|| [im.dialog.unread](./im-dialog-unread.md) | Marks messages as "unread" ||
|| [im.dialog.writing](./im-dialog-writing.md) | Sends the "someone is typing..." indicator ||
|| [im.message.add](./im-message-add.md) | Adds a message ||
|| [im.message.command](./im-message-command.md) | Uses a bot command ||
|| [im.message.delete](./im-message-delete.md) | Deletes a chatbot message ||
|| [im.message.like](./im-message-like.md) | Changes the "like" status of a message ||
|| [im.message.share](./im-message-share.md) | Creates an object based on a message ||
|| [im.message.update](./im-message-update.md) | Modifies a sent message ||
|#