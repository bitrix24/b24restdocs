# Create a New Custom Field for Companies crm.company.userfield.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing
- success response is absent
- error response is absent
- links to pages that have not yet been created are not specified (crm.userfield.fields)

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.userfield.add` creates a new custom field for companies.

The system limit for the field name is 20 characters. The custom field name always has the prefix UF_CRM_, meaning the actual length of the name is 13 characters.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`array`](../../../data-types.md) | Set of fields - an array in the form array("field"=>"value"[, ...]), containing the description of the custom field. ||
|| **LIST**
[`unknown`](../../../data-types.md) | Contains a set of list values for custom fields of type List. Specified when creating/updating the field. Each value is an array with the fields: 
- **VALUE** - the value of the list item. This field is required when creating a new item. 
- **SORT** - sorting.
- **DEF** - if equal to Y, the list item is the default value. For multiple fields, multiple DEF=Y are allowed. For non-multiple fields, the first one will be considered default.
- **XML_ID** - external code of the value. This parameter is only considered when updating already existing list item values.
- **ID** - identifier of the value. If specified, it is considered an update of an existing list item value, not the creation of a new one. This is only relevant when calling the `*.userfield.update` methods.
- **DEL** - if equal to Y, the existing list item will be deleted. This is applied if the ID parameter is filled. ||
|#

Full field descriptions can be obtained by calling the `crm.userfield.fields` method.

## Examples

**Example #1: Creating a Text Field**

```js
BX24.callMethod(
    "crm.company.userfield.add",
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

**Example #2: Creating a List**

```js
BX24.callMethod(
    "crm.company.userfield.add",
    {
        fields:
        {
            "FIELD_NAME": "MY_LIST",
            "EDIT_FORM_LABEL": "My List",
            "LIST_COLUMN_LABEL": "My List",
            "USER_TYPE_ID": "enumeration",
            "LIST": [
                { "VALUE": "Item #1" },
                { "VALUE": "Item #2" },
                { "VALUE": "Item #3" },
                { "VALUE": "Item #4" },
                { "VALUE": "Item #5" }
                ],
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