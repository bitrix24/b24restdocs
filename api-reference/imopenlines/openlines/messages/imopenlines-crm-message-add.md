# Send a Message to Open Channel imopenlines.crm.message.add

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imopenlines.crm.message.add` sends a message on behalf of an employee or bot in a chat linked to a CRM entity.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **CRM_ENTITY_TYPE***  
[`string`](../../../data-types.md) | Type of the CRM entity:
- lead — lead
- deal — deal
- company — company
- contact — contact
 ||
|| **CRM_ENTITY***  
[`integer`](../../../data-types.md) | Identifier of the CRM entity linked to the chat.

You can obtain a list of entities of a specific CRM entity type using the method [crm.item.list](../../../crm/universal/crm-item-list.md) ||
|| **USER_ID***  
[`integer`](../../../data-types.md) | Identifier of the message sender — user or bot, who must be a participant in the chat.

You can obtain the user ID using the method [user.get](../../../user/user-get.md) or [user.search](../../../user/user-search.md).

A list of chat bots can be obtained using the method [imbot.bot.list](../../../chat-bots/outdated/bots/imbot-bot-list.md) ||
|| **CHAT_ID***  
[`integer`](../../../data-types.md) | Identifier of the open channel chat linked to the CRM entity. 

You can obtain the chat ID using the method [imopenlines.crm.chat.get](../chats/imopenlines-crm-chat-get.md) or [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md) ||
|| **MESSAGE***  
[`string`](../../../data-types.md) | The text of the message that will be displayed in the chat ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CRM_ENTITY_TYPE":"lead","CRM_ENTITY":1195,"USER_ID":27,"CHAT_ID":1341,"MESSAGE":"Message text"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.crm.message.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CRM_ENTITY_TYPE":"lead","CRM_ENTITY":1195,"USER_ID":27,"CHAT_ID":1341,"MESSAGE":"Message text","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imopenlines.crm.message.add
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod(
        'imopenlines.crm.message.add',
        {
          CRM_ENTITY_TYPE: 'lead',
          CRM_ENTITY: 1195,
          USER_ID: 27,
          CHAT_ID: 1341,
          MESSAGE: 'Message text',
        }
      );

      const { result } = response.getData();
      console.log('Created message:', result);
    } catch (error) {
      console.error('Error sending message:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.crm.message.add',
                [
                    'CRM_ENTITY_TYPE' => 'lead',
                    'CRM_ENTITY' => 1195,
                    'USER_ID' => 27,
                    'CHAT_ID' => 1341,
                    'MESSAGE' => 'Message text',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Created message ID: ' . $result->data();
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error sending message: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.crm.message.add',
        {
            CRM_ENTITY_TYPE: 'lead',
            CRM_ENTITY: 1195,
            USER_ID: 27,
            CHAT_ID: 1341,
            MESSAGE: 'Message text',
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
        'imopenlines.crm.message.add',
        [
            'CRM_ENTITY_TYPE' => 'lead',
            'CRM_ENTITY' => 1195,
            'USER_ID' => 27,
            'CHAT_ID' => 1341,
            'MESSAGE' => 'Message text',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Created message ID: ' . $result['result'];
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": 19880117,
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00",
        "operating_reset_at":1762349466,
        "operating": 0
    }
}
```

## Returned Result

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created message in the chat ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time  ||
|#

## Error Handling

HTTP Code: **400**

```json
{
    "error": "CHAT_NOT_IN_CRM",
    "error_description": "Chat does not belong to the CRM entity being checked"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|
|| **Code** | **Description** | **Value** ||
|| `CHAT_NOT_IN_CRM`| Chat does not belong to the CRM entity being checked | Chat is not linked to the CRM ||
|| `CANCELED`| You cannot send messages to the specified chat | User does not have access to the chat ||
|| `ACCESS_DENIED`| Access denied! User doesn't have access to this entity | User does not have access to the CRM entity ||
|| `ERROR_ARGUMENT` | Argument `CRM_ENTITY_TYPE` is null or empty | Invalid required parameter `CRM_ENTITY_TYPE` ||
|| `ERROR_ARGUMENT` | Argument `CRM_ENTITY` is null or empty | Invalid required parameter `CRM_ENTITY` ||
|| `ERROR_ARGUMENT` | Argument `USER_ID` is null or empty | Invalid required parameter `USER_ID` ||
|| `ERROR_ARGUMENT` | Argument `MESSAGE` is null or empty | Invalid required parameter `MESSAGE` ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-message-quick-save.md)