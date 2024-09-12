# Get Chat by Symbolic Code imopenlines.session.open

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent
- from Sergey's file: also recommend the method for getting the chat by CRM object ID, as it is more reliable

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the chat identifier by USER_CODE.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Example** | **Description** ||
|| **USER_CODE***
[`unknown`](../../../data-types.md) | `livechat`\|`58`\|`2042`\|`479` | Chat code, can be found in ENTITY_ID ||
|#

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
        'imopenlines.session.open',
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

## Response in Case of Success

```json
{
    "chatId":"2043"
}
```

## Response in Case of Error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ACCESS_DENIED** | The current user does not have access to the specified chat ||
|#