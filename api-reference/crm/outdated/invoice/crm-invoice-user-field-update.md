# Update Custom Field crm.invoice.userfield.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

This method updates an existing custom field for invoices.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id** | Identifier of the custom field ||
|| **fields** | Set of fields - an array of the form `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values from the method [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md) ||
|| **LIST** | Contains a set of list values for custom fields of type List. Specified when creating/updating the field. Each value is an array with the following fields: 
- **VALUE** - the value of the list item. This field is required when creating a new item  
- **SORT** - sorting 
- **DEF** - if equal to Y, the list item is the default value. For multiple fields, multiple DEF=Y are allowed. For non-multiple fields, the first one will be considered default  
- **XML_ID** - external code of the value. This parameter is considered only when updating already existing values of the list item
- **ID** - identifier of the value. If specified, it is considered that this is an update of an existing list item value, not the creation of a new one. Makes sense only when calling the `*.userfield.update` methods
- **DEL** - if equal to Y, the existing list item will be deleted. Applied if the ID parameter is filled ||
|#

## Code Examples

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id": "**put_id_here**", "fields": {"EDIT_FORM_LABEL": "**put_label_here**", "LIST_COLUMN_LABEL": "**put_label_here**"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.invoice.userfield.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id": "**put_id_here**", "fields": {"EDIT_FORM_LABEL": "**put_label_here**", "LIST_COLUMN_LABEL": "**put_label_here**"}, "auth": "**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.invoice.userfield.update
    ```

- JS

    ```js
    var id = prompt("Enter ID");
    var label = prompt("Enter new name");
    BX24.callMethod(
        "crm.invoice.userfield.update",
        {
            id: id,
            fields: {
                "EDIT_FORM_LABEL": label,
                "LIST_COLUMN_LABEL": label
            }
        },
        function (result)
        {
            if (result.error())
                console.error(result.error());
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

    $id = $_REQUEST['id']; // Assuming ID is passed as a request parameter
    $label = $_REQUEST['label']; // Assuming label is passed as a request parameter

    $result = CRest::call(
        'crm.invoice.userfield.update',
        [
            'id' => $id,
            'fields' => [
                'EDIT_FORM_LABEL' => $label,
                'LIST_COLUMN_LABEL' => $label
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}