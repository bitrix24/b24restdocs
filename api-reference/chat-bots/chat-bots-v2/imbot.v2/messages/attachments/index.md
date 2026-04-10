# Attachments in Messages ATTACH

Attachments `ATTACH` allow you to add structured content to messages: text blocks, links, images, files, dividers, and tables.

![Attachments](./_images/attach1.png){width=520}

Methods that support working with `ATTACH`:

**Chatbots 2.0 (`imbot.v2`)**

- [imbot.v2.Chat.Message.send](../chat-message-send.md) — send a message on behalf of the chatbot
- [imbot.v2.Chat.Message.update](../chat-message-update.md) — modify a chatbot message
- [imbot.v2.Command.answer](../../commands/command-answer.md) — send a chatbot response to a command

**Chats (`im`)**

- [im.message.add](../../../../../chats/messages/im-message-add.md) — send a message in a chat
- [im.message.update](../../../../../chats/messages/im-message-update.md) — modify a sent message

**Notifications (`im.notify`)**

- [im.notify](../../../../../chats/notifications/im-notify.md) — send a notification
- [im.notify.personal.add](../../../../../chats/notifications/im-notify-personal-add.md) — send a personal notification
- [im.notify.system.add](../../../../../chats/notifications/im-notify-system-add.md) — send a system notification

**Deprecated Chatbots (`imbot`)**

- [imbot.message.add](../../../../outdated/messages/imbot-message-add.md) — send a message on behalf of the chatbot
- [imbot.message.update](../../../../outdated/messages/imbot-message-update.md) — modify a sent chatbot message
- [imbot.command.answer](../../../../outdated/commands/imbot-command-answer.md) — send a chatbot response to a command

## ATTACH Object Formats

You can pass `ATTACH` in one of two formats:

1. Full form: an object with attachment metadata and an array of `BLOCKS`
2. Short form: an array of blocks without a wrapper

### Full Form ATTACH

{% list tabs %}

- JS

    ```js
    ATTACH: {
        ID: 1,
        COLOR_TOKEN: 'secondary',
        COLOR: '#29619b',
        BLOCKS: [
            {...},
            {...}
        ]
    }
    ```

- PHP

    ```php
    'ATTACH' => [
        'ID' => 1,
        'COLOR_TOKEN' => 'secondary',
        'COLOR' => '#29619b',
        'BLOCKS' => [
            [...],
            [...],
        ]
    ]
    ```

{% endlist %}

### Full Form Fields

#| 
|| **Field** 
`type` | **Description** ||
|| **ID**
[`integer`](../../../../../data-types.md) | Identifier of the attachment within the message ||
|| **COLOR_TOKEN**
[`string`](../../../../../data-types.md) | Color scheme of the attachment. Allowed values: `primary`, `secondary`, `alert`, `base`. Default: `base` ||
|| **COLOR**
[`string`](../../../../../data-types.md) | Explicit HEX color of the attachment. Used for compatibility with older scripts and in some types of notifications ||
|| **BLOCKS**
[`array`](../../../../../data-types.md) | Array of content blocks in the attachment. Block types are described in the [Block Collections](./block-collections/index.md) section ||
|#

### Example of Full Form

{% include [Example Note](../../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat20921","fields":{"message":"Attachment with primary color","attach":{"ID":1,"COLOR_TOKEN":"primary","COLOR":"#29619b","BLOCKS":[{"MESSAGE":"The API will be available in the update [B]im 24.0.0[/B]"}]}}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.Message.send
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"dialogId":"chat20921","fields":{"message":"Attachment with primary color","attach":{"ID":1,"COLOR_TOKEN":"primary","COLOR":"#29619b","BLOCKS":[{"MESSAGE":"The API will be available in the update [B]im 24.0.0[/B]"}]}},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.Message.send
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.Message.send', {
        botId: 456,
        dialogId: 'chat20921',
        fields: {
          message: 'Attachment with primary color',
          attach: {
            ID: 1,
            COLOR_TOKEN: 'primary',
            COLOR: '#29619b',
            BLOCKS: [
              {
                MESSAGE: 'The API will be available in the update [B]im 24.0.0[/B]'
              }
            ]
          }
        }
      });

      const result = response.getData().result.id;
      console.log('Created message ID:', result);
    } catch (error) {
      console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.v2.Chat.Message.send',
                [
                    'botId' => 456,
                    'dialogId' => 'chat20921',
                    'fields' => [
                        'message' => 'Attachment with primary color',
                        'attach' => [
                            'ID' => 1,
                            'COLOR_TOKEN' => 'primary',
                            'COLOR' => '#29619b',
                            'BLOCKS' => [
                                [
                                    'MESSAGE' => 'The API will be available in the update [B]im 24.0.0[/B]'
                                }
                            ]
                        ]
                    ]
                ]
            );

        $result = $response->getResponseData()->getResult()['id'];
        echo 'Created message ID: ' . $result;
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Chat.Message.send',
        {
            botId: 456,
            dialogId: 'chat20921',
            fields: {
                message: 'Attachment with primary color',
                attach: {
                    ID: 1,
                    COLOR_TOKEN: 'primary',
                    COLOR: '#29619b',
                    BLOCKS: [
                        {
                            MESSAGE: 'The API will be available in the update [B]im 24.0.0[/B]'
                        }
                    ]
                }
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log('Message ID:', result.data().id);
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.v2.Chat.Message.send',
        [
            'botId' => 456,
            'dialogId' => 'chat20921',
            'fields' => [
                'message' => 'Attachment with primary color',
                'attach' => [
                    'ID' => 1,
                    'COLOR_TOKEN' => 'primary',
                    'COLOR' => '#29619b',
                    'BLOCKS' => [
                        [
                            'MESSAGE' => 'The API will be available in the update [B]im 24.0.0[/B]'
                        }
                    ]
                ]
            ]
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Message ID: ' . $result['result']['id'];
    }
    ```

{% endlist %}

### Short Form ATTACH

If attachment metadata (`ID`, `COLOR_TOKEN`, `COLOR`) is not needed, you can directly pass an array of blocks:

{% list tabs %}

- JS

    ```js
    ATTACH: [
        {...},
        {...}
    ]
    ```

- PHP

    ```php
    'ATTACH' => [
        [...],
        [...],
    ]
    ```

{% endlist %}

### Example of Short Form

{% include [Example Note](../../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat20921","fields":{"message":"Text block","attach":[{"MESSAGE":"The API will be available in the update [B]im 24.0.0[/B]"}]}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.Message.send
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"dialogId":"chat20921","fields":{"message":"Text block","attach":[{"MESSAGE":"The API will be available in the update [B]im 24.0.0[/B]"}]},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.Message.send
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.Message.send', {
        botId: 456,
        dialogId: 'chat20921',
        fields: {
          message: 'Text block',
          attach: [
            {
              MESSAGE: 'The API will be available in the update [B]im 24.0.0[/B]'
            }
          ]
        }
      });

      const result = response.getData().result.id;
      console.log('Created message ID:', result);
    } catch (error) {
      console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.v2.Chat.Message.send',
                [
                    'botId' => 456,
                    'dialogId' => 'chat20921',
                    'fields' => [
                        'message' => 'Text block',
                        'attach' => [
                            [
                                'MESSAGE' => 'The API will be available in the update [B]im 24.0.0[/B]'
                            ]
                        ]
                    ]
                ]
            );

        $result = $response->getResponseData()->getResult()['id'];
        echo 'Created message ID: ' . $result;
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Chat.Message.send',
        {
            botId: 456,
            dialogId: 'chat20921',
            fields: {
                message: 'Text block',
                attach: [
                    {
                        MESSAGE: 'The API will be available in the update [B]im 24.0.0[/B]'
                    }
                ]
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log('Message ID:', result.data().id);
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.v2.Chat.Message.send',
        [
            'botId' => 456,
            'dialogId' => 'chat20921',
            'fields' => [
                'message' => 'Text block',
                'attach' => [
                    [
                        'MESSAGE' => 'The API will be available in the update [B]im 24.0.0[/B]'
                    }
                ]
            ]
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Message ID: ' . $result['result']['id'];
    }
    ```

{% endlist %}

## Limitations and Errors

- Maximum size of serialized `ATTACH`: `60000` characters
- An incorrect structure returns the error `ATTACH_ERROR`
- Exceeding the limit returns the error `ATTACH_OVERSIZE`

## Link Validation

Supported in attachment blocks:

- absolute URLs: `http://` and `https://`
- relative URLs from the root of Bitrix: `/company/personal/user/1/`

{% note warning %}

The content of `ATTACH` is not automatically transmitted in XMPP, email, and push notifications.

{% endnote %}

## Continue Learning

- [API imbot.v2 Change Log](../../../change-log.md)
- [{#T}](./constructor.md)
- [{#T}](./block-collections/index.md)
- [{#T}](../message-keyboards.md)
- [{#T}](../chat-message-send.md)
- [{#T}](../chat-message-update.md)
- [{#T}](../../../../../chats/notifications/im-notify.md)