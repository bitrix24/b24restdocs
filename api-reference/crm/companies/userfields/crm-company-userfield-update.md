# Update Existing Custom Field for Companies crm.company.userfield.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing
- success response is absent
- error response is absent
- links to yet-to-be-created pages (crm.userfield.fields) are not provided

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.userfield.update` updates an existing custom field for companies.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Identifier of the custom field. ||
|| **fields**
[`unknown`](../../../data-types.md) | Set of fields - an array in the form array("field_to_update"=>"value"[, ...]), where "field_to_update" can take values returned by the method `crm.userfield.fields`. ||
|| **LIST**
[`unknown`](../../../data-types.md) | Contains a set of list values for custom fields of type List. Specified when creating/updating the field. Each value is an array with the following fields:
- **VALUE** - value of the list item. This field is required when creating a new item.
- **SORT** - sorting.
- **DEF** - if equal to `Y`, the list item is the default value. For multiple fields, multiple `DEF=Y` are allowed. For non-multiple fields, the first one will be considered default.
- **XML_ID** - external code of the value. This parameter is considered only when updating already existing values of the list item.
- **ID** - identifier of the value. If specified, it is considered an update of an existing list item value, not the creation of a new one. It only makes sense when calling `*.userfield.update` methods.
- **DEL** - if equal to `Y`, the existing list item will be deleted. This applies if the `ID` parameter is filled. ||
|#

## Example

```js
var id = prompt("Enter ID");
var label = prompt("Enter new name");
BX24.callMethod(
    "crm.company.userfield.update",
    {
        id: id,
        fields:
        {
            "EDIT_FORM_LABEL": label,
            "LIST_COLUMN_LABEL": label
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
        {
            console.dir(result.data());             
            if(result.more())
                result.next();                        
        }
    }
);
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}