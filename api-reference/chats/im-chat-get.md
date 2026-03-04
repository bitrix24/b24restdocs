# Get Chat ID im.chat.get

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.get` retrieves the chat ID.

## Method Parameters

{% include [Footnote on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE***
[`string`](../data-types.md) | The type of object for linking the chat to an external context. Passed as a string.

Possible values:
- `VIDEOCONF` — video conference chat
- `AI_ASSISTANT_PRIVATE` — private chat with an AI assistant
- `LINES` — open line chat from the operator's side
- `LIVECHAT` — open line chat from the client's side
- `ANNOUNCEMENT` — announcement chat
- `CALENDAR` — chat related to a calendar event
- `MAIL` — chat related to email correspondence
- `CRM` — system chat "for discussion" of a CRM entity. The method will not return IDs of other chats related to the CRM entity
- `SONET_GROUP` — social network group chat
- `TASKS_TASK` — task chat in the [new task card](../tasks/tasks-new.md)
- `TASKS` — system task chat in the old task card
- `CALL` — chat related to a call
||
|| **ENTITY_ID***
[`string`](../data-types.md) | Identifier of the object within `ENTITY_TYPE`.

Passed as a string. The format depends on the selected `ENTITY_TYPE`.

Supported formats for common types:
- `CRM` — `<CRM_TYPE>`\|`<ID>`, for example `LEAD`\|`13`, `DEAL`\|`1663`, `CONTACT`\|`25`, `COMPANY`\|`7`. For smart processes — `DYNAMIC_<entityTypeId>`\|`<itemId>`
- `LINES` — `<connectorId>`\|`<lineId>`\|`<connectorChatId>`\|`<connectorUserId>`, for example `telegrambot`\|`2`\|`209607941`\|`744`
- `LIVECHAT` — `<connectorId>`\|`<lineId>`
- `TASKS`, `TASKS_TASK` — task identifier, for example `8293`
- `CALENDAR` — calendar event identifier
- `SONET_GROUP` — group identifier

For other `ENTITY_TYPE`, the format is defined by the module or integration. It can be an arbitrary string.

When [creating a chat](./im-chat-add.md), you can pass an arbitrary pair of `ENTITY_TYPE` and `ENTITY_ID`. The method `im.chat.get` will return the chat if called with the same pair of values ||
|#

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ENTITY_TYPE":"CRM","ENTITY_ID":"DEAL|1663"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.chat.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ENTITY_TYPE":"CRM","ENTITY_ID":"DEAL|1663","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.chat.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.chat.get',
            {
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
                'im.chat.get',
                [
                    'ENTITY_TYPE' => 'CRM',
                    'ENTITY_ID' => 'DEAL|1663',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'CHAT_ID: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.chat.get',
        {
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
        'im.chat.get',
        [
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
    "result": {
        "ID": 1437
    },
    "time": {
        "start": 1772028217,
        "finish": 1772028217.949613,
        "duration": 0.949613094329834,
        "processing": 0,
        "date_start": "2026-02-25T17:03:37+01:00",
        "date_finish": "2026-02-25T17:03:37+01:00",
        "operating_reset_at": 1772028817,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Returns an object with the chat ID `ID`. 

The method will return `null`:
- if the chat is not found
- if required parameters are not specified
- if parameters are filled incorrectly
||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

{% include notitle [error handling](../../_includes/error-info.md) %}

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-chat-add.md)
- [{#T}](./im-dialog-get.md)
- [{#T}](./chat-update/index.md)
- [{#T}](./im-recent-get.md)
- [{#T}](./im-recent-list.md)