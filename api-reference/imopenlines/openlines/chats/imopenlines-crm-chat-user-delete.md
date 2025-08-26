# Delete User from Chat imopenlines.crm.chat.user.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

This method removes a user from the CRM entity chat.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **CRM_ENTITY_TYPE***  
[`unknown`](../../../data-types.md) | Type of CRM entity:
- lead
- deal
- company
- contact
 ||
|| **CRM_ENTITY***  
[`unknown`](../../../data-types.md) | Identifier of the CRM entity ||
|| **USER_ID***  
[`unknown`](../../../data-types.md) | Identifier of the user or bot we want to remove from the chat ||
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
    try
    {
        const response = await $b24.callMethod(
            'imopenlines.crm.chat.user.delete',
            {
                CRM_ENTITY_TYPE: 'deal',
                CRM_ENTITY: 288,
                USER_ID: 12,
                CHAT_ID: 8773
            }
        );
        
        const result = response.getData().result;
        console.log(result);
    }
    catch( error )
    {
        console.error(error.ex);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.crm.chat.user.delete',
                [
                    'CRM_ENTITY_TYPE' => 'deal',
                    'CRM_ENTITY'      => 288,
                    'USER_ID'         => 12,
                    'CHAT_ID'         => 8773,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error()->ex);
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting chat user: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.crm.chat.user.delete',
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

- PHP CRest

    // example for php

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
|| **CRM_CHAT_EMPTY_USER** | User identifier is not specified ||
|| **CRM_CHAT_EMPTY_CRM_DATA** | CRM data is not specified ||
|| **IM_NOT_INSTALLED** | IM module is not installed ||
|| **CHAT_NOT_IN_CRM** | Chat does not belong to the CRM entity ||
|| **CHAT_DELETE_USER_PERMISSION_DENIED** | User does not have access to the CRM entity ||
|| **CRM_CHAT_USER_NOT_ACTIVE** | The user being removed is not active ||
|#