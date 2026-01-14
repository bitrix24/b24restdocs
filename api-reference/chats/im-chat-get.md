# Get Chat ID im.chat.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing
- error response not documented
- links to yet-to-be-created pages not provided
- from Sergei's file: unclear in what context the method is applicable, needs clarification

{% endnote %}

{% endif %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.get` retrieves the chat ID.

{% include [Parameter Notes](../../_includes/required.md) %}

#|
|| **Parameter** | **Example** | **Description** ||
|| **ENTITY_TYPE^*^**
[`unknown`](../data-types.md) | `CRM`, `LINES`, `LIVECHAT` | Identifier of the entity. Can be used to find the chat and easily determine the context in event handlers:
- [ONIMBOTMESSAGEADD](../chat-bots/messages/events/on-imbot-message-add.md),
- [ONIMBOTMESSAGEUPDATE](../chat-bots/messages/events/on-imbot-message-update.md),
- [ONIMBOTMESSAGEDELETE](../chat-bots/messages/events/on-imbot-message-delete.md) ||
|| **ENTITY_ID^*^**
[`unknown`](../data-types.md) | `LEAD`\|`13` | Numeric identifier of the entity. Can be used to find the chat and easily determine the context in event handlers:
- [ONIMBOTMESSAGEADD](../chat-bots/messages/events/on-imbot-message-add.md),
- [ONIMBOTMESSAGEUPDATE](../chat-bots/messages/events/on-imbot-message-update.md),
- [ONIMBOTMESSAGEDELETE](../chat-bots/messages/events/on-imbot-message-delete.md) ||
|#

{% note info "" %}

How to get the task chat ID is described in the article [New Task Card: Overview of Changes](../tasks/tasks-new.md)

{% endnote %}

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'im.chat.get',
    		{
    			ENTITY_TYPE: "LINES",
    			ENTITY_ID: "telegrambot|2|209607941|744"
    		}
    	);
    	
    	const result = response.getData().result;
    	result.error()
    		? console.error(result.error())
    		: console.info(result)
    	;
    }
    catch( error )
    {
    	console.error('Error:', error);
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
                    'ENTITY_TYPE' => "LINES",
                    'ENTITY_ID'   => "telegrambot|2|209607941|744"
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting chat information: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.chat.get',
        {
            ENTITY_TYPE: "LINES",
            ENTITY_ID: "telegrambot|2|209607941|744"
        
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    {% include [Explanation of restCommand](./_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.chat.get',
        Array(
            'ENTITY_TYPE' => 'CRM',
            'ENTITY_ID' => 'LEAD|13',
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Example Notes](../../_includes/examples.md) %}

## Successful Response

```json
{
    "result": 10
}
```

**Execution Result**: returns the chat ID `CHAT_ID` or `null`.