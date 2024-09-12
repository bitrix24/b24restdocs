# Add Field crm.lead.userfield.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- edits needed to meet writing standards
- parameter requirements are not specified
- examples are missing (in other languages)
- success response is missing
- error response is missing
- link to the yet-to-be-created page is not provided

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.userfield.add` creates a new custom field for leads.

The system limitation on the field name is 20 characters. The custom field name is always prefixed with `UF_CRM_`, meaning the actual length of the name is 13 characters.

#|
|| **Parameter** | **Description** ||
|| **fields** | A set of fields – an array of the form `array("field"=>"value"[, ...])`, containing the description of the custom field. ||
|| **LIST** | Contains a set of list values for custom fields of type List. Specified when creating/updating the field. Each value is an array with the following fields: 
- **VALUE** - the value of the list item. This field is required when creating a new item.
- **SORT** - sorting. 
- **DEF** - if equal to `Y`, the list item is the default value. For multiple fields, several `DEF=Y` are allowed. For non-multiple fields, the first will be considered default. 
- **XML_ID** - external code of the value. This parameter is only considered when updating already existing list item values. 
- **ID** - identifier of the value. If specified, it is considered an update of an existing list item value, not the creation of a new one. It only makes sense when calling the `*.userfield.update` methods. 
- **DEL** - if equal to Y, the existing list item will be deleted. This applies if the ID parameter is filled. ||
|#

A complete description of the fields can be obtained by calling the method [crm.userfield.fields](.).

## Examples

```js
BX24.callMethod(
    "crm.lead.userfield.add",
    {
        fields:
        {
            "FIELD_NAME": "MY_STRING",
            "EDIT_FORM_LABEL": "My String",
            "LIST_COLUMN_LABEL": "My String",
            "USER_TYPE_ID": "string",
            "XML_ID": "MY_STRING",
            "SETTINGS": { "DEFAULT_VALUE": "Hello, World!" }
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
```

```js
BX24.callMethod(
    "crm.lead.userfield.add",
    {
        fields:
        {
            "FIELD_NAME": "MY_LIST",
            "EDIT_FORM_LABEL": "My List",
            "LIST_COLUMN_LABEL": "My List",
            "USER_TYPE_ID": "enumeration",
            "LIST": [ { "VALUE": "Item #1" }, { "VALUE": "Item #2" }, { "VALUE": "Item #3" }, { "VALUE": "Item #4" }, { "VALUE": "Item #5" } ],
            "XML_ID": "MY_LIST",
            "SETTINGS": { "LIST_HEIGHT": 3 }
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);    
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}