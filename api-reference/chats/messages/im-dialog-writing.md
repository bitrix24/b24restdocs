# Send the "someone is writing to you..." im.dialog.writing

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

The method `im.dialog.writing` sends the status "someone is writing to you...".

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **DIALOG_ID^*^**
[`unknown`](../../data-types.md) | `13` | Identifier of the dialog. Format:
- **chatXXX** – chat of the recipient, if the message is for a chat
- **XXX** – identifier of the recipient, if the message is for a private dialog | 30 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

```js
B24.callMethod(
    'im.dialog.writing',
    {
        'DIALOG_ID': '13'
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

{% include [Examples Notes](../../../_includes/examples.md) %}

## Response on Success

```json
{
"result": true
}
```

## Response on Error

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat identifier not provided"
}
```

### Description of Keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **DIALOG_ID_EMPTY** | The `DIALOG_ID` parameter is not provided or does not match the format `XXX` ||
|| **CHAT_ID_EMPTY** | The `DIALOG_ID` parameter is not provided or does not match the format `chatXXX` ||
|| **ACCESS_ERROR**| The user does not have permission for this chat ||
|#