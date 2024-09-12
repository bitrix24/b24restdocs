# Responding to a notification that supports quick reply im.notify.answer

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

The method `im.notify.answer` provides a response to a notification that supports quick replies.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ID^*^**
[`unknown`](../../data-types.md) | `270` | Identifier of the notification that supports quick reply | `30` ||
|| **ANSWER_TEXT^*^**
[`unknown`](../../data-types.md) | `'Hello'` | Text of the quick reply | `30` ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

```js
B24.callMethod(
    'im.notify.answer',
    {
        ID: 270,
        ANSWER_TEXT: 'Hello'
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

{% include [Examples Notes](../../../_includes/examples.md) %}

## Successful Response

```json
{
    "result_message": [
        "Your response has been successfully sent."
    ]
}
```

An array of messages regarding your response is returned.

Example of a response if an identifier of a notification that does not support quick reply is provided:

```json
{
    "result_message": false
}
```

## Error Response

```json
{
    "error":"NOTIFY_ID_ERROR",
    "error_description":"Notification ID can't be empty"
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ID_ERROR** | The parameter `ID` was not provided or it is not a number ||
|| **ANSWER_TEXT_ERROR** | The parameter `ANSWER_TEXT` was not provided or it is not a non-empty string ||
|#