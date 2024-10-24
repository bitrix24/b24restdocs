# Finish the dialog of another operator imopenlines.operator.another.finish

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing parameters or fields
- parameter types not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method finishes the dialog of another operator.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Example** | **Description** ||
|| **CHAT_ID***
[`unknown`](../../../data-types.md) |`2020` | Identifier of the chat ||
|#

## Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS

    ```javascript
    BX.callMethod(
        'imopenlines.operator.another.finish',
        {
            CHAT_ID: 2024
        },
        function(result)
        {
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

    // example for PHP

{% endlist %}

## Success Response

```json
true
```

## Error Response

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ACCESS_DENIED** | The current user does not have access to the specified chat ||
|| **CHAT_TYPE** | The specified chat is not an open line ||
|| **CHAT_ID** | An incorrect chat identifier was provided ||
|#