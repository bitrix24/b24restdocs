# Set the "unread" flag for messages im.dialog.unread

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.dialog.unread` changes the read status of messages. All messages after the specified one (including the message itself) are marked as unread.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **DIALOG_ID^*^**
[`unknown`](../../data-types.md) | `chat29`
or
`256` | Identifier of the dialog. Format:
- **chatXXX** – chat of the recipient, if the message is for a chat
- **XXX** – identifier of the recipient, if the message is for a private dialog | 21 ||
|| **MESSAGE_ID^*^**
[`unknown`](../../data-types.md) | `12` | Identifier of the first unread message | 21 ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.dialog.unread',
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
        'im.dialog.unread',
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

{% include [Note on examples](../../../_includes/examples.md) %}

## Response on success

```json
{
    "result": true
}
```

## Response on error

```json
{
    "error": "MESSAGE_ID_ERROR",
    "error_description": "Message ID can't be empty"
}
```

### Description of keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **MESSAGE_ID_ERROR** | An incorrect message identifier was specified ||
|| **DIALOG_ID_EMPTY** | An incorrect dialog identifier was specified ||
|#