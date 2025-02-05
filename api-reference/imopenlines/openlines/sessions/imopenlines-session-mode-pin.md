# Pin or Unpin Dialog imopenlines.session.mode.pin

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method pins or unpins a dialog to the current operator.

## Method Parameters

{% include [Parameter Notes](../../../../_includes/required.md) %}

#| 
|| **Name** 
`Type` | **Example** | **Default** | **Description** ||
|| **CHAT_ID*** 
[`unknown`](../../../data-types.md) | `2020` | | Identifier of the chat ||
|| **ACTIVATE** 
[`unknown`](../../../data-types.md) | `Y` | N | Activation flag ||
|#

## Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS

    ```js
    BX24.callMethod(
        'imopenlines.session.mode.pin',
        {
            CHAT_ID: 2024
        },
        function(result)
        {
            if(result.error())
            {
                console.warn(result.error().ex);
                return false;
            }

            console.log(result.data());
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

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| **ACCESS_DENIED** | The current user does not have access to the specified chat ||
|| **CHAT_TYPE** | The specified chat is not an open line ||
|| **CHAT_ID** | An incorrect chat identifier has been provided ||
|| **ERROR_USER_NOT_OPERATOR** | Attempt to pin/unpin the chat by a user who is not an operator ||
|#