# Get the List of Recent Messages im.dialog.messages.get

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant

The method `im.dialog.messages.get` retrieves messages from the specified dialog, including system messages. It does not support standard pagination due to the potential large volume of data.

{% note info "" %}

Messages can only be retrieved without participating in the chat for Open Line chats via the method [imopenlines.session.history.get](../../imopenlines/openlines/sessions/imopenlines-session-history-get.md).

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier of the chat in the format:

- `chatXXX` — chat
- `sgXXX` — group or project chat
- `XXX` — user identifier for personal chat

The chat identifier can be obtained using the method [im.chat.get](../im-chat-get.md). The user identifier can be obtained using the methods [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) ||
|| **LAST_ID**
[`integer`](../../data-types.md) | Identifier of the message older than which messages need to be loaded. The method will return messages with identifiers less than the specified one. ||
|| **FIRST_ID**
[`integer`](../../data-types.md) | Identifier of the message newer than which messages need to be loaded. The method will return messages with identifiers greater than the specified one.

To load the very first messages of the dialog — pass `FIRST_ID` with the value `0`. ||
|| **LIMIT**
[`integer`](../../data-types.md) | Limit on the number of messages in the response. If `LAST_ID` and `FIRST_ID` are not provided, the method will return the last N messages of the dialog specified in `LIMIT`.

Default is `20`. Maximum value is `50`. ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat1489","FIRST_ID":84869,"LIMIT":10}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.dialog.messages.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat1489","FIRST_ID":84869,"LIMIT":10,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.dialog.messages.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.dialog.messages.get',
            {
                DIALOG_ID: 'chat1489',
                FIRST_ID: 84869,
                LIMIT: 10
            }
        );

        console.log(response.getData().result);
    }
    catch (error)
    {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.dialog.messages.get',
            [
                'DIALOG_ID' => 'chat1489',
                'FIRST_ID' => 84869,
                'LIMIT' => 10,
            ]
        );

        $result = $response->getResponseData()->getResult();
        print_r($result);
    } catch (Throwable $e) {
        error_log($e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.dialog.messages.get',
        {
            DIALOG_ID: 'chat1489',
            FIRST_ID: 84869,
            LIMIT: 10
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.dialog.messages.get',
        [
            'DIALOG_ID' => 'chat1489',
            'FIRST_ID' => 84869,
            'LIMIT' => 10,
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "chat_id": 1489,
        "messages": [
            {
                "id": 84877,
                "chat_id": 1489,
                "author_id": 503,
                "date": "2026-03-04T09:43:26+02:00",
                "text": "We are very glad to see you!",
                "unread": false,
                "uuid": "0c42a08f-4235-49fc-994f-c9bccd499ac1",
                "replaces": [],
                "params": {
                    "LIKE": [547]
                },
                "disappearing_date": null
            },
            {
                "id": 84875,
                "chat_id": 1489,
                "author_id": 503,
                "date": "2026-03-04T09:43:21+02:00",
                "text": "Hello, Anna! Here we will discuss the project.",
                "unread": false,
                "uuid": "db2e826a-dd18-4ab5-b76c-4084e106ee28",
                "replaces": [],
                "params": [],
                "disappearing_date": null
            },
            ...,
            {
                "id": 84869,
                "chat_id": 1489,
                "author_id": 0,
                "date": "2026-03-04T09:42:31+02:00",
                "text": "[USER=503 REPLACE]John Smith[/USER] invited to the chat [USER=547 REPLACE]Anna Johnson[/USER]",
                "unread": false,
                "uuid": null,
                "replaces": [],
                "params": {
                    "CODE": ["CHAT_JOIN"],
                    "NOTIFY": "N"
                },
                "disappearing_date": null
            },
            ...
        ],
        "users": [
            {
                "id": 503,
                "active": true,
                "name": "John Smith",
                "first_name": "John",
                "last_name": "Smith",
                "work_position": "admin",
                "color": "#4ba984",
                "avatar": "https://mysite.com/upload/resize_cache/main/avatar.jpg",
                "avatar_hr": "https://mysite.com/upload/resize_cache/main/avatar.jpg",
                "gender": "M",
                "birthday": "",
                "extranet": false,
                "network": false,
                "bot": false,
                "connector": false,
                "external_auth_id": "socservices",
                "status": "online",
                "idle": false,
                "last_activity_date": "2026-03-04T10:13:14+02:00",
                "mobile_last_date": false,
                "desktop_last_date": false,
                "absent": false,
                "departments": [667],
                "phones": false,
                "bot_data": null,
                "type": "user",
                "website": "",
                "email": "smith@mysite.com"
            },
            {
                "id": 547,
                "active": true,
                "name": "Anna Johnson",
                "first_name": "Anna",
                "last_name": "Johnson",
                ...
            }
        ],
        "files": [
            {
                "id": 5255,
                "chatId": 1489,
                "date": "2026-03-02T16:10:00+02:00",
                "type": "image",
                "name": "image.png",
                "extension": "png",
                "size": 2144,
                "image": {
                    "height": 61,
                    "width": 72
                },
                "status": "done",
                "progress": 100,
                "authorId": 503,
                "authorName": "John Smith",
                "urlPreview": "https://mysite.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=5255&exact=N&_esd=s6P3x5qDBEKU0NiS7sczr69Y%2FHHR8Za8EXa7STOAXIVOylYhMsnMj5nGU0VXeQ1PIsqm%2F0GNxOju5wR1jNj76d%2FZnVgpyqeIcJ4UiWXm8CJsrmARXWpxWe%2BgJ%2BpGqx0M5CxgjNzIopQp2cwM&fileName=image.png",
                "urlShow": "https://mysite.com/bitrix/services/main/ajax.php?action=disk.api.file.showImage&SITE_ID=s1&humanRE=1&fileId=5255&width=1280&height=1280&signature=4b6b2bbba680d3bccd8b70e398d94c1c3cfcb018f813089c32db3bb25df594f5&exact=N&_esd=s6P3x5qDBEKU0NiS7sczr69Y%2FHHR8Za8EXa7STOAXIVOylYhMsnMj5nGU0VXeQ1PIsqm%2F0GNxOju5wR1jNj76d%2FZnVgpyqeIcJ4UiWXm8CJsrmARXWpxWe%2BgJ%2BpGqx0M5CxgjNzIopQp2cwM&fileName=image.png",
                "urlDownload": "https://mysite.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=5255&exact=N&_esd=s6P3x5qDBEKU0NiS7sczr69Y%2FHHR8Za8EXa7STOAXIVOylYhMsnMj5nGU0VXeQ1PIsqm%2F0GNxOju5wR1jNj76d%2FZnVgpyqeIcJ4UiWXm8CJsrmARXWpxWe%2BgJ%2BpGqx0M5CxgjNzIopQp2cwM&fileName=image.png",
                "viewerAttrs": {
                    "viewer": "",
                    "viewerType": "image",
                    "src": "https://mysite.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=5255&exact=N&_esd=s6P3x5qDBEKU0NiS7sczr69Y%2FHHR8Za8EXa7STOAXIVOylYhMsnMj5nGU0VXeQ1PIsqm%2F0GNxOju5wR1jNj76d%2FZnVgpyqeIcJ4UiWXm8CJsrmARXWpxWe%2BgJ%2BpGqx0M5CxgjNzIopQp2cwM&fileName=image.png",
                    "viewerResized": "",
                    "objectId": "5255",
                    "viewerGroupBy": "1489",
                    "imChatId": 1489,
                    "title": "image.png",
                    "actions": "[{\"type\":\"download\"},{\"type\":\"copyToMe\",\"text\":\"Save to Drive\",\"action\":\"BXIM.disk.saveToDiskAction\",\"params\":{\"fileId\":\"5255\"},\"extension\":\"disk.viewer.actions\",\"buttonIconClass\":\"ui-btn-icon-cloud\"}]"
                },
                "mediaUrl": {
                    "preview": {
                        "250": "https://mysite.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=5255&exact=N&_esd=s6P3x5qDBEKU0NiS7sczr69Y%2FHHR8Za8EXa7STOAXIVOylYhMsnMj5nGU0VXeQ1PIsqm%2F0GNxOju5wR1jNj76d%2FZnVgpyqeIcJ4UiWXm8CJsrmARXWpxWe%2BgJ%2BpGqx0M5CxgjNzIopQp2cwM&fileName=image.png"
                    }
                },
                "isTranscribable": false,
                "isVideoNote": false,
                "isVoiceNote": false
            }
        ]
    },
    "time": {
        "start": 1772608704,
        "finish": 1772608704.545697,
        "duration": 0.5456969738006592,
        "processing": 0,
        "date_start": "2026-03-04T10:18:24+02:00",
        "date_finish": "2026-03-04T10:18:24+02:00",
        "operating_reset_at": 1772609304,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **chat_id**
[`integer`](../../data-types.md) | Identifier of the chat ||
|| **messages**
[`array`](../../data-types.md) | Array of messages [(detailed description)](#message).

The method will return an empty array if a non-existent identifier is specified in `LAST_ID` or `FIRST_ID`. ||
|| **users**
[`array`](../../data-types.md) | Users from the selection [(detailed description)](#user).

The method will return an empty array if a non-existent identifier is specified in `LAST_ID` or `FIRST_ID`. ||
|| **files**
[`array`](../../data-types.md) | Files from the selection [(detailed description)](#file).

The method will return an empty array if a non-existent identifier is specified in `LAST_ID` or `FIRST_ID`. ||
|#

#### Object message {#message}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the message ||
|| **chat_id**
[`integer`](../../data-types.md) | Identifier of the chat ||
|| **author_id**
[`integer`](../../data-types.md) | Identifier of the author, `0` for system messages ||
|| **date**
[`datetime`](../../data-types.md) | Date of the message in ISO 8601 format ||
|| **text**
[`string`](../../data-types.md) | Text of the message ||
|| **unread**
[`boolean`](../../data-types.md) | Indicator of unread message ||
|| **uuid**
[`string`](../../data-types.md) | Unique identifier of the message, `null` for system messages ||
|| **replaces**
[`array`](../../data-types.md) | Array of text replacements for the message ||
|| **params**
[`object`](../../data-types.md) | Additional parameters of the message [(detailed description)](#params).

The set of fields in the object depends on the type of message: regular or system. ||
|| **disappearing_date**
[`datetime`](../../data-types.md) | Date of disappearance of the message, `null` if not set ||
|#

#### Object params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **LIKE**
[`array`](../../data-types.md) | Identifiers of users who reacted to the message ||
|| **CODE**
[`array`](../../data-types.md) | Codes of system events:
- `CHAT_JOIN` — user added to the chat
- `CHAT_LEAVE` — user left the chat ||
|| **NOTIFY**
[`string`](../../data-types.md) | Indicator of notification sending. Value `N` — notification is not sent ||
|#

#### Object user {#user}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the user ||
|| **active**
[`boolean`](../../data-types.md) | Indicator of active user ||
|| **name**
[`string`](../../data-types.md) | Full name ||
|| **first_name**
[`string`](../../data-types.md) | First name ||
|| **last_name**
[`string`](../../data-types.md) | Last name ||
|| **work_position**
[`string`](../../data-types.md) | Job title ||
|| **color**
[`string`](../../data-types.md) | Avatar color in hex format ||
|| **avatar**
[`string`](../../data-types.md) | Link to the avatar ||
|| **avatar_hr**
[`string`](../../data-types.md) | Link to the high-resolution avatar ||
|| **gender**
[`string`](../../data-types.md) | Gender.
- `M` — male
- `F` — female ||
|| **birthday**
[`string`](../../data-types.md) | Date of birth ||
|| **extranet**
[`boolean`](../../data-types.md) | Indicator of extranet user ||
|| **network**
[`boolean`](../../data-types.md) | Indicator of Bitrix24 network user ||
|| **bot**
[`boolean`](../../data-types.md) | Indicator of bot ||
|| **connector**
[`boolean`](../../data-types.md) | Indicator of Open Lines user ||
|| **external_auth_id**
[`string`](../../data-types.md) | Type of authentication ||
|| **status**
[`string`](../../data-types.md) | User status ||
|| **idle**
[`boolean`](../../data-types.md) | Indicator of user inactivity ||
|| **last_activity_date**
[`datetime`](../../data-types.md) | Date of last activity ||
|| **mobile_last_date**
[`datetime`](../../data-types.md) | Date of last activity in the mobile app, `false` if the app was not used ||
|| **desktop_last_date**
[`datetime`](../../data-types.md) | Date of last activity in the desktop app, `false` if the app was not used ||
|| **absent**
[`boolean`](../../data-types.md) | Indicator of user absence ||
|| **departments**
[`array`](../../data-types.md) | Identifiers of user departments ||
|| **phones**
[`object`](../../data-types.md) | User phones, `false` if not specified ||
|| **bot_data**
[`object`](../../data-types.md) | Bot data, `null` for regular users ||
|| **type**
[`string`](../../data-types.md) | User type ||
|| **website**
[`string`](../../data-types.md) | User website ||
|| **email**
[`string`](../../data-types.md) | User e-mail ||
|#

#### Object file {#file}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the file ||
|| **chatId**
[`integer`](../../data-types.md) | Identifier of the chat ||
|| **date**
[`datetime`](../../data-types.md) | Date of file upload ||
|| **type**
[`string`](../../data-types.md) | Type of file: `image`, `video`, `audio`, `file` ||
|| **name**
[`string`](../../data-types.md) | Name of the file ||
|| **extension**
[`string`](../../data-types.md) | File extension ||
|| **size**
[`integer`](../../data-types.md) | Size in bytes ||
|| **image**
[`object`](../../data-types.md) | Dimensions of the image for files of type `image` [(detailed description)](#image) ||
|| **status**
[`string`](../../data-types.md) | Status of the file: `done` — uploaded ||
|| **progress**
[`integer`](../../data-types.md) | Percentage of file upload ||
|| **authorId**
[`integer`](../../data-types.md) | Identifier of the file author ||
|| **authorName**
[`string`](../../data-types.md) | Name of the file author ||
|| **urlPreview**
[`string`](../../data-types.md) | Link for file preview ||
|| **urlShow**
[`string`](../../data-types.md) | Link for displaying the file ||
|| **urlDownload**
[`string`](../../data-types.md) | Link for downloading the file ||
|| **viewerAttrs**
[`object`](../../data-types.md) | Attributes for file viewer [(detailed description)](#viewerAttrs) ||
|| **mediaUrl**
[`object`](../../data-types.md) | Media file URL for preview [(detailed description)](#mediaUrl) ||
|| **isTranscribable**
[`boolean`](../../data-types.md) | Indicator of transcribability ||
|| **isVideoNote**
[`boolean`](../../data-types.md) | Indicator of video note ||
|| **isVoiceNote**
[`boolean`](../../data-types.md) | Indicator of voice note ||
|#

#### Object image {#image}

#|
|| **Name**
`type` | **Description** ||
|| **height**
[`integer`](../../data-types.md) | Height of the image in pixels ||
|| **width**
[`integer`](../../data-types.md) | Width of the image in pixels ||
|#

#### Object viewerAttrs {#viewerAttrs}

#|
|| **Name**
`type` | **Description** ||
|| **viewer**
[`string`](../../data-types.md) | Type of viewer ||
|| **viewerType**
[`string`](../../data-types.md) | Type of file display ||
|| **src**
[`string`](../../data-types.md) | Link to the file ||
|| **viewerResized**
[`string`](../../data-types.md) | Link to the reduced version of the file ||
|| **objectId**
[`string`](../../data-types.md) | Identifier of the file object ||
|| **viewerGroupBy**
[`string`](../../data-types.md) | Identifier of the group for viewing files in the chat ||
|| **imChatId**
[`integer`](../../data-types.md) | Identifier of the chat ||
|| **title**
[`string`](../../data-types.md) | Name of the file ||
|| **actions**
[`string`](../../data-types.md) | Available actions with the file in JSON format ||
|#

#### Object mediaUrl {#mediaUrl}

#|
|| **Name**
`type` | **Description** ||
|| **preview**
[`object`](../../data-types.md) | Links for file preview. The keys of the object are the dimensions of the image in pixels ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `DIALOG_ID_EMPTY` | Dialog ID can't be empty | The `DIALOG_ID` parameter is not provided, is empty, or is in an incorrect format. ||
|| `400` | `FIRST_ID_STRING` | First ID can't be string | The `FIRST_ID` parameter is provided with a non-numeric value. ||
|| `400` | `LAST_ID_STRING` | Last ID can't be string | The `LAST_ID` parameter is provided with a non-numeric value. ||
|| `403` | `ACCESS_ERROR` | You do not have access to the specified dialog | The user does not have access to the dialog. ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-dialog-messages-search.md)
- [{#T}](./im-dialog-read.md)
- [{#T}](./im-dialog-unread.md)
- [{#T}](./im-dialog-writing.md)