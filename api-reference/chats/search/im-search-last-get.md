# Get Search History im.search.last.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.last.get` retrieves a list of items from the last search.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **SKIP_OPENLINES**
[`unknown`](../../data-types.md) | `N` | Skip open line chats | 18 ||
|| **SKIP_CHAT**
[`unknown`](../../data-types.md) | `N` | Skip chats | 18 ||
|| **SKIP_DIALOG**
[`unknown`](../../data-types.md) | `N` | Skip one-on-one dialogs | 18 ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'im.search.last.get',
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error('Error:', error.ex);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.search.last.get',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error()->ex);
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting last search: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.search.last.get',
        {},
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
        'im.user.business.list',
        Array(),
        $_REQUEST[
            "auth"
        ]
    );    
    ```

- cURL

    // example for cURL

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response on Success

```json
{    
    "result": [
        {
            "id": 1,
            "type": "user",
            "title": "Eugene Shelenkov",
            "avatar": {
                "url": "http://192.168.2.232/upload/resize_cache/main/1d3/100_100_2/shelenkov.png",
                "color": "#df532d"
            },
            "user": {
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
                "last_activity_date": "2018-01-29T17:35:31+01:00",
                "desktop_last_date": false,
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
            }
        }
    ]
}            
```

### Description of Keys

- `id` – identifier of the dialog (number if user, chatXXX if it is a chat)
- `name` – type of record (`user` – if user, `chat` – if it is a chat)
- `avatar` – object describing the avatar of the record:
  - `url` – link to the avatar (if empty, the avatar is not set)
  - `color` – color of the dialog in hex format
- `title` – title of the record
- `user` – object describing user data (not available if the record type is chat):
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
  - `connector` – indicator of open line user (`true/false`)
  - `external_auth_id` – external authorization code
  - `status` – selected user status
  - `idle` – date when the user stepped away from the computer, in ATOM format (if not set, `false`)
  - `last_activity_date` – date of the user's last action in ATOM format
  - `mobile_last_date` – date of the last action in the mobile app in ATOM format (if not set, `false`)
  - `absent` – date until which the user is on vacation, in ATOM format (if not set, `false`)
- `chat` – object describing chat data (not available if the record type is user):
  - `id` – chat identifier
  - `title` – chat name
  - `owner` – identifier of the user who owns the chat
  - `extranet` – indicator of participation in the chat by an external extranet user (`true/false`)
  - `color` – chat color in hex format
  - `avatar` – link to the avatar (if empty, the avatar is not set)
  - `type` – type of chat (group chat, call chat, open line chat, etc.)
  - `entity_type` – external code for the chat – type
  - `entity_id` – external code for the chat – identifier
  - `entity_data_1` – external data for the chat
  - `entity_data_2` – external data for the chat
  - `entity_data_3` – external data for the chat
  - `date_create` – date of chat creation in ATOM format
  - `message_type` – type of chat messages