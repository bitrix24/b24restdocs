# How to Interact with Notification Buttons im.notify.confirm

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- revisions needed for writing standards
- parameter types not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.confirm` interacts with notification buttons.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ID^*^**
[`unknown`](../../data-types.md) | `288` | Identifier of the notification that supports response selection via button clicks | `30` ||
|| **NOTIFY_VALUE^*^**
[`unknown`](../../data-types.md) | `'Y'` | Value of the selected response (button value) | `30` ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

For example, consider the notification:

- the **Accept** button has the value `'Y'`
- the **Decline** button has the value `'N'`

## Examples

```js
B24.callMethod(
    'im.notify.confirm',
    {
        ID: 288,
        NOTIFY_VALUE: 'Y'
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
);
```

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Success Response

```json
{
    "result_message": [
        "Invitation accepted"
    ]
}
```

## Error Response

```json
{
    "error":"NOTIFY_VALUE_ERROR",
    "error_description":"Notification Value can't be empty"
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ID_ERROR** | Parameter `ID` not provided or it is not a number ||
|| **NOTIFY_VALUE_ERROR** | Parameter `NOTIFY_VALUE` not specified or it is empty ||
|#