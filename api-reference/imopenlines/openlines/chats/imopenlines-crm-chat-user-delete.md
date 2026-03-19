# Remove User from Chat imopenlines.crm.chat.user.delete

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to the CRM entity

The method `imopenlines.crm.chat.user.delete` removes a user from the chat associated with the CRM entity.

## Method Parameters

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CRM_ENTITY_TYPE***
[`string`](../../../data-types.md) | Type of the CRM entity. Possible values:
- `lead` — lead
- `deal` — deal
- `company` — company
- `contact` — contact ||
|| **CRM_ENTITY***
[`integer`](../../../data-types.md) | Identifier of the CRM entity.

You can obtain the identifier using the universal method [getting a list of CRM entities](../../../crm/universal/crm-item-list.md) ||
|| **USER_ID***
[`integer`](../../../data-types.md) | Identifier of the user or bot to be removed from the chat.

You can obtain the user identifier using the [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) methods ||
|| **CHAT_ID**
[`integer`](../../../data-types.md) | Identifier of the chat. 

By default, the last chat associated with the CRM entity is used.

You can obtain the identifiers of chats associated with the CRM entity using the [imopenlines.crm.chat.get](./imopenlines-crm-chat-get.md) method ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CRM_ENTITY_TYPE":"lead","CRM_ENTITY":1205,"USER_ID":503,"CHAT_ID":1763}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.crm.chat.user.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CRM_ENTITY_TYPE":"lead","CRM_ENTITY":1205,"USER_ID":503,"CHAT_ID":1763,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imopenlines.crm.chat.user.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imopenlines.crm.chat.user.delete',
            {
                CRM_ENTITY_TYPE: 'lead',
                CRM_ENTITY: 1205,
                USER_ID: 503,
                CHAT_ID: 1763
            }
        );

        const result = response.getData().result;
        console.log(result);
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
                'imopenlines.crm.chat.user.delete',
                [
                    'CRM_ENTITY_TYPE' => 'lead',
                    'CRM_ENTITY' => 1205,
                    'USER_ID' => 503,
                    'CHAT_ID' => 1763,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.crm.chat.user.delete',
        {
            CRM_ENTITY_TYPE: 'lead',
            CRM_ENTITY: 1205,
            USER_ID: 503,
            CHAT_ID: 1763
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
        'imopenlines.crm.chat.user.delete',
        [
            'CRM_ENTITY_TYPE' => 'lead',
            'CRM_ENTITY' => 1205,
            'USER_ID' => 503,
            'CHAT_ID' => 1763,
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 1763,
    "time": {
        "start": 1773814361,
        "finish": 1773814361.128245,
        "duration": 0.12824511528015137,
        "processing": 0,
        "date_start": "2026-03-18T09:12:41+01:00",
        "date_finish": "2026-03-18T09:12:41+01:00",
        "operating_reset_at": 1773814961,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the chat from which the user was removed.

If `CHAT_ID` is not provided and the chat for the CRM entity is not found or does not exist, the method will return `"result":0` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "The value of an argument CRM_ENTITY has an invalid type"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `ACCESS_DENIED` | Access denied! You don't have access to join user to chat | Possible reasons:
- invalid or non-existent `CRM_ENTITY_TYPE` specified 
- the user executing the method does not have access to the CRM entity
||
|| `400` | `CRM_CHAT_EMPTY_USER` | User identifier is not specified | Required parameter `USER_ID` is not specified ||
|| `400` | `CRM_CHAT_EMPTY_CRM_DATA` | Empty CRM data | Required parameters `CRM_ENTITY_TYPE` and `CRM_ENTITY` are not provided ||
|| `400` | `CRM_CHAT_EMPTY_CRM_DATA` | CRM data is not specified | CRM data is not provided ||
|| `400` | `ERROR_ARGUMENT` | The value of an argument CRM_ENTITY has an invalid type | The parameter `CRM_ENTITY` is passed in an incorrect format ||
|| `400` | `IM_NOT_INSTALLED` | Module im is not installed | The `im` module is not installed ||
|| `400` | `CHAT_NOT_IN_CRM` | Chat does not belong to the CRM entity being checked | Possible reasons:
- the chat is not linked to the CRM entity
- the chat with the specified `CHAT_ID` does not exist ||
|| `403` | `CHAT_DELETE_USER_PERMISSION_DENIED` | You don't have access to delete a user from this chat | The user does not have permission to remove a participant from the chat ||
|| `400` | `CRM_CHAT_USER_NOT_ACTIVE` | Chat user is not active | The user being removed with `USER_ID` is not active or does not exist ||
|| `400` | `WRONG_REQUEST` | You don't have access or user already not in chat | The user has already been removed from the chat or is unavailable for removal ||
|| `400` | `USER_NOT_FOUND` | The specified user is not in the chat | The user with identifier `CHAT_ID` is not in the specified chat ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./imopenlines-crm-chat-get-last-id.md)
- [{#T}](./imopenlines-crm-chat-get.md)
- [{#T}](./imopenlines-crm-chat-user-add.md)