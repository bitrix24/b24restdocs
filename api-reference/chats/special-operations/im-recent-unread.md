# Set or Remove the "Unread" Flag for the Chat im.recent.unread

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.recent.unread` sets the "unread" flag on a chat or conversation.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **DIALOG_ID^*^**
[`unknown`](../../data-types.md) | `'chat74'` | Identifier of the conversation. Format:
- **chatXXX** – chat of the recipient, if the message is for a chat
- **XXX** – identifier of the recipient, if the message is for a private conversation | 30 ||
|| **ACTION**
[`unknown`](../../data-types.md) | `'Y'` | Set / remove the "unread" flag on the conversation - `'Y'|'N'` | 30 ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```js
B24.callMethod(
    'im.recent.unread',
    {
        DIALOG_ID: 'chat74',
        ACTION: 'Y'
    },
    res => {
        if (res.error())
        {
        console.error(result.error().ex);
        }
        else
        {
        console.log(res.data())
        }
    }
)
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Successful Response

```json
{
    "result": true //if the flag was successfully set|removed
}
```

## Error Response

```json
{
    "error":"DIALOG_ID_EMPTY",
    "error_description":"Dialog ID can't be empty"
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **DIALOG_ID_EMPTY** | The `DIALOG_ID` parameter was not provided or does not match the format. ||
|#