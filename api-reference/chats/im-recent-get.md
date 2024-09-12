# Get a Shortened List of Recent Chats im.recent.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.recent.get` retrieves a list of the user's recent conversations.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **SKIP_OPENLINES**
[`unknown`](../data-types.md) | `N` | Skip open line chats | 18 ||
|| **SKIP_CHAT**
[`unknown`](../data-types.md) | `N` | Skip chats | 18 ||
|| **SKIP_DIALOG**
[`unknown`](../data-types.md) | `N` | Skip one-on-one dialogs | 18 ||
|| **LAST_UPDATE**
[`unknown`](../data-types.md) | `2019-07-11T10:45:31+02:00` | Sampling limit to minimize transmitted data, date in ATOM format | 23 ||
|| **ONLY_OPENLINES**
[`unknown`](../data-types.md) | `N` | Sample only open line chats | 29 ||
|| **LAST_SYNC_DATE**
[`unknown`](../data-types.md) | `2019-07-11T10:45:31+02:00` | Date of the previous sample for loading changes that occurred in the list since that time. The sample returns data no older than 7 days. Date in ATOM format | 29 ||
|#

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.recent.get',
        {
            'SKIP_OPENLINES': 'Y'
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

- PHP

    {% include [Explanation about restCommand](./_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.user.status.idle.start',
        Array(
            'SKIP_OPENLINES' => 'Y'
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}

## Response in Case of Success

```json
{
    "result": [
        {
            "id": "1",
             "type": "user",
             "avatar": {
                "url": "http://www.hazz/upload/resize_cache/main/1af/100_100_2/1464255149.png",
                "color": "#df532d"
             },
            "title": "Eugene Shelenkov",
            "message": {
                "id": "30468",
                "text": "1",
                "file": false,
                "attach": false,
                "author_id": "1"
            },
            "counter": "3",
            "date": "2017-10-17T11:12:56+02:00",
            "user": {
                "id": "1",
                "name": "Eugene Shelenkov",
                "first_name": "Eugene",
                "last_name": "Shelenkov",
                "work_position": "IT Specialist",
                "color": "#df532d",
                "avatar": "http://www.hazz/upload/resize_cache/main/1af/100_100_2/1464255149.png",
                "gender": "M",
                "birthday": false,
                "extranet": false,
                "network": false,
                "bot": false,
                "connector": false,
                "external_auth_id": "default",
                "status": "online",
                "idle": false,
                "last_activity_date": "2017-10-17T11:16:01+02:00",
                "mobile_last_date": "2017-05-26T12:04:58+02:00",
                "absent": "2017-11-01T00:00:00+02:00"
            }
        },
        {
            "id": "chat21191",
            "type": "chat",
            "avatar": {
                "url": "",
                "color": "#4ba984"
            },
            "title": "Mint Chat #3",
            "message": {
                "id": "30467",
                "text": "Permission to update Bitrix24 received from [Attachment]",
                "file": false,
                "attach": true,
                "author_id": "2"
            },
            "counter": "0",
            "date": "2017-10-17T10:38:20+02:00",
            "chat": {
                "id": "21191",
                "title": "Mint Chat #3",
                "owner": "2",
                "extranet": false,
                "avatar": "",
                "color": "#4ba984",
                "type": "chat",
                "entity_type": "",
                "entity_data_1": "",
                "entity_data_2": "",
                "entity_data_3": "",
                "date_create": "2017-10-14T12:15:32+02:00",
                "message_type": "C"
            }
        }
    ]
}
```

### Description of Keys

- `id` – identifier of the dialog (number if user; chatXXX if it's a chat)
- `type` – type of record (`user` – if user, `chat` – if it's a chat)
- `avatar` – object describing the avatar of the record:
  - `url` – link to the avatar (if empty, then the avatar is not set)
  - `color` – color of the dialog in hex format
- `title` – title of the record (First name, last name – for user, chat name – for chat)
- `message` – object describing the message:
  - `id` – identifier of the message
  - `text` – text of the message (without BB codes and line breaks)
  - `file` – presence of files (`true/false`)
  - `attach` – presence of attachments (`true/false`)
  - `author_id` – author of the message
  - `date` – date of the message in ATOM format
- `counter` – counter of unread messages
- `user` – object describing user data (not available if the record type is chat):
  - `id` – user identifier
  - `name` – user's full name
  - `first_name` – user's first name
  - `last_name` – user's last name
  - `work_position` – position
  - `color` – user's color in hex format
  - `avatar` – link to the avatar (if empty, then the avatar is not set)
  - `gender` – user's gender
  - `birthday` – user's birthday in DD-MM format, if empty – not set
  - `extranet` – indicator of external extranet user (`true/false`)
  - `network` – indicator of Bitrix24.Network user (`true/false`)
  - `bot` – indicator of bot (`true/false`)
  - `connector` – indicator of open line user (`true/false`)
  - `external_auth_id` – external authorization code
  - `status` – selected status of the user
  - `idle` – date when the user stepped away from the computer, in ATOM format (if not set, `false`)
  - `last_activity_date` – date of the user's last action in ATOM format
  - `mobile_last_date` – date of the last action in the mobile app in ATOM format (if not set, `false`)
  - `absent` – date until which the user is on vacation, in ATOM format (if not set, `false`)
- `chat` – object describing chat data (not available if the record type is user):
  - `id` – chat identifier
  - `title` – chat name
  - `owner` – identifier of the user who owns the chat
  - `extranet` – indicator of participation in the chat of an external extranet user (`true/false`)
  - `color` – color of the chat in hex format
  - `avatar` – link to the avatar (if empty, then the avatar is not set)
  - `type` – type of chat (group chat, call chat, open line chat, etc.)
  - `entity_type` – external code for the chat – type
  - `entity_id` – external code for the chat – identifier
  - `entity_data_1` – external data for the chat
  - `entity_data_2` – external data for the chat
  - `entity_data_3` – external data for the chat
  - `date_create` – date of chat creation in ATOM format
  - `message_type` – type of chat messages