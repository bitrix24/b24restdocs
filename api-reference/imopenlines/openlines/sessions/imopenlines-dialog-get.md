# Get Information About the Open Channel Operator's Dialogue imopenlines.dialog.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves information about the operator's dialogue (chat) in the open channel.

## Method Parameters

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID**
[`integer`](../../../data-types.md) | `13` | Numeric identifier of the chat | 2 ||
|| **DIALOG_ID**
[`string`](../../../data-types.md) | `chat29`
or
`256` | Identifier of the dialogue. Format:
- **chatXXX** — chat of the recipient, if the message is for a chat
- **XXX** — identifier of the recipient, if the message is for a private dialogue | 2 ||
|| **SESSION_ID**
[`integer`](../../../data-types.md) | `1743` | Identifier of the session within the open channel | 2 ||
|| **USER_CODE**
[`string`](../../../data-types.md) | `livechat`\|`1`\|`1373`\|`211` | String identifier of the open channel user from CRM, example `livechat`\|`1`\|`1373`\|`211` or `imol`\|`livechat`\|`1`\|`1373`\|`211` | 2 ||
|#

Any of the parameters can be used for the call.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'imopenlines.dialog.get',
        {
            USER_CODE: 'livechat|1|1373|211'
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

    {% include [Explanation about restCommand](../../../chat-bots/_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'imopenlines.dialog.get',
        Array(
            'DIALOG_ID' => 'chat29'
        ),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}

## Successful Response

```json
{
    "result": {
        "avatar": "",
        "color": "#4ba984",
        "date_create": "2020-05-12T17:40:55+02:00",
        "dialog_id": null,
        "entity_data_1": "N|NONE|0|N|N|0|1591872180|1|0|",
        "entity_data_2": "",
        "entity_data_3": "",
        "entity_id": "livechat|1|1363|203",
        "entity_type": "LINES",
        "extranet": false,
        "id": 1364,
        "manager_list": [],
        "message_type": "L",
        "name": "Eugene Perekopsky - Priority Support",
        "owner": 0,
        "type": "lines"
    }
}
```

### Key Descriptions

- `avatar` – link to the avatar (if empty, the avatar is not set)
- `color` – chat color in hex format
- `date_create` – chat creation date in ATOM format
- `dialog_id` – identifier of the dialogue
- `entity_data_1` – external data for the chat
- `entity_data_2` – external data for the chat
- `entity_data_3` – external data for the chat
- `entity_id` – external code for the chat – identifier
- `entity_type` – external code for the chat – type
- `extranet` – indicator of external extranet user participation in the chat (`true/false`)
- `id` – identifier of the chat
- `manager_list` – list of operators
- `message_type` – type of chat messages
- `name` – name of the open channel
- `owner` – identifier of the user who owns the chat
- `type` – type of chat (group chat, call chat, open channel chat, etc.)

## Error Response

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **DIALOG_ID_EMPTY** | Dialogue identifier not provided ||
|| **ACCESS_ERROR** | Current user does not have access permission to the dialogue ||
|#