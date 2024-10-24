# Add User to Existing Chat imopenlines.crm.chat.user.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds a user to a CRM entity chat.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`Type` | **Description** ||
|| **CRM_ENTITY_TYPE*** 
[`unknown`](../../../data-types.md) | Type of CRM entity:
- lead
- deal
- company
- contact ||
|| **CRM_ENTITY*** 
[`unknown`](../../../data-types.md) | Identifier of the CRM entity ||
|| **USER_ID*** 
[`unknown`](../../../data-types.md) | Identifier of the user or bot to be added to the chat ||
|| **CHAT_ID**
[`unknown`](../../../data-types.md) | Identifier of the chat. If not specified, the last chat linked to the CRM entity will be used ||
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
        'imopenlines.crm.chat.user.add',
        {
            CRM_ENTITY_TYPE: 'deal',
            CRM_ENTITY: 288,
            USER_ID: 12,
            CHAT_ID: 8773
        },
        function(result) {
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

Returns CHAT_ID on success.

```js
8773
```

## Error Response

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ACCESS_DENIED** | The current user does not have access ||
|| **ERROR_ARGUMENT** | One of the arguments is missing or incorrect ||
|| **IM_NOT_INSTALLED** | The im module is not installed ||
|| **CHAT_NOT_IN_CRM** | The chat does not belong to the CRM entity ||
|| **CRM_CHAT_USER_NOT_ACTIVE** | The current user does not have access to the list of users not in line ||
|#