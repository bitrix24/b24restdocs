# Send a message to the open channel imopenlines.crm.message.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing
- from Sergei's file: on behalf of the employee, specify features and recommended sequence of actions

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method sends a message on behalf of the user in the CRM entity chat.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **CRM_ENTITY_TYPE***  
[`unknown`](../../../data-types.md) | Type of CRM entity:
- lead — lead
- deal — deal
- company — company
- contact — contact
 ||
|| **CRM_ENTITY_ID***  
[`unknown`](../../../data-types.md) | Identifier of the CRM entity ||
|| **USER_ID***  
[`unknown`](../../../data-types.md) | Identifier of the user or bot we want to add to the chat ||
|| **CHAT_ID***  
[`unknown`](../../../data-types.md) | Identifier of the chat ||
|| **MESSAGE***  
[`unknown`](../../../data-types.md) | Message text ||
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
        'imopenlines.crm.message.add',
        {
            CRM_ENTITY_TYPE: 'deal',
            CRM_ENTITY_ID: 288,
            USER_ID: 12,
            CHAT_ID: 8773,
            MESSAGE: 'Message text'
        }, function(result) {
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

Returns the identifier of the sent message.

```json
19880117
```

## Error Response

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ACCESS_DENIED** | The user does not have access ||
|| **ERROR_ARGUMENT** | One of the arguments is missing or incorrect ||
|| **CHAT_NOT_IN_CRM** | The chat does not belong to the CRM entity ||
|| **MESSAGE_ADD_ERROR** | Error sending the message ||
|#