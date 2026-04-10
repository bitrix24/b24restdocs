# Add User to Existing Chat imopenlines.crm.chat.user.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to the CRM entity

The method `imopenlines.crm.chat.user.add` adds a user to a chat associated with a CRM entity.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

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

You can obtain the identifier using the universal method [get CRM item list](../../../crm/universal/crm-item-list.md) ||
|| **USER_ID***
[`integer`](../../../data-types.md) | Identifier of the user or bot to be added to the chat.

You can obtain the user identifier using the [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) methods ||
|| **CHAT_ID**
[`integer`](../../../data-types.md) | Identifier of the chat.

By default, the last chat associated with the CRM entity is used.

You can obtain the identifiers of chats associated with the CRM entity using the [imopenlines.crm.chat.get](./imopenlines-crm-chat-get.md) method ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CRM_ENTITY_TYPE":"lead","CRM_ENTITY":1205,"USER_ID":503,"CHAT_ID":1763}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.crm.chat.user.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CRM_ENTITY_TYPE":"lead","CRM_ENTITY":1205,"USER_ID":503,"CHAT_ID":1763,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imopenlines.crm.chat.user.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imopenlines.crm.chat.user.add',
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
                'imopenlines.crm.chat.user.add',
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
        'imopenlines.crm.chat.user.add',
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
        'imopenlines.crm.chat.user.add',
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
        "start": 1773814042,
        "finish": 1773814043.189039,
        "duration": 1.1890389919281006,
        "processing": 0,
        "date_start": "2026-03-18T09:07:22+01:00",
        "date_finish": "2026-03-18T09:07:23+01:00",
        "operating_reset_at": 1773814643,
        "operating": 0.16756796836853027
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the chat to which the user has been added.

If `CHAT_ID` is not provided and no chat is found for the CRM entity, the method will return `"result":0` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "CHAT_NOT_IN_CRM",
    "error_description": "Chat does not belong to the CRM entity being checked"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `ACCESS_DENIED` | Access denied! You don't have access to join user to chat | The user executing the method does not have permission to add users to the CRM entity chat ||
|| `403` | `ACCESS_DENIED` | Access denied! This user does not have access to the chat because he does not have access to this CRM entity | The user `USER_ID` does not have access to the CRM entity ||
|| `400` | `ERROR_ARGUMENT` | Argument CRM_ENTITY_TYPE is null or empty | Required parameter `CRM_ENTITY_TYPE` is not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument CRM_ENTITY is null or empty | Required parameter `CRM_ENTITY` is not provided ||
|| `400` | `ERROR_ARGUMENT` | The value of an argument `CRM_ENTITY` has an invalid type | The `CRM_ENTITY` parameter is provided in an incorrect format ||
|| `400` | `ERROR_ARGUMENT` | Argument Empty USER_ID is null or empty | Required parameter `USER_ID` is not provided ||
|| `400` | `IM_NOT_INSTALLED` | Module im is not installed | The `im` module is not installed ||
|| `400` | `CHAT_NOT_IN_CRM` | Chat does not belong to the CRM entity being checked | The chat `CHAT_ID` is not associated with the CRM entity ||
|| `400` | `CRM_CHAT_USER_NOT_ACTIVE` | Current user has no access to users list outside open line | The current user does not have access to the user list outside the open line ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./imopenlines-crm-chat-get-last-id.md)
- [{#T}](./imopenlines-crm-chat-get.md)
- [{#T}](./imopenlines-crm-chat-user-delete.md)