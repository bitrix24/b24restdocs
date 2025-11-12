# Get the list of recent messages im.dialog.messages.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant

The method `im.dialog.messages.get` retrieves the list of recent messages in a chat.

{% include [Notes on parameters](../../../_includes/required.md) %}

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **DIALOG_ID^*^**
[`unknown`](../../data-types.md) | `chat29`
or
`256` | Identifier of the dialog. Format:
- **chatXXX** – chat of the recipient, if the message is for a chat
- **XXX** – identifier of the recipient, if the message is for a private dialog | 19 ||
|| **LAST_ID**
[`unknown`](../../data-types.md) | `28561624` | Identifier of the last loaded message | 19 ||
|| **FIRST_ID**
[`unknown`](../../data-types.md) | `454322` | Identifier of the first loaded message | 19 ||
|| **LIMIT**
[`unknown`](../../data-types.md) | `20` | Limit on the number of messages retrieved in the dialog | 19 ||
|#

{% note info "" %}

You can retrieve messages without being a participant in the chat only for open line chats. For this, use the method [imopenlines.session.history.get](../../imopenlines/openlines/sessions/imopenlines-session-history-get.md).

{% endnote %}

- If the keys `LAST_ID` and `FIRST_ID` are not provided, the last 20 messages of the chat will be loaded.
- To load the previous 20 messages, you need to pass `LAST_ID` with the identifier of the minimum message ID obtained from the last selection.
- If you need to load the next 20 messages, you must pass `FIRST_ID` with the identifier of the maximum message ID obtained from the last selection.
- If you need to load the first 20 messages, you must pass `FIRST_ID` with the identifier 0, and you will receive the result with the very first available message in this chat.

{% note warning %}

Due to the potentially large volume of data, this method does not support standard pagination in the Bitrix24 Rest API.

{% endnote %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.dialog.messages.get',
            {
                DIALOG_ID: 'chat29'
            }
        );
        
        const result = response.getData().result;
        console.log(result);
    }
    catch( error )
    {
        console.error(error.ex);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.dialog.messages.get',
                [
                    'DIALOG_ID' => 'chat29'
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error()->ex;
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting dialog messages: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.dialog.messages.get',
        {
            DIALOG_ID: 'chat29'
        },
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    {% include [Explanation about restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.dialog.messages.get',
        Array(
            'DIALOG_ID' => 'chat29'
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

- cURL

    // example for cURL

{% endlist %}

{% include [Notes on examples](../../../_includes/examples.md) %}

## Successful response

```json
{
    "result": {
        "messages": [
         {
            "id": 28561624,
            "chat_id": 29,
            "author_id": 1,
            "date": "2018-01-29T16:58:47+02:00",
            "text": "https://my-domain.com",
            "params": {
             "URL_ID": [
                5
             ],
             "URL_ONLY": "Y",
             "ATTACH": [
                {
                 "ID": "5",
                 "BLOCKS": [
                    {
                     "RICH_LINK": [
                        {
                         "NAME": "My site",
                         "LINK": "https://my-domain.com",
                         "DESC": "My personal site",
                         "PREVIEW": "https://my-domain.com/logo/i/share-logo.png"
                        }
                     ]
                    }
                 ],
                 "COLOR": "transparent"
                }
             ]
            }
         },
         {
            "id": 28561623,
            "chat_id": 29,
            "author_id": 28,
            "date": "2018-01-29T16:43:38+02:00",
            "text": "",
            "params": {
             "FILE_ID": [
                540
             ]
            }
         },
         {
            "id": 28561622,
            "chat_id": 29,
            "author_id": 1,
            "date": "2018-01-29T16:43:12+02:00",
            "text": "It's working yes :) :)",
            "params": {
             "IS_EDITED": "Y"
            }
         },
         {
            "id": 28561618,
            "chat_id": 29,
            "author_id": 0,
            "date": "2018-01-25T15:15:22+02:00",
            "text": "Eugene Shelenkov changed the chat topic to \"Big chat\"",
            "params": null
         }
        ],
        "users": [
         {
            "id": 1,
            "name": "Eugene Shelenkov",
            "first_name": "Eugene",
            "last_name": "Shelankov",
            "work_position": "",
            "color": "#df532d",
            "avatar": "http://192.168.2.232/upload/resize_cache/main/1d3/100_100_2/shelenkov.png",
            "gender": "M",
            "birthday": "",
            "extranet": false,
            "network": false,
            "bot": false,
            "connector": false,
            "external_auth_id": "default",
            "status": "online",
            "idle": false,
            "last_activity_date": "2018-01-29T17:35:31+02:00",
            "mobile_last_date": false,
            "departments": [
             50
            ],
            "absent": false,
            "phones": {
             "work_phone": "",
             "personal_mobile": "",
             "personal_phone": ""
            }
         },
         {
            "id": 28,
            "name": "Ivan Elpidin",
            "first_name": "Ivan",
            "last_name": "Elpidin",
            "work_position": "manager",
            "color": "#728f7a",
            "avatar": "http://192.168.2.232/upload/resize_cache/main/8b8/100_100_2/26.jpg",
            "gender": "M",
            "birthday": "26-01",
            "extranet": false,
            "network": false,
            "bot": false,
            "connector": false,
            "external_auth_id": "default",
            "status": "online",
            "idle": false,
            "last_activity_date": false,
            "mobile_last_date": false,
            "departments": [
             58
            ],
            "absent": false,
            "phones": {
             "work_phone": "+1 (495) 222-33-55",
             "personal_mobile": "+1 (495) 123-55-65",
             "personal_phone": ""
            }
         }
        ],
        "files": [
            {
                "id": 540,
                "chatId": 29,
                "date": "2018-01-29T16:43:39+02:00",
                "type": "image",
                "preview": "",
                "name": "1176297_698081120237288_696773366_n.jpeg",
                "size": 55013,
                "image": {
                    "width": 960,
                    "height": 640
                },
                "status": "done",
                "progress": 100,
                "authorId": 1,
                "authorName": "Eugene Shelenkov",
                "urlPreview": "http://192.168.2.232/bitrix/components/bitrix/im.messenger/show.file.php?fileId=540&preview=Y&fileName=1176297_698081120237288_696773366_n.jpeg",
                "urlShow": "http://192.168.2.232/bitrix/components/bitrix/im.messenger/show.file.php?fileId=540&fileName=1176297_698081120237288_696773366_n.jpeg",
                "urlDownload": "http://192.168.2.232/bitrix/components/bitrix/im.messenger/download.file.php?fileId=540"
            }
        ],
        "chat_id": 29
    }
}
```

### Description of keys

- `messages` – array of messages:

    - `id` – message identifier
    - `chat_id` – chat identifier
    - `author_id` – message author (0 - if the message is system)
    - `date` – message date in ATOM format
    - `text` – message text
    - `params` – message parameters, an object of parameters, if parameters are not provided `null` (the main types will be described below)

- `users` – objects describing user data:

    - `id` – user identifier
    - `name` – user's full name
    - `first_name` – user's first name
    - `last_name` – user's last name
    - `work_position` – position
    - `color` – user's color in hex format
    - `avatar` – link to the avatar (if empty, the avatar is not set)
    - `gender` – user's gender
    - `birthday` – user's birthday in DD-MM format, if empty – not set
    - `extranet` – indicator of external extranet user (`true/false`)
    - `network` – indicator of Bitrix24.Network user (`true/false`)
    - `bot` – indicator of bot (`true/false`)
    - `connector` – indicator of open lines user (`true/false`)
    - `external_auth_id` – external authorization code
    - `status` – selected user status
    - `idle` – date when the user stepped away from the computer, in ATOM format (if not set, `false`)
    - `last_activity_date` – date of the user's last action in ATOM format
    - `mobile_last_date` – date of the last action in the mobile application in ATOM format (if not set, `false`)
    - `absent` – date until which the user is on vacation, in ATOM format (if not set, `false`)

- `files` – object describing files in the selected messages:

    - `id` – file identifier
    - `chatId` – chat identifier
    - `date` – file modification date
    - `type` – file type: `image` – image, `file` – file
    - `name` – name of the uploaded file
    - `size` – actual size of the image in bytes
    - `image` – actual size of the image in px
    - `width` – width in px
    - `height` – height in px
    - `status` – current status: `done` – uploaded, `upload` – uploading
    - `progress` – upload status in percentage: `100` – uploaded, `-1` – current status not available
    - `authorId` – identifier of the user who uploaded the object
    - `authorName` – user's full name who uploaded the object
    - `urlPreview` – link to the image preview (available for images)
    - `urlShow` – link to view the object in "view" mode
    - `urlDownload` – link to start the download

- `chat_id` – chat identifier

### Description of additional parameters

- `ATTACH` – object containing rich formatting
- `KEYBOARD` – object containing keyboard description
- `IS_DELETED` – flag indicating that the message is deleted
- `IS_EDITED` – flag indicating that the message is edited
- `FILE_ID` – array of file identifiers
- `LIKE` – array of user identifiers who voted for the message

## Error response

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

### Description of keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **DIALOG_ID_EMPTY** | Dialog identifier not provided ||
|| **ACCESS_ERROR** | The current user does not have access permission to the dialog ||
|#