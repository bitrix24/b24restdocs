# Create Chat im.chat.add

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.add` creates a new chat.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **USERS**
[`array`](../data-types.md) | An array of user IDs to be added to the chat.

The chat Creator is automatically added to the chat as the Owner ||
|| **TYPE**
[`string`](../data-types.md) | Type of chat: 
- `OPEN` — open chat
- `CHAT` — closed chat

By default, a closed chat `CHAT` is created ||
|| **TITLE**
[`string`](../data-types.md) | Title of the chat. 

If the parameter is not provided, the title will be automatically generated using the template `#COLOR# chat №#NUMBER#` or `Chat with #USERS_NAMES#` ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description of the chat ||
|| **COLOR**
[`string`](../data-types.md) | Color of the chat. Possible values:
- `RED` — red
- `GREEN` — green
- `MINT` — mint
- `LIGHT_BLUE` — light blue
- `DARK_BLUE` — dark blue
- `PURPLE` — purple
- `AQUA` — aqua
- `PINK` — pink
- `LIME` — lime
- `BROWN` — brown
- `AZURE` — azure
- `KHAKI` — khaki
- `SAND` — sand
- `MARENGO` — marengo
- `GRAY` — gray
- `GRAPHITE` — graphite ||
|| **MESSAGE**
[`string`](../data-types.md) | The first message in the chat ||
|| **AVATAR**
[`string`](../data-types.md) | Avatar of the chat in base64 string format.

Maximum image size is 5000x5000.

{% note tip "Typical use-cases and scenarios" %}

- [How to upload files](../files/how-to-upload-files.md)

{% endnote %}

||
|| **ENTITY_TYPE**
[`string`](../data-types.md) | Type of object to link the chat with external context.

Possible values:
- `VIDEOCONF` — video conference chat
- `AI_ASSISTANT_PRIVATE` — private chat with AI assistant
- `LINES` — open line chat from the operator's side
- `LIVECHAT` — open line chat from the client's side
- `ANNOUNCEMENT` — announcement chat
- `CALENDAR` — chat related to a calendar event
- `MAIL` — chat related to email correspondence
- `CRM` — chat related to a CRM entity
- `SONET_GROUP` — social network group chat
- `TASKS` — chat related to a task
- `CALL` — chat related to a call

||
|| **ENTITY_ID**
[`string`](../data-types.md) | Identifier of the object within `ENTITY_TYPE`.

Passed as a string. The format depends on the selected `ENTITY_TYPE`.

Supported formats for common types:
- `CRM` — `<CRM_TYPE>`\|`<ID>`, for example `LEAD`\|`13`, `DEAL`\|`1663`, `CONTACT`\|`25`, `COMPANY`\|`7`. For smart processes — `DYNAMIC_<entityTypeId>`\|`<itemId>`
- `LINES` — `<connectorId>`\|`<lineId>`\|`<connectorChatId>`\|`<connectorUserId>`, for example `telegrambot`\|`2`\|`209607941`\|`744`
- `LIVECHAT` — `<connectorId>`\|`<lineId>`
- `TASKS` — task identifier, for example `8293`
- `CALENDAR` — calendar event identifier
- `SONET_GROUP` — group identifier

For other `ENTITY_TYPE`, the format is defined by the module or integration. It can be an arbitrary string.

When creating a chat, you can pass any pair of `ENTITY_TYPE` and `ENTITY_ID`. The method [get chat identifier](./im-chat-get.md) will return the chat if called with the same pair of values
||
|| **COPILOT_MAIN_ROLE**
[`string`](../data-types.md) | Code of the main role for BitrixGPT.

Possible values:
- `copilot_assistant` — universal default role
- any code of an available BitrixGPT role from the AI library ||
|#

## Code Examples

{% include [Example Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USERS":[103, 547],"TYPE":"CHAT","TITLE":"Deal Chat","DESCRIPTION":"Discussing the deal here","COLOR":"PINK","MESSAGE":"Welcome to the deal chat","ENTITY_TYPE":"CRM","ENTITY_ID":"DEAL|1663"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.chat.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USERS":[103, 547],"TYPE":"CHAT","TITLE":"Deal Chat","DESCRIPTION":"Discussing the deal here","COLOR":"PINK","MESSAGE":"Welcome to the deal chat","ENTITY_TYPE":"CRM","ENTITY_ID":"DEAL|1663","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.chat.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.chat.add',
            {
                USERS: [103, 547],
                TYPE: 'CHAT',
                TITLE: 'Deal Chat',
                DESCRIPTION: 'Discussing the deal here',
                COLOR: 'PINK',
                MESSAGE: 'Welcome to the deal chat',
                ENTITY_TYPE: 'CRM',
                ENTITY_ID: 'DEAL|1663'
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
        $response = $b24Service
            ->core
            ->call(
                'im.chat.add',
                [
                    'USERS' => [103, 547],
                    'TYPE' => 'CHAT',
                    'TITLE' => 'Deal Chat',
                    'DESCRIPTION' => 'Discussing the deal here',
                    'COLOR' => 'PINK',
                    'MESSAGE' => 'Welcome to the deal chat',
                    'ENTITY_TYPE' => 'CRM',
                    'ENTITY_ID' => 'DEAL|1663',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'CHAT_ID: ' . $result;
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.chat.add',
        {
            USERS: [103, 547],
            TYPE: 'CHAT',
            TITLE: 'Deal Chat',
            DESCRIPTION: 'Discussing the deal here',
            COLOR: 'PINK',
            MESSAGE: 'Welcome to the deal chat',
            ENTITY_TYPE: 'CRM',
            ENTITY_ID: 'DEAL|1663',
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
        'im.chat.add',
        [
            'USERS' => [103, 547],
            'TYPE' => 'CHAT',
            'TITLE' => 'Deal Chat',
            'DESCRIPTION' => 'Discussing the deal here',
            'COLOR' => 'PINK',
            'MESSAGE' => 'Welcome to the deal chat',
            'ENTITY_TYPE' => 'CRM',
            'ENTITY_ID' => 'DEAL|1663',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 1417,
    "time": {
        "start": 1772009915,
        "finish": 1772009915.872788,
        "duration": 0.8727879524230957,
        "processing": 0,
        "date_start": "2026-02-25T11:58:35+01:00",
        "date_finish": "2026-02-25T11:58:35+01:00",
        "operating_reset_at": 1772010515,
        "operating": 0.20950984954833984
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Identifier of the created chat ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../_includes/error-info.md) %}

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-chat-get.md)
- [{#T}](./im-dialog-get.md)
- [{#T}](./im-recent-get.md)
- [{#T}](./im-recent-list.md)