# Get the ID of the Last Chat imopenlines.crm.chat.getLastId

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves the `ID` of the last chat associated with a CRM entity.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** | **Available since** ||
|| **CRM_ENTITY_TYPE***
[`unknown`](../../../data-types.md) | Type of CRM entity: 
- LEAD — lead
- DEAL — deal
- COMPANY — company
- CONTACT — contact
 | ||
|| **CRM_ENTITY***
[`unknown`](../../../data-types.md) | Identifier of the CRM entity | ||
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
    function crmChatGetLastId() {
        var params = {
            CRM_ENTITY_TYPE: 'LEAD',
            CRM_ENTITY: 1,
        };
        BX24.callMethod(
            'imopenlines.crm.chat.getLastId',
            params,
            function (result) {
                if (result.error())
                    alert("Error: " + result.error());
                else
                    alert("Success: " + result.data());
            }
        );
    }
    ```

- PHP

    // example for PHP

{% endlist %}