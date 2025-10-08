# Transfer the conversation to an operator by Id imopenlines.bot.session.transfer

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines, imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method transfers the conversation to a specific operator.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Example** | **Description** | **Revision** ||
|| **CHAT_ID***
[`unknown`](../../../data-types.md) | `112` | Identifier of the chat | 1 ||
|| **USER_ID***
[`unknown`](../../../data-types.md) | `12` | Identifier of the user to whom the conversation is being redirected | 1 ||
|| **LEAVE***
[`unknown`](../../../data-types.md) | `N` | Y/N. If N is specified, the chatbot will not leave this chat after redirection and will remain until the user confirms | 1 ||
|#

{% note info %}

Instead of `USER_ID`, you can specify `QUEUE_ID` to switch to another open line. The `ID` of the open line can be obtained using the method [imopenlines.config.list.get](../imopenlines-config-list-get.md).

{% endnote %}

## Examples

{% include [Explanation of restCommand](../../../chat-bots/_includes/rest-command.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":112,"USER_ID":12,"LEAVE":"N"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.bot.session.transfer
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":112,"USER_ID":12,"LEAVE":"N","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imopenlines.bot.session.transfer
    ```

- JS

    ```js  
    try
    {
        const response = await $b24.callMethod(
            'imopenlines.bot.session.transfer',
            {
                CHAT_ID: 112,
                USER_ID: 12,
                LEAVE: 'N'
            }
        );
        
        const result = response.getData().result;
        console.log('Conversation successfully transferred:', result);
        processResult(result);
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
                'imopenlines.bot.session.transfer',
                [
                    'CHAT_ID' => 112,
                    'USER_ID' => 12,
                    'LEAVE' => 'N'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error transferring session: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.bot.session.transfer', 
        {
            CHAT_ID: 112,
            USER_ID: 12, 
            LEAVE: 'N'  
        },
        function(result) {
            if (result.error()) {
                console.error('Error:', result.error().ex);
            } else {
                console.log('Conversation successfully transferred:', result.data());
            }
        }
    );
    ```	

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imopenlines.bot.session.transfer',
        [
            'CHAT_ID' => 112,
            'USER_ID' => 12,
            'LEAVE' => 'N'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

{% include [Note on examples](../../../../_includes/examples.md) %}

## Response on success

```json
{
    "result": true
}
```

## Response on error

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat identifier not provided"
}
```

### Description of keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided ||
|| **USER_ID_EMPTY** | User identifier to whom the conversation needs to be redirected not provided ||
|| **WRONG_CHAT** | Incorrect user identifier specified or this user is a chatbot or extranet user ||
|| **BOT_ID_ERROR** | Incorrect chatbot identifier ||
|#