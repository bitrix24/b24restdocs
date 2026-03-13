# Get Message History of the Dialogue imopenlines.session.history.get

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imopenlines.session.history.get` returns the message history of an open line session.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **SESSION_ID**
[`integer`](../../../data-types.md) | Identifier of the open line session.

The identifier can be obtained using the [imopenlines.dialog.get](./imopenlines-dialog-get.md) method from the `entity_data_1` field. The `SESSION_ID` identifier is located in the sixth parameter of the `entity_data_1` string. For example, for ```"entity_data_1":"Y|LEAD|1205|N|N|321|1773223732|0|0|0"```, the session identifier is `321` ||
|| **CHAT_ID**
[`integer`](../../../data-types.md) | Numeric identifier of the open line chat without the `chat` prefix. For example, `1763`, not `chat1763`.

The identifier can be obtained using the [imopenlines.dialog.get](./imopenlines-dialog-get.md) or [imopenlines.crm.chat.get](../chats/imopenlines-crm-chat-get.md) methods ||
|#

The method accepts one of the parameters: `SESSION_ID` or `CHAT_ID`.

- If `SESSION_ID` is provided, the method operates based on it.
- If `SESSION_ID` is not provided, the method attempts to determine the last session by `CHAT_ID` sorted by `ID DESC`.

In practice, for stable results, it is recommended to provide `SESSION_ID`.

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SESSION_ID":321}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.session.history.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SESSION_ID":321,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imopenlines.session.history.get
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'imopenlines.session.history.get',
            {
                SESSION_ID: 321,
            }
        );

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
                'imopenlines.session.history.get',
                [
                    'SESSION_ID' => 321,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error getting session history: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.session.history.get',
        {
            SESSION_ID: 321,
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
        'imopenlines.session.history.get',
        [
            'SESSION_ID' => 321,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Success: ' . print_r($result['result'], true);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "chatId": 1763,
        "canJoin": "Y",
        "canVoteHead": "Y",
        "sessionId": 321,
        "sessionVoteHead": 0,
        "sessionCommentHead": null,
        "userId": "chat1763",
        "message": {
            "85851": {
                "id": "85851",
                "chatid": "1763",
                "senderid": "103",
                "recipientid": "chat1763",
                "date": "2026-03-11T13:12:46+02:00",
                "text": "Open the task card and click Observers",
                "textlegacy": "Open the task card and click Observers",
                "params": {
                    "connectorMid": ["85853"],
                    "fileId": [5437]
                }
            },
            "85833": {
                "id": "85833",
                "chatid": "1763",
                "senderid": "0",
                "recipientid": "chat1763",
                "date": "2026-03-11T13:09:25+02:00",
                "text": "[b]A new lead has been created[/b]",
                "textlegacy": "<b>A new lead has been created</b>",
                "params": {
                    "attach": [
                        {
                            "id": 1773223765,
                            "blocks": [
                                {
                                    "link": [
                                        {
                                            "name": "Filling out the CRM form \"Contact Information Form for Open Lines\"",
                                            "link": "/crm/lead/details/1205/"
                                        }
                                    ]
                                },
                                {
                                    "grid": [
                                        {
                                            "display": "ROW",
                                            "name": "Name",
                                            "value": "John",
                                            "colorToken": "base"
                                        },
                                        {
                                            "display": "LINE",
                                            "name": "Phone",
                                            "value": "+11234567890",
                                            "height": 20,
                                            "colorToken": "base"
                                        }
                                    ]
                                }
                            ],
                            "description": "",
                            "color": "#df532d"
                        }
                    ]
                }
            },
            ... // description for each message
        },
        "usersMessage": {
            "chat1763": [
                "85851",
                "85833",
            ]
        },
        "users": {
            "103": {
                "id": "103",
                "name": "Samantha Johnson",
                "active": true,
                "firstName": "Samantha",
                "lastName": "Johnson",
                "workPosition": "IT Department Head",
                "color": "#4ba984",
                "avatar": "https://example.bitrix24.com/upload/main/avatar.png",
                "avatarId": "8644",
                "birthday": "08-03",
                "gender": "F",
                "phoneDevice": false,
                "phones": false,
                "extranet": false,
                "network": false,
                "bot": false,
                "connector": false,
                "profile": "/company/personal/user/103/",
                "externalAuthId": "socservices",
                "status": "online",
                "idle": false,
                "lastActivityDate": "2026-03-11T13:14:35+02:00",
                "mobileLastDate": false,
                "desktopLastDate": false,
                "departments": [1, 7],
                "absent": false,
                "type": "user",
                "services": null,
                "botData": null
            },
            ... // description for each user
        },
        "openlines": {
            "canvoteashead": {
                "22": true
            }
        },
        "userInGroup": {
            "1": {
                "id": 1,
                "users": ["103"]
            },
            ... // description for each department
        },
        "woUserInGroup": [],
        "chat": {
            "1763": {
                "id": "1763",
                "parentChatId": 0,
                "parentMessageId": 0,
                "name": "John - Bitrix24 Documentation",
                "owner": "103",
                "color": "#ba9c7b",
                "extranet": false,
                "avatar": "/bitrix/js/im/images/blank.gif",
                "call": "0",
                "callNumber": "",
                "entityType": "LINES",
                "entityId": "livechat|22|1761|587",
                "entityData1": "Y|LEAD|1205|N|N|321|1773223732|0|0|0",
                "entityData2": "LEAD|1205|COMPANY|0|CONTACT|0|DEAL|0",
                "entityData3": "",
                "messageCount": 14,
                "public": "",
                "muteList": {
                    "103": false,
                    "587": false
                },
                "managerList": [103],
                "dateCreate": "2026-03-11T12:08:52+02:00",
                "type": "lines",
                "entityLink": {
                    "type": "LINES",
                    "url": "",
                    "id": "livechat|22|1761|587"
                },
                "permissions": {
                    "manageUsersAdd": "member",
                    "manageUsersDelete": "manager",
                    "manageUi": "member",
                    "manageSettings": "owner",
                    "manageMessages": "member",
                    "canPost": "member"
                },
                "textFieldEnabled": true,
                "backgroundId": null,
                "messageType": "L",
                "isNew": false
            }
        },
        "userBlockChat": {
            "1763": {
                "103": false,
                "587": false
            }
        },
        "userInChat": {
            "1763": [103, 587]
        },
        "files": {
            "5437": {
                "id": 5437,
                "chatid": 1763,
                "date": "2026-03-11T13:12:46+02:00",
                "type": "image",
                "name": "2311.png",
                "extension": "png",
                "size": 70855,
                "image": {
                    "height": 615,
                    "width": 646
                },
                "status": "done",
                "progress": 100,
                "authorid": 103,
                "authorname": "Samantha Johnson",
                "urlpreview": "https://some-domain.bitrix24.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=5437&exact=N&_esd=oYQLNdHU%3D&fileName=2311.png",
                "urlshow": "https://some-domain.bitrix24.com/bitrix/services/main/ajax.php?action=disk.api.file.showImage&SITE_ID=s1&humanRE=1&fileId=5437&width=1280&height=1280&signature=d1007a9ed47599e2160b993ca&exact=N&_esd=oYQLNdHU%3D&fileName=2311.png",
                "urldownload": "https://some-domain.bitrix24.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=5437&exact=N&_esd=oYQLNdHU%3D&fileName=2311.png",
                "viewerattrs": {
                    "viewer": "",
                    "viewertype": "image",
                    "src": "https://some-domain.bitrix24.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=5437&exact=N&_esd=oYQLNdHU%3D&fileName=2311.png",
                    "viewerresized": "",
                    "objectid": "5437",
                    "viewergroupby": "1763",
                    "imchatid": 1763,
                    "title": "2311.png",
                    "actions": "[{\"type\":\"download\"},{\"type\":\"copyToMe\",\"text\":\"Save to Drive\",\"action\":\"BXIM.disk.saveToDiskAction\",\"params\":{\"fileId\":\"5437\"},\"extension\":\"disk.viewer.actions\",\"buttonIconClass\":\"ui-btn-icon-cloud\"}]"
                },
                "mediaurl": {
                    "preview": {
                        "250": "https://some-domain.bitrix24.com/bitrix/services/main/ajax.php?action=disk.api.file.showImage&SITE_ID=s1&humanRE=1&fileId=5437&width=250&height=250&signature=c28f11fbae0ee5a0c40970c4e8e554&exact=Y&_esd=oYQLNdHU%3D&fileName=2311.png"
                    }
                },
                "istranscribable": false,
                "isvideonote": false,
                "isvoicenote": false
            }
        }
    },
    "time": {
        "start": 1773224138,
        "finish": 1773224138.143344,
        "duration": 0.14334392547607422,
        "processing": 0,
        "date_start": "2026-03-11T13:15:38+02:00",
        "date_finish": "2026-03-11T13:15:38+02:00",
        "operating_reset_at": 1773224738,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root object of the response.

The structure of the object is described in detail [below](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

### Result Object {#result}

#|
|| **Name**
`Type` | **Description** ||
|| **chatId**
[`integer`](../../../data-types.md) | Identifier of the chat ||
|| **canJoin**
[`string`](../../../data-types.md) | Flag indicating the ability to join the dialogue: `Y` or `N` ||
|| **canVoteHead**
[`string`](../../../data-types.md) | Flag indicating the ability to evaluate operators by the supervisor: `Y` or `N` ||
|| **sessionId**
[`integer`](../../../data-types.md) | Identifier of the session ||
|| **sessionVoteHead**
[`integer`](../../../data-types.md) | Rating given by the supervisor ||
|| **sessionCommentHead**
[`string`](../../../data-types.md) | Comment from the supervisor on the rating or `null` ||
|| **userId**
[`string`](../../../data-types.md) | Identifier of the chat in the format `chat<ID>` ||
|| **message**
[`object`](../../../data-types.md) | Chat messages indexed by message identifier.

The structure of the object is described in detail [below](#message) ||
|| **usersMessage**
[`object`](../../../data-types.md) | Relationship between the chat and the list of message identifiers.

The structure of the object is described in detail [below](#users-message) ||
|| **users**
[`object`](../../../data-types.md) | Object of chat participants, where the key is the user identifier.

The structure of the object is described in detail [below](#users) ||
|| **openlines**
[`object`](../../../data-types.md) | Open line data.

The structure of the object is described in detail [below](#openlines) ||
|| **userInGroup**
[`object`](../../../data-types.md) | Users grouped by departments, or an empty array 

The structure of the object is described in detail [below](#user-in-group) ||
|| **woUserInGroup**
[`array`](../../../data-types.md) | Users not included in the departments from `userInGroup` ||
|| **chat**
[`object`](../../../data-types.md) | Chat data by its identifier.

The structure of the object is described in detail [below](#chat) ||
|| **userBlockChat**
[`object`](../../../data-types.md) | Flags for blocking the chat by users.

The structure of the object is described in detail [below](#user-block-chat) ||
|| **userInChat**
[`object`](../../../data-types.md) | Composition of chat participants or an empty array. 

The structure of the chat participants is described in detail [below](#user-in-chat) ||
|| **files**
[`object`](../../../data-types.md) | Files associated with messages, or an empty array.

The structure of the files object is described in detail [below](#files) ||
|#

### Message Object {#message}

#|
|| **Name**
`Type` | **Description** ||
|| **<messageId>**
[`object`](../../../data-types.md) | Message object.

The structure of the message object is described in detail [below](#message-item) ||
|#

### Message Item Object {#message-item}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Identifier of the message ||
|| **chatid**
[`string`](../../../data-types.md) | Identifier of the chat ||
|| **senderid**
[`string`](../../../data-types.md) | Identifier of the sender ||
|| **recipientid**
[`string`](../../../data-types.md) | Recipient of the message in the format `chat<ID>` ||
|| **date**
[`datetime`](../../../data-types.md) | Date and time of the message in ISO 8601 format (RFC3339) ||
|| **text**
[`string`](../../../data-types.md) | Text of the message ||
|| **textlegacy**
[`string`](../../../data-types.md) | Text of the message in legacy format ||
|| **params**
[`object`](../../../data-types.md) | Service parameters of the message ||
|#

### Users Message Object {#users-message}

#|
|| **Name**
`Type` | **Description** ||
|| **chat\<chatId\>**
[`array`](../../../data-types.md) | Array of message identifiers related to the chat ||
|#

### Users Object {#users}

#|
|| **Name**
`Type` | **Description** ||
|| **\<userId\>**
[`object`](../../../data-types.md) | User data.

The structure of the user object is described in detail [below](#user-item) ||
|#

### User Item Object {#user-item}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Identifier of the user ||
|| **name**
[`string`](../../../data-types.md) | Full name of the user ||
|| **firstName**
[`string`](../../../data-types.md) | First name of the user ||
|| **lastName**
[`string`](../../../data-types.md) | Last name of the user ||
|| **workPosition**
[`string`](../../../data-types.md) | Position or `null` ||
|| **avatar**
[`string`](../../../data-types.md) | Link to the user's avatar ||
|| **avatarId**
[`integer`](../../../data-types.md) | Identifier of the avatar file or `null` ||
|| **gender**
[`string`](../../../data-types.md) | Gender of the user ||
|| **extranet**
[`boolean`](../../../data-types.md) | Extranet user flag ||
|| **connector**
[`boolean`](../../../data-types.md) | Connector user flag ||
|| **profile**
[`string`](../../../data-types.md) | Link to the profile ||
|| **externalAuthId**
[`string`](../../../data-types.md) | External authentication type ||
|| **status**
[`string`](../../../data-types.md) | Online status or `null` ||
|| **lastActivityDate**
[`datetime`](../../../data-types.md) | Date and time of the last activity in ISO 8601 format (RFC3339) ||
|| **departments**
[`array`](../../../data-types.md) | Identifiers of departments ||
|| **type**
[`string`](../../../data-types.md) | Type of user ||
|#

### Open Lines Object {#openlines}

#|
|| **Name**
`Type` | **Description** ||
|| **canvoteashead**
[`object`](../../../data-types.md) | Object of the form `{"<CONFIG_ID>": <boolean>}`, where the key is the identifier of the open line settings `CONFIG_ID`, and the value `true/false` indicates whether the supervisor can evaluate the operator's work in the session of this line ||
|#

### User In Group Object {#user-in-group}

#|
|| **Name**
`Type` | **Description** ||
|| **\<departmentId\>**
[`object`](../../../data-types.md) | Department data.

The structure of the department object is described in detail [below](#group-item) ||
|#

### Group Item Object {#group-item}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the department ||
|| **users**
[`array`](../../../data-types.md) | List of user identifiers that belong to the department ||
|#

### Chat Object {#chat}

#|
|| **Name**
`Type` | **Description** ||
|| **\<chatId\>**
[`object`](../../../data-types.md) | Chat data.

The structure of the department object is described in detail [below](#chat-item) ||
|#

### Chat Item Object {#chat-item}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Identifier of the chat ||
|| **name**
[`string`](../../../data-types.md) | Name of the chat ||
|| **owner**
[`string`](../../../data-types.md) | Identifier of the chat owner ||
|| **entityType**
[`string`](../../../data-types.md) | Type of the chat object ||
|| **entityId**
[`string`](../../../data-types.md) | External identifier of the chat ||
|| **entityData1**
[`string`](../../../data-types.md) | String with chat metadata ||
|| **entityData2**
[`string`](../../../data-types.md) | Additional chat metadata ||
|| **entityData3**
[`string`](../../../data-types.md) | Additional chat metadata ||
|| **messageCount**
[`integer`](../../../data-types.md) | Number of messages in the chat ||
|| **public**
[`string`](../../../data-types.md) | Public flag of the chat ||
|| **muteList**
[`object`](../../../data-types.md) | Object where the key is the user identifier, and the value `true/false` indicates whether notifications are turned off for this user in the chat ||
|| **managerList**
[`array`](../../../data-types.md) | List of identifiers of operator-managers ||
|| **dateCreate**
[`datetime`](../../../data-types.md) | Date and time of chat creation in ISO 8601 format (RFC3339) ||
|| **type**
[`string`](../../../data-types.md) | Type of chat ||
|| **entityLink**
[`object`](../../../data-types.md) | Data linking the chat to the open line ||
|| **permissions**
[`object`](../../../data-types.md) | Access permissions in the chat ||
|| **textFieldEnabled**
[`boolean`](../../../data-types.md) | Flag indicating the availability of the message input field ||
|| **backgroundId**
[`integer`](../../../data-types.md) | Identifier of the chat background or `null` ||
|| **messageType**
[`string`](../../../data-types.md) | Type of chat messages ||
|| **isNew**
[`boolean`](../../../data-types.md) | Flag indicating a new chat ||
|#

### User Block Chat Object {#user-block-chat}

#|
|| **Name**
`Type` | **Description** ||
|| **\<chatId\>**
[`object`](../../../data-types.md) | Object with flags for blocking users in the chat: key — user identifier, value — `true` or `false` || ||
|#

### User In Chat Object {#user-in-chat}

#|
|| **Name**
`Type` | **Description** ||
|| **\<chatId\>**
[`array`](../../../data-types.md) | Array of user identifiers that are in the chat ||
|#

### Files Object {#files}

#|
|| **Name**
`Type` | **Description** ||
|| **\<fileId\>**
[`object`](../../../data-types.md) | File data. 

The structure of the object is described in detail [below](#file-item) ||
|#

### File Item Object {#file-item}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the file ||
|| **chatid**
[`integer`](../../../data-types.md) | Identifier of the chat ||
|| **date**
[`datetime`](../../../data-types.md) | Date and time of upload in ISO 8601 format (RFC3339) ||
|| **type**
[`string`](../../../data-types.md) | Type of file ||
|| **name**
[`string`](../../../data-types.md) | Name of the file ||
|| **extension**
[`string`](../../../data-types.md) | File extension ||
|| **size**
[`integer`](../../../data-types.md) | Size of the file in bytes ||
|| **image**
[`object`](../../../data-types.md) | Image parameters (`width`, `height`) ||
|| **status**
[`string`](../../../data-types.md) | Status of file processing ||
|| **progress**
[`integer`](../../../data-types.md) | Processing progress in percentage ||
|| **authorid**
[`integer`](../../../data-types.md) | Identifier of the file author ||
|| **authorname**
[`string`](../../../data-types.md) | Name of the file author ||
|| **urlpreview**
[`string`](../../../data-types.md) | URL of the file preview ||
|| **urlshow**
[`string`](../../../data-types.md) | URL of the file view ||
|| **urldownload**
[`string`](../../../data-types.md) | URL of the file download ||
|| **viewerattrs**
[`object`](../../../data-types.md) | Viewing parameters of the file in the interface ||
|| **mediaurl**
[`string`](../../../data-types.md) | URL of the media file ||
|| **istranscribable**
[`boolean`](../../../data-types.md) | Flag indicating the availability of file transcription ||
|| **isvideonote**
[`boolean`](../../../data-types.md) | Flag indicating a video note ||
|| **isvoicenote**
[`boolean`](../../../data-types.md) | Flag indicating a voice note ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_REQUIRED_PARAM",
    "error_description": "Session ID or Chat ID must be provided"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `MISSING_REQUIRED_PARAM` | Session ID or Chat ID must be provided | Neither `SESSION_ID` nor `CHAT_ID` was provided ||
|| `INVALID_SESSION_ID` | Unable to determine session ID from provided parameters | Unable to determine the session from the provided parameters ||
|| `ACCESS_DENIED` | You cannot open this conversation as you do not have sufficient permissions | Insufficient permissions to view the history, session not found or unavailable ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-crm-lead-create.md)
- [{#T}](./imopenlines-dialog-get.md)
- [{#T}](./imopenlines-message-session-start.md)
- [{#T}](./imopenlines-session-head-vote.md)
- [{#T}](./imopenlines-session-intercept.md)
- [{#T}](./imopenlines-session-join.md)
- [{#T}](./imopenlines-session-mode-pin-all.md)
- [{#T}](./imopenlines-session-mode-pin.md)
- [{#T}](./imopenlines-session-mode-silent.md)
- [{#T}](./imopenlines-session-mode-unpin-all.md)
- [{#T}](./imopenlines-session-open.md)
- [{#T}](./imopenlines-session-start.md)