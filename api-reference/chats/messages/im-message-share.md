# Create an Object Based on the Message im.message.share

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `im.message.share` creates new entities based on a chat message: a new chat, a task, a post in News, or an event in the calendar.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **MESSAGE_ID^*^**
[`unknown`](../../data-types.md) | `289` | Identifier of the message for which a new entity will be created | 30 ||
|| **DIALOG_ID^*^**
[`unknown`](../../data-types.md) | `'chat74'` | Identifier of the dialog. Format:
- **chatXXX** – chat of the recipient if the message is for a chat
- **XXX** – identifier of the recipient if the message is for a private dialog | 30 ||
|| **TYPE^*^**
[`unknown`](../../data-types.md) | `'TASK'` | Type of the entity being created:
- `'CHAT'` – a new chat will be created based on the message
- `'TASK'` – a task will be created based on the message
- `'POST'` – a post in News will be created based on the message
- `'CALEND'` – an event in the calendar will be created based on the message | 30 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

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

{% include [Example Notes](../../../_includes/examples.md) %}

## Success Response

```json
{
    "result": true
}
```

## Error Response

```json
{
    "error": "PARAMS_ERROR",
    "error_description": "Incorrect params"
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **MESSAGE_ID_ERROR** | The `MESSAGE_ID` parameter is not set or is not a number ||
|| **DIALOG_ID_EMPTY** | The `DIALOG_ID` parameter is not set or does not match the format ||
|| **ACCESS_ERROR** | The current user does not have access permission to the chat or dialog ||
|| **PARAMS_ERROR** | The `TYPE` parameter is not set or does not match the available types ||
|#