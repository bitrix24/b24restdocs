# Rate Employee's Performance in the imopenlines.session.head.vote Dialog

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method is used for rating the dialogue by a supervisor.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Example** | **Description** ||
|| **SESSION_ID***
[`unknown`](../../../data-types.md) | `494` | Session identifier ||
|| **RATING**
[`unknown`](../../../data-types.md) | `5` | Number of stars from 1 to 5 ||
|| **COMMENT**
[`unknown`](../../../data-types.md) | `Well done` | Supervisor's comment ||
|#

**Note:** Although two parameters are optional, at least one of them must be filled.

## Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS


    ```js
    BX24.callMethod(
        'imopenlines.session.head.vote',
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
|| **CHAT_ID** | An incorrect chat identifier has been provided ||
|| **WRONG_PARAMETER** | At least one of the optional parameters must be specified ||
|#