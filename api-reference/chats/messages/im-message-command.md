# Using the Bot Command `im.message.command`

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

The method `im.message.command` utilizes bot commands.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **MESSAGE_ID^*^**
[`unknown`](../../data-types.md) | `278` | Identifier of the message to send a command to the bot | 30 ||
|| **BOT_ID^*^**
[`unknown`](../../data-types.md) | `1` | Identifier of the bot in the chat | 30 ||
|| **COMMAND^*^**
[`unknown`](../../data-types.md) | `'KEYBOARD'` | Command that the bot should execute | 30 ||
|| **COMMAND_PARAMS^*^**
[`unknown`](../../data-types.md) | `'stop'` | Parameters for the command | 30 ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

It is necessary to send a message that includes the selection of bot commands.

## Examples

```javascript
B24.callMethod(
    'im.message.command',
    {
        MESSAGE_ID: 278,
        BOT_ID: 1,
        COMMAND: 'KEYBOARD',
        COMMAND_PARAMS: 'stop'
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

{% include [Examples Note](../../../_includes/examples.md) %}

## Successful Response

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

### Description of Keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **MESSAGE_ID_ERROR** | The `MESSAGE_ID` parameter is not set or is not a number ||
|| **BOT_ID_ERROR** | The `BOT_ID` parameter is not set or is not a number ||
|| **COMMAND_ERROR** | The `COMMAND` parameter is not set ||
|| **PARAMS_ERROR** | The `COMMAND_PARAMS` parameter is not set or does not match the bot command parameter ||
|#