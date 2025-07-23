# Get Chat Data im.dialog.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.dialog.get` retrieves information about a dialog.

{% include [Footnote on parameters](../../_includes/required.md) %}

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **DIALOG_ID^*^**
[`string`](../data-types.md) | `chat29`
or
`256` | Identifier of the dialog. Format:
- **chatXXX** – chat of the recipient, if the message is for a chat
- **XXX** – identifier of the recipient, if the message is for a private dialog | 24 ||
|#

## Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'im.dialog.get',
        {
            DIALOG_ID: 'chat1'
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

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"DIALOG_ID":"chat1"}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.dialog.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"DIALOG_ID":"chat1","auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/im.dialog.get
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.dialog.get',
        [
            'DIALOG_ID' => 'chat1'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

```json
{
    "result":
    {
        "id": "21191",
        "title": "Mint Chat #3",
        "owner": "2",
        "extranet": false,
        "avatar": "",
        "color": "#4ba984",
        "type": "chat",
        "entity_type": "",
        "entity_data_1": "",
        "entity_data_2": "",
        "entity_data_3": "",
        "date_create": "2017-10-14T12:15:32+02:00",
        "message_type": "C"
    }
}
```

### Key Descriptions

- `id` – chat identifier
- `title` – chat title
- `owner` – identifier of the user who owns the chat
- `extranet` – indicates whether an external extranet user is participating in the chat (`true/false`)
- `color` – chat color in hex format
- `avatar` – link to the avatar (if empty, it means no avatar is set)
- `type` – type of chat (group chat, call chat, open channel chat, etc.)
- `entity_type` – external code for the chat – type
- `entity_id` – external code for the chat – identifier
- `entity_data_1` – external data for the chat
- `entity_data_2` – external data for the chat
- `entity_data_3` – external data for the chat
- `date_create` – chat creation date in ATOM format
- `message_type` – type of chat messages

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
|| **DIALOG_ID_EMPTY** | Dialog identifier not provided ||
|| **ACCESS_ERROR** | Current user does not have access permission to the dialog ||
|#