# Get Chat for CRM Object imopenlines.crm.chat.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves the chat for a CRM object.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **CRM_ENTITY_TYPE***
[`unknown`](../../../data-types.md) | Type of CRM entity 
- lead
- deal
- company
- contact
 ||
|| **CRM_ENTITY***
[`unknown`](../../../data-types.md) | Identifier of the CRM entity ||
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
        'imopenlines.crm.chat.get',
        {
            CRM_ENTITY_TYPE: 'deal',
            CRM_ENTITY: 288,
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

## Response on Success

An array of objects containing the chat ID, connector ID, and connector title.

```json
{
    "result": [
        {
            "CHAT_ID": "9852",
            "CONNECTOR_ID": "livechat",
            "CONNECTOR_TITLE": "Live Chat"
        }
    ]
}
```
## Response on Error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ACCESS_DENIED** | The current user does not have access ||
|| **ERROR_ARGUMENT** | One of the arguments is missing or incorrect ||
|#