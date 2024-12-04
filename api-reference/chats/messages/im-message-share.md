# Create an object based on the message im.message.share

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.message.share` creates new entities based on a chat message: a new chat, task, post in Feed, or event in the calendar.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **MESSAGE_ID^*^**
[`unknown`](../../data-types.md) | `289` | Identifier of the message for which a new entity will be created | 30 ||
|| **DIALOG_ID^*^**
[`unknown`](../../data-types.md) | `'chat74'` | Identifier of the dialog. Format:
- **chatXXX** – chat of the recipient, if the message is for a chat
- **XXX** – identifier of the recipient, if the message is for a private dialog | 30 ||
|| **TYPE^*^**
[`unknown`](../../data-types.md) | `'TASK'` | Type of the entity to be created:
- `'CHAT'` – a new chat will be created based on the message
- `'TASK'` – a task will be created based on the message
- `'POST'` – a post will be created in Feed based on the message
- `'CALEND'` – an event will be created in the calendar based on the message | 30 ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    B24.callMethod(
        'im.message.share',
        {
            MESSAGE_ID: 289,
            DIALOG_ID: 'chat74',
            TYPE: 'CHAT',
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

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response on success

```json
{
    "result": true
}
```

## Response on error

```json
{
    "error": "PARAMS_ERROR",
    "error_description": "Incorrect params"
}
```

### Description of keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **MESSAGE_ID_ERROR** | The parameter `MESSAGE_ID` is not set or is not a number ||
|| **DIALOG_ID_EMPTY** | The parameter `DIALOG_ID` is not set or does not match the format ||
|| **ACCESS_ERROR** | The current user does not have access permission to the chat or dialog ||
|| **PARAMS_ERROR** | The parameter `TYPE` is not set or does not match the existing types ||
|#