# Set the "read" flag for messages im.dialog.read

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.dialog.read` changes the read status of messages. All messages up to the specified one (including the message itself) are marked as read.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **DIALOG_ID^*^**
[`unknown`](../../data-types.md) | `chat29`
or
`256` | Identifier of the dialog. Format:
- **chatXXX** – recipient's chat if the message is for a chat
- **XXX** – recipient's identifier if the message is for a private dialog | 21 ||
|| **MESSAGE_ID^*^**
[`unknown`](../../data-types.md) | `12` | Identifier of the last read message in the dialog | 21 ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples 

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.dialog.read',
        {
            'DIALOG_ID': chat29,
            'MESSAGE_ID': 12,
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

    {% include [Explanation about restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.dialog.read',
        Array(
            'DIALOG_ID' => chat29,
            'MESSAGE_ID' => 12,
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}

## Successful Response

```json
{
    "result":
    {
        "dialogId": "chat76",
        "chatId": 76,
        "counter": 1,
        "lastId": 6930
    }
}
```

- **dialogId** – identifier of the read dialog
- **chatId** – identifier of the chat
- **counter** – number of unread messages after executing the method
- **lastId** – last read message

If the method fails to set the new read mark:

```json
{
"result": false
}
```

## Error Response

```json
{
    "error": "MESSAGE_ID_ERROR",
    "error_description": "Message ID can't be empty"
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **MESSAGE_ID_ERROR** | An incorrect message identifier was provided ||
|| **DIALOG_ID_EMPTY** | An incorrect dialog identifier was provided ||
|#