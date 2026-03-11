# Attachments in Messages ATTACH

Attachments `ATTACH` allow you to add structured content to messages: text blocks, links, images, files, dividers, and tables.

![Attachments](./_images/attach1.png)

Methods that support working with `ATTACH`:

- [im.message.add](../im-message-add.md) — send a message in a chat
- [im.message.update](../im-message-update.md) — modify a sent message
- [im.notify](../../notifications/im-notify.md) — send a notification
- [im.notify.personal.add](../../notifications/im-notify-personal-add.md) — send a personal notification
- [im.notify.system.add](../../notifications/im-notify-system-add.md) — send a system notification
- [imbot.message.add](../../../chat-bots/messages/imbot-message-add.md) — send a message on behalf of a chat bot
- [imbot.message.update](../../../chat-bots/messages/imbot-message-update.md) — modify a chat bot's message
- [imbot.command.answer](../../../chat-bots/commands/imbot-command-answer.md) — send a chat bot's response to a command

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
[`integer`](../../../data-types.md) | Identifier of the attachment within the message ||
|| **COLOR_TOKEN**
[`string`](../../../data-types.md) | Color scheme of the attachment. Allowed values: `primary`, `secondary`, `alert`, `base`. Default: `base` ||
|| **COLOR**
[`string`](../../../data-types.md) | Explicit HEX color of the attachment. Used for compatibility with older scripts and in some types of notifications ||
|| **BLOCKS**
[`array`](../../../data-types.md) | Array of content blocks in the attachment. Block types are described in the [Block Collections](./block-collections/index.md) section ||
|#

![ATTACH Object](./_images/attach_variants.png)

### Example of Full Form

{% include [Example Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat20921","MESSAGE":"Attachment with primary color","ATTACH":{"ID":1,"COLOR_TOKEN":"primary","COLOR":"#29619b","BLOCKS":[{"MESSAGE":"The API will be available in update [B]im 24.0.0[/B]"}]}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat20921","MESSAGE":"Attachment with primary color","ATTACH":{"ID":1,"COLOR_TOKEN":"primary","COLOR":"#29619b","BLOCKS":[{"MESSAGE":"The API will be available in update [B]im 24.0.0[/B]"}]},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.message.add
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.message.add', {
        DIALOG_ID: 'chat20921',
        MESSAGE: 'Attachment with primary color',
        ATTACH: {
          ID: 1,
          COLOR_TOKEN: 'primary',
          COLOR: '#29619b',
          BLOCKS: [
            {
              MESSAGE: 'The API will be available in update [B]im 24.0.0[/B]'
            }
          ]
        }
      });

      const { result } = response.getData();
      console.log(result);
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
                'im.message.add',
                [
                    'DIALOG_ID' => 'chat20921',
                    'MESSAGE' => 'Attachment with primary color',
                    'ATTACH' => [
                        'ID' => 1,
                        'COLOR_TOKEN' => 'primary',
                        'COLOR' => '#29619b',
                        'BLOCKS' => [
                            [
                                'MESSAGE' => 'The API will be available in update [B]im 24.0.0[/B]'
                            ]
                        ]
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        error_log($e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.message.add',
        {
            DIALOG_ID: 'chat20921',
            MESSAGE: 'Attachment with primary color',
            ATTACH: {
                ID: 1,
                COLOR_TOKEN: 'primary',
                COLOR: '#29619b',
                BLOCKS: [
                    {
                        MESSAGE: 'The API will be available in update [B]im 24.0.0[/B]'
                    }
                ]
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
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
        'im.message.add',
        [
            'DIALOG_ID' => 'chat20921',
            'MESSAGE' => 'Attachment with primary color',
            'ATTACH' => [
                'ID' => 1,
                'COLOR_TOKEN' => 'primary',
                'COLOR' => '#29619b',
                'BLOCKS' => [
                    [
                        'MESSAGE' => 'The API will be available in update [B]im 24.0.0[/B]'
                    ]
                ]
            ]
        ]
    );

    print_r($result);
    ```

{% endlist %}

### Short Form ATTACH

If attachment metadata (`ID`, `COLOR_TOKEN`, `COLOR`) is not needed, you can pass an array of blocks directly:

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

![Short Version of ATTACH](./_images/short_attach.png)

### Example of Short Form

{% include [Example Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat20921","MESSAGE":"Text block","ATTACH":[{"MESSAGE":"The API will be available in update [B]im 24.0.0[/B]"}]}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat20921","MESSAGE":"Text block","ATTACH":[{"MESSAGE":"The API will be available in update [B]im 24.0.0[/B]"}],"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.message.add
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.message.add', {
        DIALOG_ID: 'chat20921',
        MESSAGE: 'Text block',
        ATTACH: [
          {
            MESSAGE: 'The API will be available in update [B]im 24.0.0[/B]'
          }
        ]
      });

      const { result } = response.getData();
      console.log(result);
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
                'im.message.add',
                [
                    'DIALOG_ID' => 'chat20921',
                    'MESSAGE' => 'Text block',
                    'ATTACH' => [
                        [
                            'MESSAGE' => 'The API will be available in update [B]im 24.0.0[/B]'
                        ]
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        error_log($e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.message.add',
        {
            DIALOG_ID: 'chat20921',
            MESSAGE: 'Text block',
            ATTACH: [
                {
                    MESSAGE: 'The API will be available in update [B]im 24.0.0[/B]'
                }
            ]
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
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
        'im.message.add',
        [
            'DIALOG_ID' => 'chat20921',
            'MESSAGE' => 'Text block',
            'ATTACH' => [
                [
                    'MESSAGE' => 'The API will be available in update [B]im 24.0.0[/B]'
                ]
            ]
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Limitations and Errors

- Maximum size of serialized `ATTACH`: `60000` characters
- An incorrect structure returns the error `ATTACH_ERROR`
- Exceeding the limit returns the error `ATTACH_OVERSIZE`

## Link Validation

Supported in attachment blocks:

- absolute URLs: `http://` and `https://`
- relative URLs from the Bitrix root: `/company/personal/user/1/`

{% note warning %}

The content of `ATTACH` is not automatically transmitted in XMPP, email, and push notifications.

{% endnote %}

## Continue Learning

- [{#T}](./constructor.md)
- [{#T}](./block-collections/index.md)
- [{#T}](../im-message-add.md)
- [{#T}](../im-message-update.md)
- [{#T}](../../notifications/im-notify.md)