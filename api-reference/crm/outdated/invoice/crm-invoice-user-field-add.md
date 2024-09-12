# Create a Custom Field for Invoices crm.invoice.userfield.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

This method creates a new custom field for invoices.

The system limit for the field name is 20 characters. The custom field name always has the prefix `UF_CRM_`, meaning the actual length of the name is 13 characters.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields** | A set of fields - an array of the form `array("field"=>"value"[, ...])`, containing the description of the custom field. A complete description of the fields can be obtained by calling the method [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md). ||
|| **LIST** | Contains a set of list values for custom fields of type List. Specified when creating/updating the field. Each value is an array with the fields: 
- **VALUE** - the value of the list item. This field is required when creating a new item.  
- **SORT** - sorting. 
- **DEF** - if equal to Y, then the list item is the default value. For multiple fields, several DEF=Y are allowed. For non-multiple fields, the first one will be considered default.  
- **XML_ID** - external code of the value. This parameter is only considered when updating already existing values of the list item.
- **ID** - the identifier of the value. If specified, it is considered an update of an existing list item value, not the creation of a new one. It only makes sense when calling the `*.userfield.update` methods.
- **DEL** - if equal to Y, then the existing list item will be deleted. Used if the ID parameter is filled. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"%NAME":"Estimate"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.paysystem.list  
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"%NAME":"Estimate"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.paysystem.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.paysystem.list", {
            order: {"SORT": "ASC"},
            filter: {
                "%NAME": "Estimate",
            }
        },
        function (result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.dir(result.data());
                if (result.more())
                    result.next();
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.paysystem.list',
        [
            'order' => ['SORT' => 'ASC'],
            'filter' => ['%NAME' => 'Estimate']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}