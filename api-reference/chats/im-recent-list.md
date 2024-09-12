# Get the list of chats im.recent.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

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

The method `im.recent.list` retrieves a list of the user's recent conversations (with pagination support).

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **SKIP_OPENLINES**
[`unknown`](../data-types.md) | `'Y'` | Skip open line chats | 30 ||
|| **SKIP_DIALOG**
[`unknown`](../data-types.md) | `'N'` | Skip one-on-one dialogs | 30 ||
|| **SKIP_CHAT**
[`unknown`](../data-types.md) | `'Y'` | Skip chats | 30 ||
|| **LAST_MESSAGE_DATE**
[`unknown`](../data-types.md) | `'2021-10-30'` | Date from the last element of the previous selection | 30 ||
|#

## Examples

```js
B24.callMethod(
    'im.recent.list',
    {
        LAST_MESSAGE_DATE: '2021-10-30'
    },
    res => {
        if (res.error())
        {
        console.error(result.error().ex);
        }
        else
        {
        console.log(res.data())
        }
    }
)
```

{% include [Footnote on examples](../../_includes/examples.md) %}

## Response in case of success

```json
{
"items": [
    {
     "id": "chat71",
     "chat_id": 71,
     "type": "chat",
     "avatar": {
        "url": "",
        "color": "#8474c8"
     },
     "title": "Purple Guest #3 - Open Line 2",
     "message": {
        "id": 267,
        "text": "Form \"Contact Information Form for Open Lines\" sent [Attachment]",
        "file": false,
        "author_id": 0,
        "attach": true,
        "date": "2021-10-27T18:20:45+02:00",
        "status": "received"
     },
     "counter": 3,
     "pinned": false,
     "unread": false,
     "date_update": "2021-10-27T18:20:45+02:00",
     "chat": {
        "id": 71,
        "name": "Purple Guest #3 - Open Line 2",
        "owner": 0,
        "extranet": false,
        "avatar": "",
        "color": "#8474c8",
        "type": "lines",
        "entity_type": "LINES",
        "entity_id": "livechat|2|70|2016",
        "entity_data_1": "N|NONE|0|N|N|7|1635351612|0|0|0",
        "entity_data_2": "",
        "entity_data_3": "",
        "mute_list": [],
        "manager_list": [],
        "date_create": "2021-10-27T18:20:12+02:00",
        "message_type": "L"
     },
     "lines": {
        "id": 7,
        "status": 5,
        "date_create": "2021-10-27T18:20:12+02:00"
     },
     "user": {
        "id": 0
     },
     "options": []
    },
    ...
],
"hasMore": false
}
```

### Description of keys

- `id` – identifier of the dialog (if a number – it's a user; if **chatXXX** – it's a chat)
- `type` – type of the record (if **user** – it's a user, if **chat** – it's a chat)
- `avatar` – object describing the avatar of the record:
  - `url` – link to the avatar (if empty, the avatar is not set)
  - `color` – color of the dialog in **hex** format
- `title` – title of the record (first and last name for a user, chat name for a chat)
- `messages` – object describing the message:
  - `id` – identifier of the message
  - `text` – text of the message (without BB codes and line breaks)
  - `file` – are there files (**true/false**)
  - `attach` – are there attachments (**true/false**)
  - `author_id` – author of the message
  - `date` – date of the message
- `counter` – counter of unread messages
- `user` – object describing user data (not available if the record type is chat):
  - `id` – identifier of the user
  - `name` – first and last name of the user
  - `first_name` – first name of the user
  - `last_name` – last name of the user
  - `work_position` – position
  - `color` – color of the user in **hex** format
  - `avatar` – link to the avatar (if empty, the avatar is not set)
  - `gender` – gender of the user
  - `birthday` – birthday of the user in **DD-MM** format, if empty – not set
  - `extranet` – indicator of external extranet user (**true/false**)
  - `network` – indicator of **Bitrix24.Network** user (**true/false**)
  - `bot` – indicator of a bot (**true/false**)
  - `connector` – indicator of an open lines user (**true/false**)
  - `external_auth_id` – external authorization code
  - `status` – selected status of the user
  - `idle` – date when the user stepped away from the computer, in **ATOM** format (if not set, then **false**)
  - `last_activity_date` – date of the user's last action in **ATOM** format
  - `mobile_last_date` – date of the last action in the mobile app in **ATOM** format (if not set, then **false**)
  - `absent` – date until which the user is on vacation, in **ATOM** format (if not set, then **false**)
- `chat` – object describing chat data (not available if the record type is user):
  - `id` – identifier of the chat
  - `title` – name of the chat
  - `owner` – identifier of the user who owns the chat
  - `extranet` – indicator of participation in the chat of an external extranet user (**true/false**)
  - `color` – color of the chat in **hex** format
  - `avatar` – link to the avatar (if empty, the avatar is not set)
  - `type` – type of chat (group chat, call chat, open line chat, etc.)
  - `entity_type` – external code for the chat – type
  - `entity_id` – external code for the chat – identifier
  - `entity_data_1` – external data for the chat
  - `entity_data_2` – external data for the chat
  - `entity_data_3` – external data for the chat
  - `date_create` – date of chat creation in **ATOM** format
  - `message_type` – type of chat messages
- `pinned` – whether the chat is pinned or not
- `unread` – whether there is a manual mark that the chat is unread
- `date_update` – date of the last change in related entities